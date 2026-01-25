
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


Holistic Data Cleaning
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

 HoloClean
 Baran

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

