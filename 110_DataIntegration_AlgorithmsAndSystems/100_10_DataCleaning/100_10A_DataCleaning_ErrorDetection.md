



# 1 Data Errors, Data Cleaning, and Error Detection 

## 1.1 Definitions

 Data Cleaning consists of two tasks:
 Error Detection: Finding erroneous values
 Error Correction: Repairing erroneous values to correct values

 What is an Error?
 General model:
 Error = A value that is different from ground truth

![](image/Pasted%20image%2020260125184428.png)



 
## 1.2 Error Types


 Based on discovery strategies: 
 Outliers:
 Data values that deviate from their corresponding column value distributions. 

 Pattern Violations:
 Data values that violate syntactic and semantic constraints. 

 Duplicates:
 Distinct records that refer to the same real-world entity with mismatched attribute values. 

 Constraint violations:
 Data values that violate any kind of integrity constraints.

![](image/Pasted%20image%2020260125184503.png)


## 1.3 Error Detection Tools:
 Input: a table, optionally along with constraints, rules, or examples.
 Output: the set of cells predicted to be erroneous, i.e., different from ground truth.

![](image/Pasted%20image%2020260125184525.png)



# 2 Detection Strategies

1. Quantitative algorithms
 Statistical outliers
2. Pattern verification and enforcement tools  Syntactical patterns, such as date formatting  Semantical patterns, such as location names
3. Deduplication
 Discovering conflicting attribute values in duplicates
4. Rule-based detection algorithms  Detecting violation of constraints

![](image/Pasted%20image%2020260125184645.png)

## 2.1 Quantitative Algorithms 

 Example 1: Histogram-based detection
 Builds a histogram of the frequency of data values.
 Marks data cells from rare bins as errors


![](image/Pasted%20image%2020260125184731.png)


 Example 2: Gaussian-based detection
 Builds a Gaussian distribution based on magnitude of numerical values. 
 Marks cells whose normalized distance is farther than a threshold.
![](image/Pasted%20image%2020260125184745.png)

## 2.2 Pattern Verification

 Syntactic Patterns Verification:
     Check whether a value adheres to the expected format.
         Examples:
         Date format mismatches: mm/dd/yyyy vs. dd.mm.yyyy 
         Phone number format: +49 30 123456 vs. 030-123456

 Semantic Patterns Verification:
     Check whether a value is meaningful within the intended concept.
         Examples:
             City name should be a real location: City = "Peter Pane"
             Colour name must be one of the standard RGB colour names: Colour = "Blurple"


![](image/Pasted%20image%2020260125184844.png)

![](image/Pasted%20image%2020260125184850.png)


## 2.3 Deduplication
## 2.4 Rule-based Detection


Rule-based Detection Algorithms: Detecting violation of constraints

### 2.4.1 Integrity Constraints: 
 Functional Dependencies
 Denial Constraints

 Functional Dependency:
 "X → Y": whenever two records have the same X values, they also have the same Y values.
 Example: Functional Dependency Violations  FD: Zip Code -> City

![](image/Pasted%20image%2020260125185458.png)

---


 Denial Constraint:
 All conditions cannot be true at the same time, otherwise we have a violation.
 Example:
 Two people living in the same state should not violate the natural ordering between salary and tax rat

![](image/Pasted%20image%2020260125185544.png)

 Denial Constraint:
 All conditions cannot be true at the same time, otherwise we have a violation.
 FDs are special cases of denial constraints:
     Example: Zip Code -> City
     It is not allowed that two records share the same Zip Code and yet disagree on the City.
        ¬(R􏰀[ZipCode] = R􏰁[ZipCode]⋀R􏰀[City] ≠R2[City])




### 2.4.2 Rule-based Algorithms: 
 Naïve approach

 Holistic error detection

### 2.4.3 Naïve approach

Executing all rules sequentially
![](image/Pasted%20image%2020260125185640.png)

![](image/Pasted%20image%2020260125185657.png)

![](image/Pasted%20image%2020260125185704.png)


 Holistic Error Detection
 Creating a conflict hypergraph
 Each cell: a vertex
 Every violation detected: a hyperedge, consisting of a set of cells formatting the violation.  A cell in multiple violations is more likely to be wrong.


### 2.4.4 Holistic error detection

 Holistic Error Detection
     Creating a conflict hypergraph
         Each cell: a vertex
         Every violation detected: a hyperedge, consisting of a set of cells formatting the violation. 
     A cell in multiple violations is more likely to be wrong.

![](image/Pasted%20image%2020260125185816.png)


## 2.5 Case Study

There has been extensive research on many different cleaning algorithms
 Usually evaluated on errors injected into clean data
     Which we find unconvincing (finding errors you injected...)
 Tools evaluated with tools of the same category 
     Well-defined but narrow error models

 How well do current techniques work "in the wild"? 
 What about combinations of techniques?

![](image/Pasted%20image%2020260125185848.png)


Analyzed 5 different datasets
-  Identified general error types that can be discovered by tools

Selected 8 different error detection systems

Measured
 effectiveness of each single system 
 combined effectivity
 upper-bound recall

Analyzed impact of enrichment

Tried out domain specific cleaning tools

![](image/Pasted%20image%2020260125185930.png)


![](image/Pasted%20image%2020260125185941.png)

![](image/Pasted%20image%2020260125185952.png)



![](image/Pasted%20image%2020260125190012.png)



