

# 1 Duplicate detection


Origins of Duplicates

 Problem 1: Representations are not identical. R
 Fuzzy duplicates 1 
 Solution: Similarity measures
     Value- and record-comparisons 
     Domain-dependent or domain-independent

 Problem 2: Data sets are large.
 Quadratic complexity: Comparison of every pair of records. 
 Solution: Algorithms
     E.g., avoid comparisons by partitioning.

![](image/Pasted%20image%2020260125000016.png)

---

Entities, Duplicates, Duplicate Detection

Entity:
An entity is something that exists in the real world, as a subject or as an object, actually or potentially, concretely or abstractly, physically or not.

Duplicate:
Two records that represent the same real-world entity.


Duplicate detection:
The process of identifying all representations of the same real-world entity in one dataset or across many and grouping them together.


![](image/Pasted%20image%2020260124235834.png)


# 2 Similarity measures

## 2.1 Edit-based Similarity Measures
![](image/Pasted%20image%2020260125000049.png)


## 2.2 Token-based Similarity Measures

![](image/Pasted%20image%2020260125000356.png)

![](image/Pasted%20image%2020260125000417.png)

## 2.3 Domain-dependent Similarity Measures

 Data types
     Special similarity for dates
         Jan. 1st, 1st of each month
     Special similarity for numerical attributes 
    
 Dictionary lookup
 Geographic distance
 Abbreviated first names
     Bill = William, Bob = Robert, Dick = Richard, Hank = Henry, Chuck = Charles, Ted = Edward, ...


## 2.4 From Value Similarity to Record Similarity

![](image/Pasted%20image%2020260125000537.png)


## 2.5 Choossing thresholds


 Previously: Notoriously difficult problem sethlearntheweights
     Calculate similarity for all pairs of records tranlateScore to classylathon Duplicatesand
         Or for a carefully chosen subset with at least many duplicates
     Sort pairs descendingly by their similarity
     Quickly browse list top to bottom to find first few non-duplicates
     Carefully continue to browse until you find more non-duplicates than duplicates 
     That similarity is a good threshold


Today:
 Learn the threshold based on found duplicates

look at the prelabels Duplicates
learn the weights
tranlate score to calssication: Duplicated and non-Dulicaptes 



# 3 Algorithms


## 3.1 
![](image/Pasted%20image%2020260125000815.png)

![](image/Pasted%20image%2020260125000821.png)

![](image/Pasted%20image%2020260125001010.png)

## 3.2 
引入 Idea: Partitioning

![](image/Pasted%20image%2020260125001036.png)


![](image/Pasted%20image%2020260125001042.png)

![](image/Pasted%20image%2020260125001052.png)


Blocking and Blocking Keys

Blocking:
Partition the records (horizontally) and compare pairs of records only within a partition 

Blocking Key:
All records within the same partition share the same blocking key. The key must be clean, and distinctive.

 Choosing the right key is difficult -> Partition multiple times by different blocking keys. 
 - Then apply transitive closure on discovered duplicates.
 - How often can you apply it without being slower than brute force?


## 3.3 

![](image/Pasted%20image%2020260125001402.png)

![](image/Pasted%20image%2020260125001627.png)

![](image/Pasted%20image%2020260125001634.png)


# 4 Data sets and evaluation


## 4.1 Precision & Recall

![](image/Pasted%20image%2020260125001750.png)


![](image/Pasted%20image%2020260125001938.png)

Fraction of predicted positives that are
actually correct
Large precision: very small false positives errors committed by the model
"Don't be wrong when predicting a positive"

Fraction of positives correctly predicted
▪ Large recall: very few positive samples misclassified as the negative class
"Make sure to predict all positives correctly"


![](image/Pasted%20image%2020260125002151.png)

## 4.2 F1 Score: Combining Precision and Recall

![](image/Pasted%20image%2020260125002214.png)


▪ Aim: build a model that optimizes both recall and precision
▪ F1 score: harmonic mean between recall and precision 𝐹1 =

![](image/Pasted%20image%2020260125002932.png)



### 4.2.1 Other effectiveness measures

![](image/Pasted%20image%2020260125004006.png)



 Accuracy
 ( T P + T N ) / ( T P + F P + T N + F N ) 为i i p l i a e e s  Used for balanced classes 日为大部分
 In duplicate detection, TN usually dominates overall result2
 Specificity
 TN / (TN + FP)
