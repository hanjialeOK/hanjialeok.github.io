# Approximately optimal approximate reinforcement learning

[https://people.eecs.berkeley.edu/~pabbeel/cs287-fa09/readings/KakadeLangford-icml2002.pdf](https://people.eecs.berkeley.edu/~pabbeel/cs287-fa09/readings/KakadeLangford-icml2002.pdf)

这篇文章给出了策略改进的下界

## 指出问题

当时的贪婪动态规划和策略梯度方法虽然取得成功，但是存在问题

- 是否存在一个性能指标在每步更新时，都能有保证的提升？
- 证明一个特定的更新提升了性能有多困难？
- 经过一定次数的策略更新后，可以获得什么样的性能？

## 公式回顾

首先回顾一下之前提到的，智能体的目标（objective）

$$\rho(\pi) = V^\pi(s_0) \qquad s_0\ 是初始状态$$

状态分布

$$d^\pi(s)=\sum_{t=0}^\infty \gamma^t Pr \left\{ s_t=s\ |\ s_0,\pi \right\}$$

很明显，下面的等式也成立，通过简单的调整连加符号的顺序即可

$$\begin{aligned}
\sum_s d^\pi(s) &= \sum_s \sum_{t=0}^\infty \gamma^t Pr \left\{ s_t=s\ |\ s_0,\pi \right\} \\
&= \sum_{t=0}^\infty \gamma^t \sum_s Pr \left\{ s_t=s\ |\ s_0,\pi \right\} \\
\end{aligned}$$

策略梯度

$$\begin{aligned}
\nabla_\theta \rho(\pi) &= \sum_s d^\pi(s) \sum_a \nabla_\theta \pi(s,a)Q^\pi(s,a) = \mathbb{E}_{s \sim d^\pi(s)} \Bigg[ \mathbb{E}_{a \sim \pi(s,a)} \left[ \frac{\nabla_\theta \pi(s,a)}{\pi(s,a)} Q^\pi(s,a) \right] \Bigg] \\
&= \sum_s d^\pi(s) \sum_a \nabla_\theta \pi(s,a)Q^\pi(s,a) - \sum_s d^\pi(s) \sum_a \nabla_\theta \pi(s,a)V^\pi(s) \\
&= \sum_s d^\pi(s) \sum_a \nabla_\theta \pi(s,a) \big( Q^\pi(s,a) - V^\pi(s) \big) \\
&= \sum_s d^\pi(s) \sum_a \nabla_\theta \pi(s,a)A^\pi(s,a) = \mathbb{E}_{s \sim d^\pi(s)} \Bigg[ \mathbb{E}_{a \sim \pi(s,a)} \left[ \frac{\nabla_\theta \pi(s,a)}{\pi(s,a)} A^\pi(s,a) \right] \Bigg] \\
\end{aligned}$$

上式第二行，等号右边第二项的值为零，因为

$$\begin{aligned}
\sum_a \nabla_\theta \pi(s,a)V^\pi(s) = V^\pi(s) \nabla_\theta \sum_a \pi(s,a) = V^\pi(s) \nabla_\theta 1 = 0
\end{aligned}$$

## 策略优势

文章中定义了一个保守更新规则

$$\pi_{new}(s,a)=(1-\alpha)\pi(s,a)+\alpha \pi'(s,a), \quad \alpha \in [0,1].$$

对于当前策略 $\pi$ 和单步更新后 $\pi'$，以及初始状态分布 $\mu$，定义策略优势（policy advantage）

$$\mathbb{A}_{\pi,\mu}(\pi') \equiv E_{s \sim d_{\pi, \mu}} \left[ E_{a \sim \pi'(s,a)}[A_\pi(s,a)] \right]$$

由于文章中的提出的初始状态分布 $\mu$ 对后来工作影响不大，所以可以忽略，从而简写为