Case Study Conclusions (2015)
1. There is no single dominant tool.
2. Improving individual tools has marginal benefit. We need a combination of tools.
3. Picking the right order in applying the tools can improve the precision and help reduce the cost of validation by humans.
4. Domain specific tools can achieve on average high precision and recall compared to general-purpose tools.
5. Rule-based systems and duplicate detection benefit from data enrichment.


## 2.6 Evaluation: Recall and Precision

![](image/Pasted%20image%2020260125190341.png)

 True positives (TP): Correctly declared errors
 False positives (FP): Incorrectly declared errors
 True negatives (TN): Correctly avoided record values i 
 False negatives (FN): Missed errors

 Precision = TP / (TP + FP)
     = TP / declared errors
     Proportion of found errors that are errors 
     Correctness

 Recall = TP / (TP + FN) 
     = TP / all errors
     Proportion of actual errors that are found 
     Completeness

Precision and Recall (Visualized)
![](image/Pasted%20image%2020260125190336.png)

![](image/Pasted%20image%2020260125190347.png)

![](image/Pasted%20image%2020260125190357.png)



## 2.7 Combined Tool 

![](image/Pasted%20image%2020260125190405.png)


Combined Tool Performance
 Naïve approach:
     k tools agree on a value to be an error
         Typical precision-recall trade-off

 Maximum entropy-based order selection:
Run tools on samples and verify the results
Pick the tool with highest precision
Verify the results
Update precision and recall of other tools accordingly Repeat step 2

### 2.7.1 Ordering-based approach

 Precision and recall depending on different minimum precision thresholds (compared to union)

![](image/Pasted%20image%2020260125190500.png)


### 2.7.2 Enrichment and Domain-specific tools

 Enrichment:
     Manually append more columns by joining to other tables of the database
         Improves performance of rule-based and duplicate detection systems
 Domain-specific tool:
     Used a commercial address cleaning service
         High precision on the specific domain
         But did not lead to the increase of overall recall



# 3 Advanced Detection techniques 

Observations: Several strategies have to be combined
 Increasing number of systems both in industry and academia. [Claudel,2016] [Dallachiesa,2013] [Kandel,2011] ...
 DQ systems are tailored towards one data quality issue.
 Datasets seldom contain only one issue: there is usually a mix of different problems (e.g. missing values and rules violations) [Arocena,2015]
 Previous work is done on heuristics-based Aggregation: Union-All, Min-K, Precision-based Ordering [PVLDB'2016]

![](image/Pasted%20image%2020260125190653.png)

## 3.1 **ML-based Approaches**

 Ensemble of detection strategies

 Use metadata as features
 Null value statistics
 Data types
 Value frequencies
 dependencies 

 Learn weights


![](image/Pasted%20image%2020260125190807.png)


----


 Goal: Holistically combine multiple error detection strategies and predict each value of the tuple t[i] as "clean" or "error": {0, 1}.
 Solution:binaryclassificationalgorithms.
 Mainquestion:howtodesignfeaturevectors?

![](image/Pasted%20image%2020260125190849.png)

----

Include Metadata
 Metadata represents characteristics of the dataset.
 Error detection is supported by the incorporation of the datasets semantic. 
 Initial feature vector is extended by the metadata features:
 The classification is performed on the extended feature vector:

![](image/Pasted%20image%2020260125190919.png)

---


Ensemble Learning With Stacking

![](image/Pasted%20image%2020260125190946.png)

----

Limitations so far
 Aggregate the results of multiple error detection algorithms 
     Unsupervised [PVLDB'16]:
         Majority vote
     Min-k (One has to choose k though) 
     Union (= Min-1)
     Semi-supervised [SSDBM'18]
         Learn the best combination of error detection techniques 
         Translate error detection into a classification task
 Overarching problem:
 Somebody needs to properly configure all of these techniques.



![](image/Pasted%20image%2020260125190957.png)

---

Goal: User tells us what to clean and not how to clean!
 Example-based approach 
     The user
         Cleans a few instances
     System
         Detects the remaining errors 
         Fixes them


1. How to represent instances?
     The same value is wrong/correct in different contexts
     One detected typo does not cover all possible typos
2. How to pick instances?
     Imbalance: dirty data vs. clean data
     Different types of dirty data
3. Later for correction: How to model the classification task?
     How can we find out-of-dataset corrections?
     A correction can theoretically be any possible String value 



## 3.2 The Detection Engine Raha.

![](image/Pasted%20image%2020260125191321.png)


Two Desiderata
 Automatically generate algorithm configurations
     Some algorithms are too complicated for automatic configuration
 Use a limit the number of labels to one label per data error 
     Active learning helps a bit [CIKM'19]
     Make the representation more accurate
Solution
1. Use simple error detection signals as embedding dimensions
2. Use the embedding to cluster values together
3. Ask for labels per cluster

----

Embed Values through Base Detector Signals

![](image/Pasted%20image%2020260125192000.png)


![](image/Pasted%20image%2020260125192022.png)

![](image/Pasted%20image%2020260125192029.png)

![](image/Pasted%20image%2020260125192035.png)

---

Iterative Tuple Selection
 Cluster values of each column individually

 Until the labeling budget is depleted:  
不足的，减少的；人员不足的，人员减少的；耗尽的
     Increase the number of clusters in each column(start with 2 per column)
     Draw a tuple that covers as many unlabeled clusters as possible across all columns. 
     Obtain a label for each tuple value
     Propagate labels and resolve labeling conflicts


![](image/Pasted%20image%2020260125192121.png)


## 3.3 Error Detection using LLMs

![](image/Pasted%20image%2020260125191352.png)


Error Detection with Large Language Models
 Challenges:
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
         How much does it cost to detect errors in one table?