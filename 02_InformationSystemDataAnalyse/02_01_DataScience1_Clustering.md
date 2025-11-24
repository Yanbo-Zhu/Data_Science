


# 1 k-Means Clustering


**K-Means** 就是将数据自动分成 **K 类**，每一类都有一个“中心”，并把每个数据点分配到离它最近的中心所代表的类中。
- **选择 K 个初始中心点**（随机从数据中挑选 K 个）
- **将所有点分配到最近的中心**（根据欧几里得距离等）
- **更新每个簇的中心点**（重新计算每类的平均值作为新的中心）    
- **重复步骤 2 和 3**，直到收敛（即簇分配不再变化或达到最大迭代次数）

- in each iteration  wird all points are considered and to calute the new mean point 
- Stop kriterium:  
    - centriod 每次 iteration 的 变化 都很小于一个预定的值 
    - Durschnitteliche Summer der quadrieteren Abstaende zum nachen centriod  小于一定值  (summe der abstande von jede points zu seinem zugehorige Cnetriods). 得到这时候的 iteration number  就是 这个 k-werte 的 representant . 就是说 到  iteration number  了, mittelpoint 就稳定了 
    - 

如何知道 k 的优值: 通过   k-werte to  zahl der durchschitte ausgefuhrt iteration 的取消
如何确定 最优的 centriod 值: stop kritium 


In den letzten beiden Vorlesungsthemen setzen wir uns mit Data Science auseinander. Data Science beruht auf Maschinellem Lernen, welches sich grob in drei Klassen unterteilen lässt: Unsupervised Learning, Supervised Learning und Reinforcement Learning. Wir beschäftigen uns in diesem Tutorium spezifisch mit Unsupervised Learning, ehe wir uns in der kommenden Woche mit Supervised Learning auseinandersetzen.

Das Tutorium in dieser Woche besteht aus zwei Jupyter Notebooks. In diesem Notebook behandeln wir zuerst k-Means Clustering, ehe wir uns im zweiten Notebook mit Hierarchischen Clustering beschäftigen. Wir empfehlen Ihnen, die Notebooks Aufgabe für Aufgabe durchzuarbeiten, da viele Aufgaben auf vorherigen Aufgaben aufbauen.

In den Data Science Tutorien benutzen wir verstärkt externe Bibliotheken für die Implementierung der Inhalte. Wir empfehlen Ihnen, sich mit diesen Bibliotheken (insbeonsdere Numpy, Pandas, scikit-learn und matplotlib) auseinanderzusetzen. Das ist allerdings keine Voraussetzung für die Prüfungsleistungen in ISDA.

Hinweis: Aufgaben, die durch einen Asterisk (*) markiert sind, sind Bonusaufgaben. Diese Aufgaben können im Tutorium behandelt werden, dies ist jedoch von den Übungsleitern nicht geplant.



```python
import copy

import matplotlib.pyplot as plt
import numpy as np
import pandas as pd
from sklearn.cluster import KMeans

# Matplotlib parameters
plt.rcParams["figure.dpi"] = 150
plt.rcParams["figure.figsize"] = [4, 3]
plt.rcParams["font.size"] = 8
```




## 1.1 Aufgabe 1: k-Means von Hand










# 2 Hierarchisches Clustering


**Hierarchisches Clustering**（层次聚类）是一种**无监督机器学习方法**，用于将数据**逐步聚合或分裂成层次结构的簇（Cluster）**。与 K-Means 不同，它**不需要提前指定聚类的数量 K**，而是生成一个**树状图（Dendrogram）**，可以根据需要随时“剪断”获得任意数量的簇。


Hierarchisches Clustering 就像你把数据点**一层一层地组合成“树”状的族谱**，你可以选择在哪一层“剪断”来得到你想要的分组数。


|方法类型|说明|
|---|---|
|🔽 **Agglomerative**（自底向上）|一开始每个点是一个簇，逐步合并最近的两个簇，直到所有点在一个簇中|
|🔼 **Divisive**（自顶向下）|所有点开始在同一个大簇中，逐步拆分成更小的簇，直到每个点单独成簇|



过程（以 Agglomerative 为例）：

1. 初始化：每个样本都是一个簇
    
2. 计算所有簇之间的距离（通常是两个簇中样本点之间的最短/最长/平均距离）
    
3. 合并距离最近的两个簇
    
4. 重复步骤 2-3，直到只剩一个大簇（或达到你想要的簇数）





# 3 

k mean 适合于 点都都集中再一次
hierachische clustering 适合于 点都连在一次, 不管 连在一次行程什么形状 



k-mean 快于 hierachische clustering,, 因为 k-mean  可以设定 最终我们想到几个 cluster,  不像 hierachische clustering, 不设置 最终的 cluster 的数量,  都要最后 合称为一个 cluster 