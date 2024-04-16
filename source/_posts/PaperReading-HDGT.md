---
title: PaperReading:HDGT
date: 2024-03-09 20:15:01
tags:
- [Deep Learning]
- [Paper Reading]
- [Trajectory Prediction]
categories: 
- [Python]
- [Pytorch]
mathjax: true
typora-root-url: ..
---

文章源地址：[HDGT: Heterogeneous Driving Graph Transformer for Multi-Agent Trajectory Prediction via Scene Encoding](https://ieeexplore.ieee.org/document/10192373)

Github源代码：[HDGT: Heterogeneous Driving Graph Transformer for Multi-Agent Trajectory Prediction via Scene Encoding](https://github.com/OpenDriveLab/HDGT)

### Introduction

对于自动驾驶领域的轨迹预测下游任务而言，目前的主流方案是对于场景中的非结构化数据进行向量化处理，然后运用深度学习的方法来进行学习与训练。然而目前为止还没有一个很好的编码方案来对这些非结构化的数据进行建模。本文提出了一种基于异构图的方法进行建模，对于每一个节点，都建立了一个以自己为中心的相对坐标系，使其能够在自己的本地坐标系下处理其他对象的相对时空状态信息，同时保持了所有节点和边的对称性，达到同时进行多个agent预测的目的。

### Related Work

#### Heterogeneous Graph Neural Networks

异构图神经网络被用在轨迹预测方向，主要体现在在一个交通场景中，图中的节点以及节点之间的相对关系可能并不是唯一的。例如可能有lane-agent，agent-agent，lane-lane等等多个不同的关系。这里可以使用异构图来很好的进行建模，保留这些相对关系。

<!--more-->

#### Transformer

传统的transformer通常由两个核心组成：attention-based模块以及feed-forward模块。在之前的轨迹预测任务中，通常都是使用一组共享的参数来进行地图上元素信息的传递和融合，这样忽略了各个元素之间的异构性。在本文中通过它们的语义关系连接节点，并使用类型特定的参数进行节点和边嵌入的聚合和更新。此外，所有节点都在它们的本地坐标系中处理信息。

####  Scene Encoder For Trajectory Prediction

在轨迹预测任务中，对于场景的建模是十分重要的。在现在的工作中，不仅需要考虑agent之间的作用，同时还要考虑地图元素（车道、交通信号灯等）对于行车轨迹的影响。现在通常使用向量化的方式来编码地图元素，例如用LSTM、1D CNN等来处理时间数据，用PointNet来处理折线，用GNN编码元素之间的关系。

#### Output Decoder for Trajectory Prediction

过去的研究中有很多对于编码后的数据的解码进行了研究，在此不多赘述。

本文主要研究的是如何高质量的使用异构图对于地图上的元素进行编码，即encoder部分。

### Methodology

![](/pic/PaperReading-HDGT/pipeline.png)

轨迹预测的输入可以整体分为两个模块：agent过去时间的状态和现在的地图元素。最终需要得到的应该是在后面T个时间帧内，每一个agent可能的轨迹以及相应的置信分数。

#### Construction of the Heterogeneous Graph

根据异构图的概念，对于一个交通场景中的元素，根据其类型的不同进行相应的建模处理。在HDGT中，一条边 $e: u → v$ 是由起始节点$u$和终点节点$v$以及它们之间的关系共同决定的。

对于agent，计算最后的时间帧上它和其它元素的距离；对于折线形状的元素，取中点为其坐标；对于多边形形状的元素，取几何中心为其坐标。对于车道节点，将它们与周围的车道节点连接，并且有左、右、入口和出口四种类型。针对车道长度差异较大的问题，将长度大于阈值的车道划分为长度相似的短车道，以确保所有车道节点具有类似的输入。通过这样的建模可以构建需要的节点集和邻接矩阵。

下面是原论文中的伪代码表示：

![](/pic/PaperReading-HDGT/code.png)

#### Relative Spatial Relation Encoding

1. **Global View to Egocentric View**

   在这一部分，对于每一个节点元素，文章设定了一个以他们自己为中心的相对坐标系，记作 $(x_{ref},y_{ref},\theta_{ref})$ 。对于节点的坐标，首先，都进行$(x,y)-(x_{ref},y_{ref})$的操作，来保证是处于自己的相对坐标系中；其次，设定一个旋转矩阵$R=\begin{bmatrix}
     cos-\theta_{ref}&-sin-\theta_{ref} \\
     sin-\theta_{ref}&cos-\theta_{ref}
   \end{bmatrix}$，用这个旋转矩阵和位置坐标${(x,y)}^T$和速度${(velocity_{x},velocity_y)}^T$做点积，得到相应的相对坐标以及速度。对于不同的地图元素，选取的相对坐标系的原点$(x_{ref},y_{ref})$也会不同。

2. **Node Encoding**

   上面经过处理后的地图元素已经处在自己的相对坐标系下，但是仍然是非结构化的。因此，需要将元素进行向量化的编码。在这篇论文中，对于所有的agent元素，使用1D CNN网络来进行向量化处理；对于所有的地图元素，使用简化的PointNet来进行处理。对于子类信息，例如车道单行/双行，道路线类型虚线/实线等，采用可学习的嵌入进行编码，然后将它们与从1D CNN或PointNet获得的几何特征进行连接。

3. **Edge Encoding**

   对于一条边 $e: u → v$，定义边的特征是$u$在$v$的坐标系下的特征。对于$u,v$而言，它们分别的对应的自己的相对坐标系表示是：$(x^u_{ref},y^u_{ref},\theta^u_{ref})$  , $(x^v_{ref},y^v_{ref},\theta^v_{ref})$ 。因此，需要的信息是： $\Delta Pose_{u→v} =  (x^v_{ref} − x^u_{ref}, y^v_{ref} − y^u_{ref}, cos(θ^v_{ref} − θ^u_{ref}),sin(θ^v_{ref} − θ^u_{ref})) = (∆x_{u→v}, ∆y_{u→v}, cos∆θ_{u→v},sin ∆θ_{u→v})$ 。用一层MLP来进行边的编码操作： $e^0_{u→v} = MLP(concat[u^0, \Delta Pose_{u→v}])$ 。

#### Heterogeneous Driving Graph Transformer Layer

在HDGT中有两种方式聚合函数：$\rho^V$应用在节点上，聚合每个节点的入边的特征；$\rho^\xi$应用在边上，聚合这个边的源节点的信息。

1. **Node Aggregation and Update**

   由于transformer之前是用来做NLP的任务，直接用来做轨迹预测任务效果必然不佳。同时，一个节点的入边特征类型可能也有很多种，因此，文章使用了一种分层的transformer的形式。的将节点表示为Q，入边的特征表示为K，V，这样就可以运用相应的注意力机制。最后把不同类型节点得到的结果concat起来即可。

2. **Edge Aggregation and Update**

   同理，对于边而言，也分为不同的类型。因此，文章也对其做了分类的处理。对于边信息的更新采用了这样方式：$e^k_{u→v} = MLP_{type(u→v)} (concat(u^{k−1},\Delta Pose_{u→v}, e^{k−1}_{u→v}))$。

#### Output Head and Training Objective

采用回归MLP和分类MLP组合的模式进行损失的计算。对每一个agent而言，将HDGT的最后一层的输出作为隐藏表示。一个MLP模块用来做回归（张量大小：N\*K\*T*2），一个MLP模块用来为每一个轨迹生成置信度（张量大小：N\*K）。

### Experiments

#### Metric

用来评估的标准和很多轨迹预测的标准一样，是用了`minADE(Minimum Average Displacement Error)`，`minFDE(Minimum Final Displacement Error)`，`MR(Missing Rate)`三个标准。

#### Datasets

用了两个dataset来做evaluation：`INTERACTION Dataset`和 `Waymo Open Motion Dataset` 

#### Results

INTERACTION Dataset：

![](/pic/PaperReading-HDGT/interaction.png)



Waymo Open Motion Dataset：

![](/pic/PaperReading-HDGT/waymo.png)

后续文章还有一些消融实验，可视化等等，这里便不在赘述，可以自行查看原文。

### Conclusion

文章提出了一种基于异构图的方法来提取更好的轨迹预测表示。它将驾驶场景建模为一个异构图，并将Transformer作为图聚合和更新函数。当进行聚合时，空间特征被归一化到局部坐标系中。所提出的方法在两个最近的大规模竞争性基准测试中取得了最先进的结果。消融研究验证了文章中每个模块的有效性。