False negatives
i9idasa.mgacc 无专义 rang
True negatives
pairs.is
如的
consider blanc of classes de re tune
All pairs
True duplicates
 = TN / true non-matches
 Again: TN dominates overall result
 Reduction ratio
 1 – ((TP+TN)/(FN+TP+FP+TN))
= 1 – accuracy
 Pairs completeness
 TP / (FN + TP) = recall
 Pairs quality
 TP / (TP + TN)

## 4.3 From binary to probabilistic evaluation

![](image/Pasted%20image%2020260125002414.png)


▪ Graphical display for the tradeoff between true positive rate (TPR) and false positive rate (FPR)
▪ Evaluate TPR and FPR for any possible 𝑦thresh and arrive at ROC curves

![](image/Pasted%20image%2020260125002421.png)


---

▪ Which model is better on average across all possible decision thresholds?
▪ Compute the integral of the ROC
▪ Edge cases:
▪ Perfect model (TPR=1, FPR=0): AUC = 1.0
▪ Random guessing (TPR=FPR): AUC = 0.5
▪ A model with higher potential has a higher AUC than another model
▪ Use case:
▪ Comparing the performance among different classifiers
▪ Comparing the performance of a classifier for different classes

![](image/Pasted%20image%2020260125002431.png)

---

▪ Precision-recall curve (PRC): tradeoff between precision and recall for different decision thresholds
▪ ROC is only suited for balanced data, PRC is also working for imbalanced data
▪ The longer a large precision can be sustained,
the better the model. Note that the curve does not
necessarily decrease monotonically with recall
▪ AUC-PRC can be computed analogously
▪ Optimal choice of probability threshold 𝑦thresh for
specific use case (either favoring high precision or recall),
e.g. max𝑦thresh 𝐹1



![](image/Pasted%20image%2020260125002440.png)

## 4.4 Imbalanced Classes

▪ Data imbalance (or class imbalance): one class label is severely over-represented (majority class) in the data set, while the minority class is the positive class
▪ Examples:
    ▪ Credit card frauds: 1 out of 100
    ▪ Machine faults: component failure after 10x operational hours
    ▪ Brake system vibrations (see homework no 1): 3-5% noise occurrence
▪ Fitting any model without consideration of the class imbalance will lead to
    ▪ Dummy models: predicting the majority class with 𝑃 𝑥 = 1.0
    ▪ Large false negatives rate

![](image/Pasted%20image%2020260125002658.png)


Strategies for Data Imbalance

▪ Different classification metrics
    ▪ FNR, TPR, F1
    ▪ Weighted scores (weight = inverse of occurrence rate)
    ▪ Matthews' correlation coefficient MCC. MCC=0 (random guessing), MCC=1.0 (best)
    ▪ PRC, AUC-PRC
▪ Data treatment: Modify class distributions (attention!!!) – not recommended
    ▪ Oversampling: insert copies (or variations) of the positive class into the data set
    ▪ Undersampling: remove samples from the majority class

![](image/Pasted%20image%2020260125002720.png)


## 4.5 Runtime/Complexity

 Problem: Measure not only quality of similarity measure, but also that of algorithm 
 Solution 1: Runtime measurements
     But: Different hardware, difficult repeatability
 Solution 2: Measure how well/poor algorithms filter candidates

![](image/Pasted%20image%2020260125004105.png)




# 5 Data fusion

 The result of duplicate detection is a set of clusters ( duplicate groups) where each group/cluster represents one real world entity.
 Groups can consist of one to thousands of entries. 
 Which record to use from each group?


![](image/Pasted%20image%2020260125004313.png)

![](image/Pasted%20image%2020260125004345.png)

![](image/Pasted%20image%2020260125004358.png)


Aggregration Functions
Min, Max, Sum, Count, Avg, StdDev
Standard aggregation
Random
Random choice
First, Last
Choose first/last value; depends on order
Longest, Shortest
Choose longest/shortest value
Choose(source)
Choose value froma particular source
ChooseDepending(col, val)
Choose depending on val in other column col
Vote 1计youhavemanyrecord Coalesce
Majority decision
Choose first non-null value
Group, Concat
Group or concatenate all values
MostRecent
Choose most recent (up-to-date) value
MostAbstract, MostSpecific
Use a taxonomy / ontology
....

