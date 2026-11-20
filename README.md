# Jiangtherapee-Raw-DeNoise-World-Best

<img width="1913" height="1042" alt="b586e94a8bf83c7f9b79fdc62e7614d1" src="https://github.com/user-attachments/assets/17f3ee90-c309-41f9-a126-3e831acb377e" />

非常感谢大家支持我更多的训练以及后续更震撼的开源项目：

（希望下面二维码折叠，并给一个小二维码的图标和展开提示）

<img width="518" height="575" alt="image" src="https://github.com/user-attachments/assets/0c3a099a-ee9a-43b3-9334-dbdf55dadd3d" />

大家好，我是姜尧耕。今天我向大家介绍我完全开源的、完全超越DXO以及ps自带降噪的降噪网络JRDN，而且提供了极度易用的软件前端。这个视频我会分成两块，第一块我来介绍我做的三点工作以及在同时做一些对比，第二块来和大家讲用法。

2022年，NafNet去掉了传统网络中的部分激活堆叠，证明了低复杂度的非线性恢复网络也能取得很强的去噪和复原效果以及更容易地保持弱信号。JDNDM把问题推进到联合去马赛克和去噪。在此之后，三星做了更多工程上的工作，他加入了不同Bayer模式的支持，加入了RCAN风格的残差结构，并把观测到的噪声标准差作为输入条件。

在这些的基础上，我在本项目也就是JRDN中把输入头完全更换成了非常具有我个人特色的局部幅度场来保证齐次性让网络支持高动态范围图片的处理。具体来讲，一片一定亮度的纹理我把它压暗两档，然后我们输入网络，最后再提亮回来，我要求完全无损。所以我构造了多尺度的幅度场，从观测的局部能量中提取随信号同比例变化的幅度，让网络在除去幅度后的表示上恢复细节，再将结果乘回幅度。这样明暗区域可以共享恢复规则了，也不会产生任何意义上的暗部独有的偏色，网络的眼中也就不会有明暗之分了。我还加入了带有多尺度幅度场选择的可学习的空间幅度场估计器（JRDN-CNN）以增强高噪声下的幅度场计算稳定性表现，在论文中我会做具体消融。

<img width="1594" height="762" alt="image" src="https://github.com/user-attachments/assets/d0ea7e0b-fc2f-4827-ad19-0723cf29dce4" />

第二点，也就是raw本身数据导致的暗部偏色的改善。我在一年多前IIARPTC一篇文章中把raw原生的暗部偏色严格归因到了黑电平的偏差，并推算了在不同的涂料及响应情况下的偏色感知尺度。

<img width="1818" height="528" alt="image" src="https://github.com/user-attachments/assets/bec7164e-20f5-4438-8f5b-3316ee73f312" />

因此我在JRDN的训练中加入了同样的退化，并测算了不同机身的黑电平偏移，以及提供了一个黑电平矫正网络（https://github.com/y-g-jiang/Video-RAW-Black-Level-Correction-Net）。如此而来raw暗部偏色就得到了第二重的抑制。注意这个对比图我已经做了亮度对齐，而暗部又暗又绿正是黑电平的问题所致。

<img width="1592" height="746" alt="image" src="https://github.com/user-attachments/assets/dd783c84-f365-4e76-b672-c46c6bc7a7b3" />

<img width="1913" height="1042" alt="image" src="https://github.com/user-attachments/assets/ef3bbe88-6bc4-4f25-b92d-c8655cd35db4" />


第三点，我还以极完善的尤其是动物的数据集以及感知损失权重和光学退化来让网络学习到镜头本身的缺陷并复原到完美光学的情形。我合理地分配了不同的计算量，并先以保真度，后引入了感知对抗训练的办法来提升了细节表现能力，这也在上面这个对比中得到了展示。不过这个模型我不推荐大家用来进行星空降噪，他对准确数据输入的要求太高，因此我近期正在进行针对性特训。

如果大家发现jrdn的一些问题，那么非常欢迎你们来建言献策，contribute to this repo（展示图、箭头指向https://github.com/y-g-jiang/Jiangtherapee-Raw-DeNoise-World-Best）。希望以后提到降噪，不要有人再去问人要哪里有dxo的学习版，而是让JRDN这样世界一流的降噪算法进入他们自己的软件中。
