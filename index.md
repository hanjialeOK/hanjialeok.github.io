<center>
    <span>
        <img src="./avatar.jpg" width="96px" style="width:100px; height:100px; border-radius:50%;">
    </span>
    <h1>韩佳乐</h1>
    <div>
        <span>
            邮箱:
            hanjiale@mail.ustc.edu.cn
        </span>
        ·
        <span>
            手机:
            15155101720
        </span>
    </div>
</center>

## 教育背景

- 中国科学技术大学 - 软件工程，强化学习 （研究生） 2021 - 至今
- 中国科学技术大学 - 计算机科学与技术 （本科） 2016 - 2021

## 项目

### 强化学习框架（2021.10 – 2022.01）

_实验室项目 Python_

使用 actor-learner 架构搭建的强化学习框架。可以在短时间内收集大量数据，缓解强化学习中的高方差问题，从而大幅提高强化算法的性能。

- 使用多 actor 多 learner 架构，多线程并行收集数据，充分利用 CPU 和 GPU 资源。
- actor 和 learner 之间使用 ZeroMQ 进行通讯，actor 上传采样数据，learner 分发更新后的参数。
- learner 可以使用多 GPU 快速进行梯度下降。

### FFmpeg 中嵌入 YOLOv3 检测算法（2021.01 – 2021.05）

_本科毕设 C++_

FFmpeg 是一款开源视频解码器和播放器。YOLO 是经典一阶检测器。在 Linux 环境下，我负责把用 LibTorch 写的 YOLOv3 算法嵌入到播放器 ffplay 源码中以实现实时检测。这是[项目地址](https://github.com/hanjialeOK/YOLOv3-in-FFmpeg)。

- 性能上，可以播放多种类型视频文件，借助 1080Ti GPU 加速，帧率稳定达到 30 fps。
- 技术上，不仅支持 YOLO 算法，其他图像处理神经网络模型均可采用该方法嵌入到 ffplay 中。
- 采用 makefile 编写管理项目。

### QT 实现音乐播放器（2019.06 – 2019.1）

_个人项目 C++_

使用 QT 模仿网易云桌面客户端，实现了整体模块设计和播放功能。这是[项目地址](https://github.com/hanjialeOK/Qt-musicPlayer-unfinshed)。

- 整体结构是 MainWidget，由上方标题栏，左侧功能栏，底部播放栏，右侧显示栏组成。
- 派生 QListWidget，实现美观的拖拽样式，拖拽超过边界时支持自动滚动，[查看效果](https://blog.csdn.net/weixin_43742643/article/details/100587922)。

## IT 技能

- 编程语言: 熟悉 C/C++，Python。
- 平台工具: 熟悉 Linux 常用命令，以及 Docker，Git 等开发工具。

## 论文

一篇 CoG 论文在投。改进 PPO 算法，在提高其样本效率的同时保持稳定性。（2022.09 – 2023.03）

- 保存 PPO 的历史轨迹数据，在训练时重复利用，提高样本利用率。
- 使用双重要性系数裁剪来控制历史数据带来的高方差，保证训练的稳定性。

## 其他

- 技术博客: [https://blog.csdn.net/weixin_43742643](https://blog.csdn.net/weixin_43742643)
- GitHub: [https://github.com/hanjialeOK](https://blog.csdn.net/weixin_43742643)
- 语言: 英语 (六级 526)
