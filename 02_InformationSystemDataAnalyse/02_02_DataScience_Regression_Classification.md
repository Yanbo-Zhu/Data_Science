

# 1 Regression


什么是回归（Regression）？

**回归**是一种**监督学习（Supervised Learning）**的方法，用来**预测连续数值型的目标变量**。

简单说，就是根据已有的数据（特征和对应的结果），学习出一个函数或模型，然后用这个模型预测新的数据对应的数值。


LinearRegression =  logisticalRegression with 1 Degress 



## 1.1 Modell-Evaluierung


Subjektive, visuelle Evaluierungen sind eine Möglichkeit, Modelle zu evaluieren. Objektive Metriken geben aber oft eine besseres Bild. Im Folgenden werden wir dafür die Metrik [*mean squared error*](https://de.wikipedia.org/wiki/Mittlere_quadratische_Abweichung) (MSE, deutsch: mittlere quadratische Abweichung) verwenden. Diese wird sehr häufig für Regressionsanalysen verwendet. Die Formel für den MSE ist:  
  
$$MSE(\mathbf{y}, \mathbf{\hat y}) = \frac{1}{N} \sum_{i=1}^{N} (y_i - \hat y_i)^2$$  
  
Zur Notation:  
- $\mathbf{y}$ steht für den Vektor der korrekten Zielwerte.  
- $\mathbf{\hat y}$ steht für den Vektor der von einem Modell vorhergesagten Werte.   
- In unserem Code haben wir für $\mathbf{y}$ bisher `y` geschrieben und `y_pred` für $\mathbf{\hat y}$  (sonst ist oft auch `y_hat` üblich).   
- $y_i$ steht für den "i-ten" Eintrag in Vektor $\mathbf{y}$.   
- $N$ ist die Zahl der Datenpunkte (also die Länge der Vektoren $\mathbf{y}$ und $\mathbf{\hat y}$).



# 2 Classificasion

- **Precision（精确率）**：预测为正的样本中，有多少是真正的正样本。
- **Recall（召回率）**：所有真实正样本中，有多少被正确预测为正。
- **F1-score** 综合考虑了精确率和召回率，是它们的调和平均，适合用来衡量分类模型性能的平衡。


Precision = 

![[image/Pasted image 20250709164220.png]]

- **TP**：真正例（被正确预测为正的样本数）    
- **FP**：假正例（被错误预测为正的样本数）
- 


Recall（召回率）
![[image/Pasted image 20250709164236.png]]
**FN**：假负例（被错误预测为负的真实正样本数）. 实际是正例，但被模型错误地预测为负例的情况。
Wenn FN = 0 :  Dann, alle positve Sample als Prositive geschatztet 




F1-score  = 2 x (Precision x recall)/( precision +recall)
The F1-score is the harmonic mean of precision and recall, calculated as:
**F1 = 2 × (Precision × Recall) / (Precision + Recall)**

---



```
true_labels = [0, 1, 0, 0, 1, 0, 1]
classification_A = [0, 1, 1, 1, 1, 0, 1]
classification_B = [1, 0, 1, 0, 1, 1, 1]
classification_C = [0, 0, 0, 0, 0, 1, 0]
classification_D = [0, 1, 0, 0, 0, 1, 0]
```


Klassifizierungen A     
前一个为实际的值, 后一个为预测试 
PP = 3,  NP  = 2 , PN = 0, NN = 2 
recall  =   3/3 + 0  = 1 
precision  = 3/ 3 + 2    = 3/5
**F1-score* =   38 



## 2.1 Confusion  matrix 


混淆矩阵是一种用来**评价分类模型性能**的工具，特别是二分类问题。它把模型的预测结果和真实结果进行对比，统计不同类型的预测情况，帮助我们直观理解模型的表现。

| 预测为正 (Positive)     | 预测为负 (Negative)     |                     |
| ------------------- | ------------------- | ------------------- |
| **实际为正 (Positive)** | True Positive (TP)  | False Negative (FN) |
| **实际为负 (Negative)** | False Positive (FP) | True Negative (TN)  |


## 2.2 Beispiel 

![[image/klassifikation.png]]



(1,1) , f(x) = 2  P     p      , tp
(1, 2 ), f(x) = 0 1 Q   p   . fn
(3,2) f(x) = 2, P     q  , np
(4,3) f(x) = 1 P    q,   np


Recall  = 1 / (1+1)  
precision    = 1 / (1+0)



## 2.3 K-nearest-neighbor 


Das untenstehende Diagramm zeigt die Verteilung der Attributswerte eines zweidimensionalen Trainingsdatensatzes inklusive der zugehörigen Labels als Farben.

Bestimmen Sie den jeweils kleinsten (ganzzahligen) Wert für k, mit dem der k-Nearest-Neighbor (kNN) Algorithmus die zusätzlichen Datenpunkte in das gewünschte Cluster einordnert. Bei gleichem k Ordnert der Algorithmus den Punkt in das Cluster mit der niedrigeren Ziffer ein. Als Abstandsmaß wird die Manhatten-Distanz verwendet



# 3 Categorical Attribute 

x =` [z, y, z]`

将这个  转换成 三个 attribute, jeder is boolean bvalue  boolean value   `z = [true, false],  y[true, false] z [true, false]`   . 这么做的好处 是 每个 atttibute 的值 为 0或者1 ,  没有 那个值更大 更小  

或者 用 one hat encoding 将 一个 Categorical Attritbue 转换成 一个  `[1,2, 3]`, 这样 值就有大小之分你

**所有值之间没有大小之分**，适合机器学习模型使用，比如线性回归、KNN 等，因为它们不应该“误以为”z 比 y 大或小。






