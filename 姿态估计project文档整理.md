# 姿态估计project文档整理

## 1-OpenPose: Realtime Multi-Person 2D Pose Estimation using Part Affinity Fields

### A-算法

#### a-实现原理

输入一幅图像，经过卷积网络提取特征，得到一组特征图，然后分成两个岔路，分别使用 CNN网络提取Part Confidence Maps 和 Part Affinity Fields；

得到这两个信息后，我们使用图论中的 Bipartite Matching（偶匹配） 求出Part Association，将同一个人的关节点连接起来，由于PAF自身的矢量性，使得生成的偶匹配很正确，最终合并为一个人的整体骨架；
最后基于PAFs求Multi-Person Parsing—>把Multi-person parsing问题转换成graphs问题—>Hungarian Algorithm(匈牙利算法)

#### b-实现神经网络

![image-20200711233843995](C:\Users\dell\AppData\Roaming\Typora\typora-user-images\image-20200711233843995.png)

阶段一：VGGNet的前10层用于为输入图像创建特征映射

阶段二：使用2分支多阶段CNN，

其中第一分支预测身体部位位置（例如肘部，膝部等）的一组2D置信度图（S）。 如下图所示，给出关键点的置信度图和亲和力图 - 左肩。

![image-20200711233945227](C:\Users\dell\AppData\Roaming\Typora\typora-user-images\image-20200711233945227.png)

第二分支预测一组部分亲和度的2D矢量场（L），其编码部分之间的关联度。 如下图所示，显示颈部和左肩之间的部分亲和力。

![image-20200711234001968](C:\Users\dell\AppData\Roaming\Typora\typora-user-images\image-20200711234001968.png)

阶段三： 通过贪心推理解析置信度和亲和力图，对图像中的所有人生成2D关键点。

### B-性能指标

![image-20200711234527507](C:\Users\dell\AppData\Roaming\Typora\typora-user-images\image-20200711234527507.png)

![image-20200711235032541](C:\Users\dell\AppData\Roaming\Typora\typora-user-images\image-20200711235032541.png)

![image-20200711235049253](C:\Users\dell\AppData\Roaming\Typora\typora-user-images\image-20200711235049253.png)

### C-数据集

MPII human multi-person dataset（which consists of 3844 training and 1758 testing groups of multiple interacting individuals in highly articulated poses with 14 body parts）[链接](http://human-pose.mpi-inf.mpg.de/)