$$\mathbb{A}_\pi(\pi') := \sum_s d^\pi(s) \sum_a \pi'(s,a)A^\pi(s,a)$$

定义策略优势是为了，在使用 $\pi$ 获得的一系列状态之上，衡量 $\pi'$ 选择较大优势的动作的程度。

下面是对 $\frac{\partial \eta_\mu}{\partial \alpha} \Big|_{\alpha=0}=\frac{1}{1-\gamma} \mathbb{A}_{\pi,\mu}$ 的证明（形式不同是因为省略了 $\mu$ 以及 $1-\gamma$ 的原因，$\eta_\mu$ 对应的是 $\rho(\pi_{new})$）

$$\begin{aligned}
\nabla_\alpha \rho(\pi_{new}) \Big| _{\alpha=0} &= \sum_s d^{\pi_{new}}(s) \sum_a \nabla_\alpha \pi_{new}(s,a) A^{\pi_{new}}(s,a) \Big| _{\alpha=0} \\
&= \sum_s d^{\pi_{new}}(s) \sum_a \big(\pi'(s,a)-\pi(s,a)\big) A^{\pi_{new}}(s,a) \Big| _{\alpha=0} \\
&= \sum_s d^\pi(s) \sum_a \big(\pi'(s,a)-\pi(s,a)\big) A^\pi(s,a) \\
&= \sum_s d^\pi(s) \sum_a \pi'(s,a) A^\pi(s,a) - \sum_s d^\pi(s) \sum_a \pi(s,a) A^\pi(s,a) \\
&= \sum_s d^\pi(s) \sum_a \pi'(s,a) A^\pi(s,a) \\
&= \mathbb{A}_\pi(\pi') \\
\end{aligned}$$

注意到上式等号右边的第二项值为零，因为

$$\begin{aligned}
\sum_s d^\pi(s) \sum_a \pi(s,a) A^\pi(s,a) &= \sum_s d^\pi(s) \sum_a \pi(s,a) \big( Q^\pi(s,a) - V^\pi(s) \big) \\
&= \sum_s d^\pi(s) \sum_a \pi(s,a) Q^\pi(s,a) - \sum_s d^\pi(s) V^\pi(s) \sum_a \pi(s,a) \\
&= \sum_s d^\pi(s) V^\pi(s) - \sum_s d^\pi(s) V^\pi(s) \cdot 1 \\
&= 0 \\
\end{aligned}$$

同理，不难看出

$$\begin{aligned}
\mathbb{A}_\pi(\pi_{new}) &= \sum_s d^\pi(s) \sum_a \pi_{new}(s,a)A^\pi(s,a) \\
&= \sum_s d^\pi(s) \sum_a \big( (1-\alpha)\pi(s,a)+\alpha \pi'(s,a) \big) A^\pi(s,a) \\
&= \alpha \sum_s d^\pi(s) \sum_a \pi'(s,a) A^\pi(s,a) \\
&= \alpha \mathbb{A}_\pi(\pi') \\
\end{aligned}$$

对于任意两种策略 $\tilde\pi$ 和 $\pi$，有

$$\rho(\tilde\pi) - \rho(\pi)=\sum_s d^{\tilde\pi}(s) \sum_a \tilde\pi(s,a) A^\pi(s,a)$$

$Proof.$

$$\begin{aligned}
\rho(\tilde\pi)&=\mathbb{E}_{s_0,a_0, \ldots s_t,a_t \sim \tilde\pi} \left[ \sum_{t=0}^\infty \gamma^t R(s_t,a_t)\ \Bigg|\ s_0 \right] \\
&=\sum_{t=0}^\infty \gamma^t \mathbb{E}_{s_0,a_0, \ldots s_t,a_t \sim \tilde\pi} \left[ R(s_t,a_t) + V^\pi(s_t) - V^\pi(s_t)\ |\ s_0 \right] \\
&=\sum_{t=0}^\infty \gamma^t \mathbb{E}_{s_0,a_0, \ldots s_t,a_t \sim \tilde\pi, s_{t+1} \sim P(s_{t+1} | s_t, \tilde\pi)} \left[  R(s_t,a_t) + \gamma V^\pi(s_{t+1}) - V^\pi(s_t)\ |\ s_0 \right] + V^\pi(s_0) \\
&=\sum_{t=0}^\infty \gamma^t \mathbb{E}_{s_0,a_0, \ldots s_t,a_t \sim \tilde\pi} \left[ Q^\pi(s_t,a_t) - V^\pi(s_t)\ |\ s_0 \right] + V^\pi(s_0) \\
&=\sum_{t=0}^\infty \gamma^t \mathbb{E}_{s_0,a_0, \ldots s_t,a_t \sim \tilde\pi} \left[ A^\pi(s_t,a_t) \right] + V^\pi(s_0) \\
&=\sum_{s} d^{\tilde\pi}(s) \sum_a \tilde\pi(s,a) A^\pi(s_t,a_t) + \rho(\pi) \\
\end{aligned}$$

$$\begin{aligned}
\rho(\tilde\pi) = V^{\tilde\pi}(s_0) &=\mathbb{E} \left[ \sum_{t=0}^\infty \gamma^t R(s_t,a_t)\ \Bigg|\ s_0, \tilde\pi \right] \\
&=\mathbb{E} \left[ \sum_{t=0}^\infty \gamma^t \big( R(s_t,a_t) + V^\pi(s_t) - V^\pi(s_t)\big)\ \Bigg|\ s_0, \tilde\pi \right] \\
&=\mathbb{E} \left[ \sum_{t=0}^\infty \gamma^t \big( R(s_t,a_t) + \gamma V^\pi(s_{t+1}) - V^\pi(s_t)\big)\ \Bigg|\ s_0, \tilde\pi \right] + V^\pi(s_0) \\
&=\mathbb{E} \left[ \sum_{t=0}^\infty \gamma^t \big( Q^\pi(s_t,a_t) - V^\pi(s_t)\big)\ \Bigg|\ s_0, \tilde\pi \right] + V^\pi(s_0) \\
&=\mathbb{E} \left[ \sum_{t=0}^\infty \gamma^t A^\pi(s_t,a_t)\ \Bigg|\ s_0, \tilde\pi \right] + V^\pi(s_0) \\
&=\sum_{s} d^{\tilde\pi}(s) \sum_a \tilde\pi(s,a) A^\pi(s,a) + \rho(\pi) \\
\end{aligned}$$

下面是最重要的部分，证明策略改进下界。不过，要先定义几个量，对于 $\pi_{new}(s,a)=(1-\alpha)\pi(s,a)+\alpha \pi'(s,a)$，用 $c_t$ 表示 $t$ 个时间步内，使用了 $\pi'$ 的次数。

- $1-\rho_t = \Pr \{c_t = 0 \}=(1-\alpha)^t$
- $\rho_t = \Pr \{ c_t \geq 1 \} = 1-(1-\alpha)^t$

$$\begin{aligned}
\rho(\pi_{new}) - \rho(\pi) &= \sum_{s} d^{\pi_{new}}(s) \sum_a \pi_{new}(s,a) A^\pi(s,a) \\
&=\sum_{s} d^{\pi_{new}}(s) \sum_a \pi'(s,a) A^\pi(s,a) \qquad pi_{new}\ 展开 \\
&=\alpha \sum_{t=0}^\infty \gamma^t \sum_s Pr \left\{ s_t=s\ |\ s_0,\pi_{new} \right\} \sum_a \pi'(s,a) A^\pi(s,a) \qquad  d^{\pi_{new}}(s)\ 展开，并且交换连加符号 \\
&=\alpha \sum_{t=0}^\infty \gamma^t \left[ (1-\rho_t) \sum_s Pr \left\{ s_t=s\ |\ s_0,\pi_{new},c_t=0  \right\} \bigg[ \sum_a \pi'(s,a) A^\pi(s,a) \bigg] + \rho_t \sum_s Pr \left\{ s_t=s\ |\ s_0,\pi_{new},c_t \geq 1 \right\} \bigg[ \sum_a \pi'(s,a) A^\pi(s,a) \bigg] \right] \qquad 这里注意\ \rho_t\ 不能穿透到最外面，因为有\ \sum_{t=0}^\infty\\
&=\alpha \sum_{t=0}^\infty \gamma^t \left[ \sum_s Pr \left\{ s_t=s\ |\ s_0,\pi_{new},c_t=0 \right\} \sum_a \pi'(s,a) A^\pi(s,a) - \rho_t \sum_s Pr \left\{ s_t=s\ |\ s_0,\pi_{new},c_t=0 \right\} \sum_a \pi'(s,a) A^\pi(s,a) + \rho_t \sum_s Pr \left\{ s_t=s\ |\ s_0,\pi_{new},c_t \geq 1 \right\} \sum_a \pi'(s,a) A^\pi(s,a) \right] \\
&\geq\alpha \sum_{t=0}^\infty \gamma^t \left[ \sum_s Pr \left\{ s_t=s\ |\ s_0,\pi \right\} \sum_a \pi'(s,a) A^\pi(s,a) - 2 \rho_t \left| \max_{s \sim \pi_{new}} \sum_a \pi'(s,a) A^\pi(s,a) \right| \right] \qquad 令\ \varepsilon = \left| \max_{s \sim \pi_{new}} \sum_a \pi'(s,a) A^\pi(s,a) \right|\\
&=\alpha \sum_{t=0}^\infty \gamma^t \sum_s Pr \left\{ s_t=s\ |\ s_0,\pi \right\} \sum_a \pi'(s,a) A^\pi(s,a) - 2 \alpha \varepsilon \sum_{t=0}^\infty \gamma^t \rho_t \\
&=\sum_{s} d^{\pi}(s) \sum_a \pi'(s,a) A^\pi(s,a) - 2 \alpha \varepsilon \left( \frac{1}{1-\gamma} - \frac{1}{1-\gamma(1-\alpha)} \right)
\end{aligned}$$
