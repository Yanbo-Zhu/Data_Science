
Lineage 血统

![](image/Pasted%20image%2020260125193338.png)


# 1 Motivation

▪ Data Lineage
    ▪ Data Lineage is the problem of mapping objects in an integrated system to objects in sources, from which the integrated object have been derived
    ▪ Also: Data Provenance
    ▪ Also: Data Pedigree
    ▪ Also: Data Origin

▪ Data Warehouses 
    ▪ Data Analysis
    ▪ Decision Support 
    ▪ Data Mining
    ▪ Aggregation

▪Difficulty of Data Lineage depends on transformation 
    □ SQL: Easy but unrealistic
        ⎼ Data Lineage via SQL views
        ⎼ Data Lineage via operators of relational algebra 
    □ General transformations: difficult but important
        ⎼ Data Lineage via complex, user-defined transformations 
        ⎼ Data Lineage via ETL processes
        ⎼ Data Lineage via composition of 60 + transformations
▪Data Lineage refers to data level. 
    □ Metadata Lineage
    ⎼ SchemaMapping
    ⎼ SchemaIntegration

----

Application Areas


▪ Explanations
    ▪ Help the user understand why an item exists
▪ Scoring
    ▪ Provide a ranked list of "most relevant" results
        ▪ Score by trustworthiness etc. of source 
    ▪ Reasoning about interactions
        ▪ Help the user understand data relationships
▪ Schema mapping debugging
    ▪ We may have a bad result: Determine why that result exists, what is faulty 
    ▪ Determine why some data is not part of the result
▪ Probabilistic databases
    ▪ We may need to know that results are correlated: Encode the relationships, use to assign probabilities

![](image/Pasted%20image%2020260125193634.png)



---


▪ Challenges
    ▪ Runtime overhead
        ▪ ETL
        ▪ During virtual integration 
    ▪ Storage requirement
        ▪ Meta data 
    ▪ Transformations
        ▪ Individual
        ▪ Nested
        ▪ Acyclic graphs
▪ Trade-off between usefulness and cost

![](image/Pasted%20image%2020260125193717.png)

▪ Goal: Table "Sales Soaring"
▪ Computer products, whose sales in the last quarter is double the average of the last three quarters
1. Create tables
2. Define transformatins as graphs 
3. Execute transformations

![](image/Pasted%20image%2020260125193749.png)

![](image/Pasted%20image%2020260125193834.png)


# 2 Data Transformation 



Transformations

▪ Dataset
▪ Set of arbitrary data
▪ Tuple, value, object ▪ here: tuple
▪ Transformation
▪ A Procedure, with a dataset as Input and a dataset as output.
▪ (More general Definition than in Slidedeck for data transformation discovery) ▪ T(I) = O
▪ Composition of transformations
▪ T = T1∘T2 or T(I) = T2(T1(I))
▪ [ Associative: (T1∘T2)∘T3 = T1∘(T2∘T3) ]


![](image/Pasted%20image%2020260125193854.png)

---
Transformations – Properties/Assumptions

![](image/Pasted%20image%2020260125193957.png)

---
Data Lineage: Definition

▪ In general: Transformations can consider all input values for each output value
![](image/Pasted%20image%2020260125194010.png)

![](image/Pasted%20image%2020260125194029.png)

----

Categorizing Transformations
▪ Two extremes
    ▪ Relational operators or views:
        ▪ Exact data lineage is identifiable 
    ▪ Completely arbitrary transformation 
        ▪ The whole input is data lineage
▪ Reality is somewhat in between 

▪ Three transformation classes


----


## 2.1 Transformations - Classification
▪ Dispatcher (literally)
▪ Each single input produces independently null or one or more outputs
▪ Aggregators
▪ An output is the result of a group of input values
▪ Black-Boxes
▪ Everything else

![](image/Pasted%20image%2020260125194123.png)


![](image/Pasted%20image%2020260125194734.png)

## 2.2 Dispatcher
▪ Every input produces null or more outputs

![](image/Pasted%20image%2020260125194133.png)

Example Cont. Dispatcher
▪ T1: Split orders
▪ T2: Select categories
▪ T3: Join and Projection
▪ T4: Aggregation and Pivotization ▪ T5: AVG computation
▪ T6: Selection for sale soars
▪ T7: Projection

![](image/Pasted%20image%2020260125194257.png)

---

Tracing Procedure
▪ Defines for output sets, therefore useful for compositions

▪ Effort:
▪ Complete Scan of the Input
▪ Transformation query for every input value


![](image/Pasted%20image%2020260125194212.png)

---

Dispatcher – Special Case
▪ Filter
▪ ∀iI, T({i}) = {i} or T({i}) = 
▪ Data Lineage: ▪ T*(o) = {o}
▪ Or T*(O) = O
▪ Tracing procedure trivial

![](image/Pasted%20image%2020260125194231.png)

Example Cont. Filter
▪ T1: Split orders
▪ T2: Select categories
▪ T3: Join and Projection
▪ T4: Aggregation and Pivotization ▪ T5: AVG computation
▪ T6: Selection for sale soars
▪ T7: Projection

![](image/Pasted%20image%2020260125194248.png)

## 2.3 Aggregators

▪ Two conditions have to be met

![](image/Pasted%20image%2020260125194315.png)

---

Tracing Aggregators

![](image/Pasted%20image%2020260125194427.png)

---


![](image/Pasted%20image%2020260125194403.png)


