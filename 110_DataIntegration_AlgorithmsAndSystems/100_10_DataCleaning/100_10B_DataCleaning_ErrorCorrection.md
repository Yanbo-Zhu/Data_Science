
# 1 Error Correction 


Definition
Error Correction Tools:
 Input: a table, and a set of detected erronous cells, optionally along with constraints, rules, or examples. 
 Output: Ideally, the cleaned version of the table, with each error being fixed to the actual correct value
(ground truth).


![](image/Pasted%20image%2020260125015649.png)

---

Evaluation Metrics 

 Precision:
 How many of the fixes were actually correct?
 𝑃𝑟𝑒𝑐𝑖𝑠𝑖𝑜𝑛 = 􏰀􏰁􏰂􏰂􏰃􏰀􏰄􏰅􏰆 􏰇􏰈􏰉􏰃􏰊 􏰃􏰂􏰂􏰁􏰂􏰋 􏰌􏰅􏰅 􏰍􏰂􏰁􏰍􏰁􏰋􏰃􏰊 􏰇􏰈􏰉􏰃􏰋
 Recall:
 How many of the existing errors did the system fix?
 𝑅𝑒𝑐𝑎𝑙𝑙 = 􏰀􏰁􏰂􏰂􏰃􏰀􏰄􏰅􏰆 􏰇􏰈􏰉􏰃􏰊 􏰃􏰂􏰂􏰁􏰂􏰋 􏰌􏰅􏰅 􏰃􏰂􏰂􏰁􏰂􏰋
 F1-Score:
 Harmonic mean

---

