

# 1 Basic 


![[image/Pasted image 20250430162002.png]]



![[image/Pasted image 20250430162134.png]]



![[image/Pasted image 20250430162925.png]]



![[image/Pasted image 20250430162959.png]]


# 2 Relational Operator 

![[image/Pasted image 20250430163539.png]]


![[image/Pasted image 20250430163602.png]]

![[image/Pasted image 20250430163715.png]]


![[image/Pasted image 20250430163814.png]]



![[image/Pasted image 20250430164059.png]]



# 3 MapReduce (Hadoop)

![[image/Pasted image 20250430164604.png]]



![[image/Pasted image 20250430164849.png]]


![[image/Pasted image 20250430165043.png]]


![[image/Pasted image 20250430165250.png]]


Advantage of MapReduce 
It reduces or really eliminates sequentiality of original  data . Because to parallelism of data 

Degree of Parallelism was determined by   the number of Key (the. shawshank, redemption , etw. )


## 3.1 MapReduce vs Relational Data Processing

• Queries in the relational algebra are composed from a set of operators with predefined semantics over relations
• MapReduce executes black box user-defined functions (UDFs) with
"parallelisation contracts" on arbitrary data
• No support for operations on multiple inputs (e.g., Join / CoGroup) in MapReduce


## 3.2 Implementation

![[image/Pasted image 20250430170213.png]]



Coordinator is master, worker is slaver 


![[image/Pasted image 20250430170457.png]]



Map phase , was HashMap 

sort-Merge is very easy to implement a memory efficient 

why sort-Merge at first, why not send the data into Reduce  directly after map 
- in cases the reduce Machine crash 
- make the sorted Data has not to always online, 



![[image/Pasted image 20250430171108.png]]




HDFS（Hadoop Distributed File System）是 Hadoop 的核心组件之一，用于存储大规模数据的分布式文件系统。它非常适合一次写入、多次读取的批处理型大数据应用，比如日志分析、搜索索引构建等。


# 4 Resilient Distributred Datasets (Spark)


![[image/Pasted image 20250430171526.png]]


In Spark 
the data fits into the aggregate memory

you do parallel computations on this rdds again using high level operators



## 4.1 how to create a RDD from datasource 

![[image/Pasted image 20250430171855.png]]


Transformation 0
![[image/Pasted image 20250430172057.png]]


![[image/Pasted image 20250430172401.png]]


![[image/Pasted image 20250430172433.png]]



The nicer type of transformation between rdds os Marrpw Dependencies betweeen 

co coreletive 

![[image/Pasted image 20250430172634.png]]



![[image/Pasted image 20250430172809.png]]


## 4.2 Lineage-Based Recovery 

![[image/Pasted image 20250430172940.png]]

![[image/Pasted image 20250430173140.png]]


![[image/Pasted image 20250430173238.png]]

![[image/Pasted image 20250430173302.png]]


![[image/Pasted image 20250430173358.png]]

you can materialize or safe individual intermediate rdds


# 5 Summary 

![[image/Pasted image 20250430173733.png]]

MapReduce 
parallelization scalability failures and concurrency.

Spark 
Abstractions like computations they offer cost grade transformation but much richer operators not really not reach API and. Also has a simple and simple and optimistic way to the fault tolerance we are spinach basically.
