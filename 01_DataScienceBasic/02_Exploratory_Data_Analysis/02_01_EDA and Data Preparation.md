

# 1 Data Properties

## 1.1 Structure -- the “shape” of a data file


1 
Data profiling is the set of activities and processes to determine the metadata about a given dataset.

![[image/Pasted image 20250428190716.png]]




----

2 
每个 column 中的 数值的 类型 
quantitative 还是 qualitative 类型 

![[image/Pasted image 20250428142359.png]]

## 1.2 Granularity -- how fine/coarse is each datum



## 1.3 Scope -- how (in)complete is the data


## 1.4 Temporality -- how is the data situated in time



## 1.5 Faithfulness -- how well does the data capture “reality”


# 2 Data Quality Problem 

![[image/Pasted image 20250428145822.png]]

![[image/Pasted image 20250428191731.png]]


▪ Missing Values or default values
▪ Truncated data (early excel limits: 65536 Rows, 255 Columns)
    ▪ Solution: be aware of consequences in analysis: how did truncation affect sample?
▪ Time Zone Inconsistencies
    ▪ Solution 1: convert to a common timezone (e.g., UTC)
    ▪ Solution 2: convert to the timezone of the location – useful in modeling behavior.
▪ Duplicated Records or Fields
    ▪ Solution: identify and eliminate (use primary key) implications on sample?
▪ Spelling Errors
    ▪ Solution: Apply corrections or drop records not in a dictionary 🡪 implications on sample?
▪ Units not specified or consistent
    ▪ Solution: Infer units, check values are in reasonable ranges for data

---
missing values

▪ Drop records with missing values
    ▪ Probably most common
    ▪ Caution: check for biases introduced by dropped values
    ▪ Missing or corrupt records might be related to something of interest
▪ Imputation: (Inferring missing values)
    ▪ Mean Imputation: replace with an average value
        ▪ Which mean? Often use closest related subgroup mean.
    ▪ Hot deck imputation: replace with a random value
    ▪ Choose a random value from the subgroup and use it for the missing value.




# 3 Data Error 

![[image/Pasted image 20250428191825.png]]

Error Detection Strategies
1. Rule-based detection algorithms
    1. Detecting violation of constraints, such as Zip Code → City
2. Pattern verification and enforcement tools
    1. Syntactical patterns, such as date formatting
    2. Semantical patterns, such as location names
3. Quantitative algorithms
    1. Statistical outliers
4. Deduplication
    1. Discovering conflicting attribute values in duplicates




# 4 Fixing Errors

## 4.1 Correction through  standardization 

1 standardize the data using canonicalize_county()
or
2 check similarity of the data 

![[image/Pasted image 20250428192115.png]]

![[image/Pasted image 20250428192126.png]]


![[image/Pasted image 20250428192137.png]]


## 4.2 Duplicate Detection and Elimination

Entity:
An entity is something that exists in the real world, as a subject or as an object, actually or potentially, concretely or abstractly, physically or not.

Duplicate:
Two records that represent the same real-world entity.

Duplicate detection:
The process of identifying all representations of the same real-world entity in one dataset or across many and grouping them together

Problem 1: Representations are not identical.
    ▪ Fuzzy duplicates
▪ Solution: Similarity measures
    ▪ Value- and record-comparisons
    ▪ Domain-dependent or domain-independent
▪ Problem 2: Data sets are large.
    ▪ Quadratic complexity: Comparison of every pair of records.
    ▪ Solution: Algorithms
    ▪ E.g., avoid comparisons by partitioning.




### 4.2.1 Check Similiarity 

Edit-based Similarity Measures
![[image/Pasted image 20250428192525.png]]

Token-based Similarity Measures 
![[image/Pasted image 20250428192557.png]]



From Value Similarity to Record Similarity
![[image/Pasted image 20250428192634.png]]


Domain-dependent Similarity Measures
![[image/Pasted image 20250428192652.png]]


# 5 Avoid unnecessary comparisons

![[image/Pasted image 20250428192740.png]]



Reduece the Compirsion times: 
Blocking and Block Keys



Blocking:
Partition the records (horizontally) and compare pairs of records only within a partition.

locking Key:
All records within the same partition share the same blocking key. The key must be clean, and distinctive.


hoosing the right key is difficult → Partition multiple times by different blocking keys.
▪ Then apply transitive closure on discovered duplicates.
▪ How often can you apply it without being slower than brute force?