▪ T1: Split orders (product lists)
▪ New schema: Orders(OID, cust-id, date, PID, amount)
▪ T2: Select category
▪ Filter for computer category
▪ T3: Join (and projection) over orders and products
▪ New schema: (OID, date, PID, amount, name, price, valid)
Filter
Aggregator
T7
T6
T5
T4
T3
Sales Soar
Dispatcher
▪ T4: Aggregation und pivotization
▪ Sales amount by quarter and product ▪ New schema: (Name, Q1, Q2, Q3, Q4)
▪ T5: Average computation
▪ New schema: (Name, Q1, Q2, Q3, AVG3, Q4)
▪ T6: Selection for sale soars ▪ T7: Projection
Aggregator Aggregator
Dispatcher Dispatcher
▪ New schema: SalesSoar(Name, AVG3, Q4)

---

### 2.3.1 Special Case of Aggregators: Context Free
▪ Context free aggregators
    ▪ 2 inputs belong/do not belong to the same partition, independent other Inputs in the partition. 
    ▪ All aggregators so far are context free.
    ▪ Counter example: Clustering and Average over cluster
        ▪ Membership of two records in a cluster depends on other members of the cluster 
▪ Tracing Procedure easier
    ▪ Intuition: Creation of partitions I is linear. After that check which partition maps to the output object.


聚合函数的特殊情况：上下文无关聚合

▪ 上下文无关聚合函数

判断两个输入是否属于同一分区时，不依赖于分区中的其他输入。

目前讨论的所有聚合函数都是上下文无关的。

▪ 反例：聚类和聚类均值

判断两条记录是否属于同一聚类，取决于聚类中的其他成员。

▪ 追踪过程更简单

直观理解：分区的创建过程是线性的，之后只需检查哪个分区映射到输出对象。

![](image/Pasted%20image%2020260125194630.png)


### 2.3.2 Special Case of Aggregators: Key Maintaining

![](image/Pasted%20image%2020260125194708.png)


▪ Key maintaining aggregators
▪ Let I have partitions I1, ..., In, so that T(I) = {o1,...on}.
▪ ∀k, ∀I'Ik : T(I') = {o'k} and o'k.key = ok.key
▪ Example: "Normal" grouping and aggregation
▪ Counter example: Grouping, which does not contain the grouping attribute
▪ Tracing procedure ▪ Effort: |I|
▪ Intuition: key in transformation result is used to check correspondence



## 2.4 Aggregator vs. Dispatcher
▪ Dispatcher
    ▪ Every Input produces independently null or more outputs.

▪ Aggregator
    ▪ Every Input is involved in at least one output
    ▪ Inputs can be partitioned, so that every partition is responsible for exactly one output

▪ Transformations, that are Dispatcher and Aggregator at the same time? 
▪ Identity
▪ Projection (Without duplicate removal)

![](image/Pasted%20image%2020260125194352.png)

## 2.5 Black Boxes


Black Boxes
▪ Transformations, which are neither Dispatcher nor Aggregators, nor have an explicit tracing procedure
▪ Example:
    ▪ Sorting and Pasting of an ordinal number.
    ▪ No Dispatcher, because Output is not independent
    ▪ No Aggregator, because each Output can only be created using the complete input.
▪ Lineage:
    ▪ T*(o,I) = I
▪ Tracing Procedure: 
    ▪ Trivial,
    ▪ "useless"


# 3 Transformation Sequences


▪ So far: Lineage and Tracing for single transformations ▪ Now: Sequences of transformations
▪ Let I2* = T2*(o,I2)
▪ Let I* = T1*(I2*,I)
▪ Then I* = (T1∘T2)*(o,I)


![](image/Pasted%20image%2020260125194935.png)

---


Tracing Procedures
▪ Naive Tracing Procedure for sequences T ∘...∘ T :
▪ Store all intermediate results Ik
▪ Tracing Procedure backwards for each transformation step 
▪ Not efficient:
    ▪ High storage requirements 
    ▪ Many transformation steps
▪ Better: Explicit combination of transformations 
    ▪ Given a transformation sequences
1. Normalize sequence via appropriate combinations 
2. Identify intermediate results that are relevant for Tracing 
3. During transformation save the intermediate results 
4. Iterative Tracing via normalized sequence

**追踪过程**

▪ **针对变换序列 \( T_1 \circ T_2 \circ \dots \circ T_n \) 的朴素追踪方法**：
- 存储所有中间结果 \( I_k \)。
- 对每个变换步骤逆向执行追踪过程。
- **效率低下**：
    - 存储需求高。
    - 变换步骤多。

▪ **更好的方法：显式组合变换**  
对于给定的变换序列：
1. 通过适当的组合对序列进行**规范化**。
2. 识别与追踪相关的**中间结果**。
3. 在变换过程中保存这些中间结果。
4. 通过规范化序列进行**迭代追踪**。

---


Normalization
▪ Principally, very and all transformations can be combined into one transformation. 
    ▪ But: Desired features might get lost.
▪ Transformation characteristics
    ▪ Class (Dispatcher, Aggregator, Filter, BlackBox) 
    ▪ Completeness
        ▪ Every Input produces an Output 
    ▪ Tracing Procedure / Inverse
    ▪ + more
▪ Combination of features with AND
▪ Identification, which features are desired, and when normalization pays off, is a complex problem.
    ▪ Cost model and algorithms are necessary

**规范化**

▪ **原则上，可以将任意多乃至所有变换组合成一个变换。**
    - 但：**所需特性可能会丢失**。

▪ **变换特性**
    - **类别**（分发器、聚合器、过滤器、黑盒）
    - **完备性**
        - 每个输入都产生一个输出
    - **可追踪性/逆变换**
    - 及其他更多特性

▪ **特性的组合采用"与"逻辑**
    - 组合后的变换特性是所有单个变换特性的交集。

▪ **识别哪些特性是所需的，以及何时进行规范化才划算，这是一个复杂的问题。**
    - 需要**成本模型和算法**来权衡决策。