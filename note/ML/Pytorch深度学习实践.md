[B站视频链接](https://www.bilibili.com/video/BV1Y7411d7Ys)

课件下载链接：https://pan.baidu.com/s/1vZ27gKp8Pl-qICn_p2PaSw 
提取码：cxe4

# Overview

## 关于这门课

**如何学习深度神经网络?**

- 类似`组装`

### Pytorch版本

教学的Pytorch版本: 0.4

截止2022.1.24的Pytorch版本: 1.10

- 要求: 学会看文档

- 技术发展过程

<img src="images/Untitled/image-20220124111749138.png" alt="image-20220124111749138" style="zoom:80%;" />

### 教学目标

用Pytorch完成学习系统

理解神经网络和深度学习

### 算法比较: 传统算法与机器学习算法

传统算法

- 穷举
- 贪心
- 分治
- 动态规划

机器学习算法

- 从Data中找出算法

<img src="images/Untitled/image-20220124113145905.png" alt="image-20220124113145905"  />



## 人工智能历史

### 学习系统的设计

![image-20220124114816146](images/Untitled/image-20220124114816146.png)

机器学习中的维度诅咒

- 内容: feature越多, 需要的样本数越多: a^n

- 解决: 降维

表示学习Present: 

- 高维空间在低维空间的流形(Manifold), 学习器--分开学习

深度学习: 

- 端到端

### 传统机器学习策略及其缺点

- 限制于人工选择的特征

- svm不能很好地处理big数据集

- 越来越多的应用程序需要处理非结构化数据

![image-20220124115046489](images/Untitled/image-20220124115046489.png)

## 神经网络历史

神经科学 -> 数学, 工程学

一个对人工智能影响深远的生物学实验

- 分层处理视觉信息
- 浅层 - 线条/颜色
- 深层 - 高级特征(猫/狗...)

<img src="images/Untitled/image-20220124202604306.png" alt="image-20220124202604306" style="zoom:80%;" />

Back Propagation

原理: 链式法则

![image-20220124203310124](images/Untitled/image-20220124203310124.png)

如何学习深度神经网络?

- 类似`组装`

# 线性模型

ML步骤:

- DataSet
- Model
- Training

- Inferring

<img src="images/Untitled/image-20220124210145654.png" alt="image-20220124210145654" style="zoom: 50%;" />

## DataSet

真实集和训练集差异

例如: 猫狗训练集

<img src="images/Untitled/image-20220124210717377.png" alt="image-20220124210717377" style="zoom: 50%;" />

策略: 划分出dev set

<img src="images/Untitled/image-20220124210814615.png" alt="image-20220124210814615" style="zoom:50%;" />

## Model Design

随机猜测w

<img src="images/Pytorch%E6%B7%B1%E5%BA%A6%E5%AD%A6%E4%B9%A0%E5%AE%9E%E8%B7%B5/image-20220126160056186.png" alt="image-20220126160056186" style="zoom: 33%;" />

思路: 让平均损失降到最低

<img src="images/Pytorch%E6%B7%B1%E5%BA%A6%E5%AD%A6%E4%B9%A0%E5%AE%9E%E8%B7%B5/image-20220126151233011.png" alt="image-20220126151233011" style="zoom: 25%;" />

概念: 平均平方误差(mean square error MSE)

<img src="images/Pytorch%E6%B7%B1%E5%BA%A6%E5%AD%A6%E4%B9%A0%E5%AE%9E%E8%B7%B5/image-20220126151410966.png" alt="image-20220126151410966" style="zoom: 25%;" />

思路: 穷举法-采样

## Training

**实时可视化+日志输出+断点重开**

工具: visdom

<img src="images/Pytorch%E6%B7%B1%E5%BA%A6%E5%AD%A6%E4%B9%A0%E5%AE%9E%E8%B7%B5/image-20220126152045175.png" alt="image-20220126152045175" style="zoom: 50%;" />

```python
import numpy as np
import matplotlib.pyplot as plt
#%%
x_data = [1.0,2.0,3.0]
y_data = [2.02,3.98,6.01]
#%%
def forward(x):
    return x*w
#%%
def loss(x,y):
    y_pred = forward(x)
    return (y_pred - y) * (y_pred - y)
#%%
w_list = []
mse_list = []
for w in np.arange(0.0,4.1,0.1):
    print("w = ",w)
    l_sum = 0
    for x_val,y_val in zip(x_data,y_data):
        y_pred_val = forward(x_val)
        loss_val = loss(x_val,y_val)
        l_sum += loss_val
        print('\t',x_val,y_val,y_pred_val,loss_val)
    print("MSE = ",l_sum/3)
    w_list.append(w)
    mse_list.append(l_sum/3)
```

# 梯度下降算法

## 思想

穷举的不可行, 指数级增加

<img src="images/Pytorch%E6%B7%B1%E5%BA%A6%E5%AD%A6%E4%B9%A0%E5%AE%9E%E8%B7%B5/image-20220126160302841.png" alt="image-20220126160302841" style="zoom: 33%;" />

采用分治? 不可以

- Loss并不是凸函数
- w超大时, 计算量仍旧很大

<img src="images/Pytorch%E6%B7%B1%E5%BA%A6%E5%AD%A6%E4%B9%A0%E5%AE%9E%E8%B7%B5/image-20220126160649572.png" alt="image-20220126160649572" style="zoom: 33%;" />

**本质**: 优化问题

**思想**: 贪心

**方法分析:**

- 情况: 非凸函数

- 理论上的缺陷: 得到在局部区域最优的结果/局部最优

  <img src="images/Pytorch%E6%B7%B1%E5%BA%A6%E5%AD%A6%E4%B9%A0%E5%AE%9E%E8%B7%B5/image-20220126161700568.png" alt="image-20220126161700568" style="zoom:33%;" />

- 实际情况: 很难陷入局部最优点

- 实际情况: 可能到"鞍点"

  <img src="images/Pytorch%E6%B7%B1%E5%BA%A6%E5%AD%A6%E4%B9%A0%E5%AE%9E%E8%B7%B5/image-20220126161633283.png" alt="image-20220126161633283" style="zoom:33%;" />

**计算方法:**

​	<img src="images/Pytorch%E6%B7%B1%E5%BA%A6%E5%AD%A6%E4%B9%A0%E5%AE%9E%E8%B7%B5/image-20220126161936820.png" alt="image-20220126161936820" style="zoom: 33%;" />

**代码实现:**

**绘图:**

- 指数加权均值

  <img src="images/Pytorch%E6%B7%B1%E5%BA%A6%E5%AD%A6%E4%B9%A0%E5%AE%9E%E8%B7%B5/image-20220126162742387.png" alt="image-20220126162742387" style="zoom: 33%;" />

- 训练失败的一种情况: 可能是学习率过高

  <img src="images/Pytorch%E6%B7%B1%E5%BA%A6%E5%AD%A6%E4%B9%A0%E5%AE%9E%E8%B7%B5/image-20220126163051294.png" alt="image-20220126163051294" style="zoom:33%;" />

## 随机梯度下降

原因: 

- 一个样本的随机性, 很可能跳过`鞍点`

<img src="images/Pytorch%E6%B7%B1%E5%BA%A6%E5%AD%A6%E4%B9%A0%E5%AE%9E%E8%B7%B5/image-20220126163300843.png" alt="image-20220126163300843" style="zoom:33%;" />

算法区别:

- `SGD`: 使用一个样本的Loss计算偏导数
- `GD`:使用全部样本的Cost计算偏导数

<img src="images/Pytorch%E6%B7%B1%E5%BA%A6%E5%AD%A6%E4%B9%A0%E5%AE%9E%E8%B7%B5/image-20220126163421667.png" alt="image-20220126163421667" style="zoom: 50%;" />

缺点: 

- SGD计算函数值不能并行, w是有依赖

<img src="images/Pytorch%E6%B7%B1%E5%BA%A6%E5%AD%A6%E4%B9%A0%E5%AE%9E%E8%B7%B5/image-20220126163717129.png" alt="image-20220126163717129" style="zoom:33%;" />

- 折中: batch

<img src="images/Pytorch%E6%B7%B1%E5%BA%A6%E5%AD%A6%E4%B9%A0%E5%AE%9E%E8%B7%B5/image-20220126163828507.png" alt="image-20220126163828507" style="zoom:33%;" />