Automation


 Automatic repair of errors:
     Similarity join with a dictionary
     Sophisticated probabilistic approaches
         Truth discovery [Yin, Han, Yu. KDD'07] 
     Conflict resolution
     Data Fusion [Bleiholder, Naumann. ACM Surveys'09] 
 Crowd-sourcing
     CrowdER [Wang et al. PVLDB'12])


 Semi-automatic repair:
 User-defined transformation
 Already hard enough!

----

Repairing Techniques


 Rule-based approaches 
     Naïve approach
     Holistic data repair
 Machine learning and probabilistic approaches 
     HoloClean
     Baran
 LLM-driven approaches



# 2 Rule-based Repairing Techniques
## 2.1 Naïve Approach

 These techniques employ rules one-by-one
     Input: a set of ICs defined on the database schema R – any database instance I of R should conform to these
constraints.
     Output: repaired instance of I, called I'.

 Example:
     FD-Violation Repair
    • FD1: City → ST 
    • FD2:AC→ST

![](image/Pasted%20image%2020260125020106.png)


## 2.2 Holistic Data Cleaning

整体的，全面的；功能整体性的


### 2.2.1 Motivation
 Rules
 The city determines the state (t4 and t6 violate this rule)
 Among employees having the same role, salaries in NYC should be higher (t5 and t6 violate this rule)



![](image/Pasted%20image%2020260125020158.png)

![](image/Pasted%20image%2020260125020258.png)


![](image/Pasted%20image%2020260125020310.png)


![](image/Pasted%20image%2020260125020356.png)



### 2.2.2 Rule Language: Denial Constraints
 The method accepts Denial Constraints (DCs) as input 
 A declarative specification of the quality rules

![](image/Pasted%20image%2020260125020445.png)


## 2.3 Conflict hypergraph (CH) and Repair context (RC)
 Conflict hypergraph (CH):
     encodes constraint violations
 Repair context (RC):
     encodes violation repairs


![](image/Pasted%20image%2020260125020644.png)


 The nodes are the violating cells
 The edges link cells involved in the same violation


![](image/Pasted%20image%2020260125020709.png)

![](image/Pasted%20image%2020260125020720.png)



 We start from cells that minimum vertex cover identifies as likely to be changed  t2[C]
 Start with t2[C] in queue
 Involved in 3 hyperedges
 Consider e1
 Add repair t2[C] = t1[C]  Add t1[C] to queue
 Consider e2
 Add repair t2[C] = t3[C]  Add t3[C] to queue
 Consider e3
 Add repair t2[C] = t4[C]  Add t4[C]
 Considering e4
 Add repair t4[C] = 5



---


## 2.4 Holistic Data Cleaning algorithms 
 Input:
     A database with errors
     A set of denial constraints (DCs)
 Output: Repaired database instance I'.

Steps:
1. Detect Violations
    • Build a conflict hypergraph 
2. Prioritize Cells to Fix
    • Use a minimum vertex cover (MVC) heuristic to choose the cells that can resolve many violations. 
3. Repair Cells One by One
    • For each selected cell:
    • Look at all constraints it participates in
    • Generate possible values
    • Choose the best update according to the cost model
    • Apply the update
4. Iterate Until Stable
    • Recompute conflicts
    • Continue repairing until no new cells need fixing.
5. Return Repaired Data





# 3 Probabilistic and ML-based Repairing techniques 

## 3.1 HoloClean

Holistic Data Repairs with Probabilistic Inference 
概率性的


![](image/Pasted%20image%2020260125134008.png)

---

### 3.1.1 Data repairing signals 
 Integrity constraints  External data  Statistical analysis



---
 Integrity constraints

 The goal is to update the input dataset such that no integrity constraints are violated.
 Minimality principle states that given two candidate sets of repairs, the one with fewer changes with
respect to the original data is preferable.


 目标：更新输入数据集，确保不违反任何完整性约束。
 最小化原则：当存在两组候选修复方案时，相较于原始数据改动更少的一组更优。


![](image/Pasted%20image%2020260125134115.png)


----
 External data 

 The matching process is usually described via a collection of matching dependencies.

![](image/Pasted%20image%2020260125134231.png)


---
 Statistical analysis

 It leverages quantitative statistics of the input dataset to update values that correspond to outliers
![](image/Pasted%20image%2020260125134257.png)



----

### 3.1.2 Challenges

 Each type of signal is associated with different operations over the input data 
     Integrity constraints require reasoning about the satisfiability of constraints
     External information requires efficient matching procedures
 Different signals may suggest conflicting repairs
     Check the repaired ZIP codes in the running example



 每种信号类型关联对输入数据的不同操作
 完整性约束：需要对约束的可满足性进行推理
 外部信息：需要高效的匹配处理流程

 不同信号可能建议相互冲突的修复方案
 请检查运行示例中已修复的邮政编码（参考原文语境建议保留"running example"的专业表述，也可译



### 3.1.3 ###

![](image/Pasted%20image%2020260125134352.png)

 Error detection
     Detect cells in dataset with potentially inaccurate values
 Compilation
     Associate each cell with a random variable that takes values from a finite domain
     Compile a probabilistic graphical model that describes the distribution of random variables for the cells
 Data Repairing
      Run statistical learning and inference over the joint distribution of variables to compute the marginal probability for all values in domain
      Assign the value that maximizes the marginal probability



 错误检测
 检测数据集中可能包含不准确值的单元格

 模型编译
 为每个单元格关联一个取值于有限域的随机变量
 编译一个描述单元格随机变量分布的概率图模型

 数据修复
 对变量的联合分布进行统计学习与推理，计算所有域值的边际概率
 为单元格分配具有最大边际概率的值





![](image/Pasted%20image%2020260125134358.png)


### 3.1.4 Example-based  cleaning approach

 Example-based approach 
 The user
     cleans a few instances 
 System
     detects the remaining errors 
     fixes them


Challenges in Example-based Cleaning
1. How to represent instances?
    1.  The same value is wrong/correct in different contexts
    2.  One detected typo does not cover all possible typos
2. How to pick instances
    1.  Imbalance: dirty data vs. clean data
    2.  Different types of dirty data
3. Now: How to model the classification task?
    1. A correction can theoretically be any possible String value 
    2. How can we find out-of-dataset corrections?

基于示例清洗的挑战

如何表示实例？
 同一值在不同情境下可能是错误的/正确的
 一种检测到的拼写错误无法涵盖所有可能的拼写错误

如何选取实例？
 数据不平衡问题：脏数据与干净数据
 脏数据存在多种类型

当前核心问题：如何建模分类任务？
理论上，一个更正结果可以是任何可能的字符串值
如何发现数据集之外的更正方案？



## 3.2 Baran

![](image/Pasted%20image%2020260125140545.png)

----

Error Correction Challenges: The Trade-Off
 Correctness
     We need high precision
     But the search space is infinite 
 Completeness
     We need high recall
         But corrections may not be found in the dataset
 Automation
     We need to involve the user minimally
         For only labeling a few data errors



![](image/Pasted%20image%2020260125140624.png)

### 3.2.1 Two-Step Task Formulation
Step 1: Correction Candidates Generation
Step 2: Learning the Actual Correction

![](image/Pasted%20image%2020260125140643.png)

---

Step 1: Correction Candidates Generation
This step increases the achievable recall bound of the error correction task.
Error Corrector Model 1 .
Error Corrector Model |M|

![](image/Pasted%20image%2020260125140715.png)


Leverage data error context to propose correction candidates
 Value-Based Correction Candidates
 Vicinity-Based Correction Candidates 周围地区，邻近地区，附近
 Domain-Based Correction Candidates

![](image/Pasted%20image%2020260125140843.png)

The Challenges of Using Error Corrector Models
Each model proposes correction candidates but...
 Heterogeneity
     With a different correction operation
 Huge Search Space
     Many of these correction candidates are irrelevant
We need to consider the models as black box correctors and process all their proposed correction candidates effectively and efficiently.

---

Step 2: Learning the Actual Correction
This step preserves high precision of the error correction task.
![](image/Pasted%20image%2020260125140952.png)


Pretraining Error Corrector Models
The Source of Correction Candidates: 
 The Dataset itself
     E.g., The Active Domain
 The user-provided examples
     E.g., "5𝑡h 𝑠𝑡𝑟" → "5𝑡h 𝑠𝑡𝑟𝑒𝑒𝑡"
Can we learn out-of-dataset correction candidates upfront without any user involvement?

纠正候选值的来源：
 数据集本身
 例如：活跃值域
 用户提供的示例
 例如："5𝑡h 𝑠𝑡𝑟" → "5𝑡h 𝑠𝑡𝑟𝑒𝑒𝑡"

能否在无需用户参与的情况下，提前学习数据集之外的纠正候选值？



 Use any previously cleaned dataset to obtain value corrections
     to create more correctors
     and pretrain generated correctors
 Benefit:
     Baran converges faster as the pretrained models generate more correction candidates with the same user labeling budget.

 利用任何已清洗过的历史数据集来获取值纠正样本
 用于生成更多的纠正器
 并对生成的纠正器进行预训练
 优势：
 通过预训练的模型，在相同的用户标注预算下能够生成更多纠正候选值，从而使 Baran 系统收敛得更

### 3.2.2 

![](image/Pasted%20image%2020260125141131.png)

![](image/Pasted%20image%2020260125141139.png)

# 4 LLM-driven Data Repair


"Table understanding" error in Data Imputation: while the model knows the person's name for the missing cell, it misses the table context.


![](image/Pasted%20image%2020260125021759.png)


Data Cleaning with Large Language Models
Challenges:
     LLMs do not understand table structures. 
     Domain Specifity
         Useful in general domains such as Address data
     Challenging in highly specialized domains, such as financial or medical data 
     Privacy
         Passing sensitive data to third-party 
     Providing examples as shots
         Which samples to pick?
         How many examples does the user need to provide? 
     Debuggability
         Non-deterministic results 
     Cost
         How much does it cost to repair errors in one table?

