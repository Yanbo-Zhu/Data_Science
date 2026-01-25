

# 1 Information quality  IQ
    
# 2 IQ criteria

1 Content-based Criteria

...concern the actual data. 

 Accuracy
     is the extent to which data is correct, reliable, and certified free of error. [WS96] 
 Completeness
     is the extent to which data is not missing and is of sufficient breadth, depth, and scope for the task at hand. [WS96] 
 Customer support
     is the amount and usefulness of human help via email or telephone. 
 Documentation
     is the amount and usefulness of documents with metadata. 
 Interpretability
     is the extent to which data is in appropriate languages, symbols, and units, and the definitions are clear. [WS96] 
 Relevancy (or relevance)
     is the extent to which data is applicable and helpful for the task at hand. [WS96] 
 Reliability
     is the degree to which the user can trust the information 
 Value-Added
     is the extent to which data is beneficial and provides advantages from its use. [WS96]

2 Technical Criteria
...concern software and hardware.
 Accessibility (or availability)
     of a DBMS is the probability that a feasible query is correctly answered in a given time range.
     Is the extent to which data are available or easily and quickly receivable [WS96].

 Latency
     is the amount of time in seconds from issuing the query until the first data item reaches the user
 Price (cost effectiveness)
     is the amount of money a user has to pay for a query.
     is the extent to which the cost of collecting appropriate data is reasonable [WS96].

 Response time
     measures the delay in seconds between submission of a query by the user and reception of the complete response from the IS.
 Security
     is the extent to which access to data is restricted appropriately to maintain its security [WS96].

 Timeliness
     is the extent to which the age of the data is appropriate for the task at hand [WS96].


Intellectual IQ Criteria
 ...concern subjective aspects. 
 Believability
     is the extent to which data is regarded as true, real, and credible [WS96]. 
 Objectivity
     is the extent to which data is unbiased, unprejudiced, and impartial [WS96]. 
 Reputation
     is the extent to which data is trusted or highly regarded in terms of its source or content [WS96].


Instantiation-related IQ Criteria
...concern the presentation of retrieved data. 
 Amount of data
     is the extent to which the quantity or volume of available data is appropriate [WS96]. 
 Representational conciseness
     is the extent to which data is compactly represented without being overwhelming [WS96].
 Representational consistency
     is the extent to which data is always represented in the same format and are compatible with previous data
[WS96].
 Understandability (ease of understanding)
     is the extent to which data are clear without ambiguity and easily comprehended [WS96].
 Verifiability (traceability, lineage)
     Is the extent to which data are well documented, verifiable, and easily attributed to a source [WS96].

 数据量
 指可用数据的数量或规模是否合适的程度 [WS96]。
 表征简洁性
 指数据在紧凑表征的同时不至于令人难以应对的程度 [WS96]。
 表征一致性
 指数据始终以相同格式呈现且与以往数据兼容的程度 [WS96]。
 可理解性（易于理解）
 指数据清晰无歧义、易于理解的程度 [WS96]。
 可验证性（可追溯性、数据溯源）
 指数据被充分记录、可验证且易于追溯至来源的程度 [WS96]。




# 3 IQ assessment

![](image/Pasted%20image%2020260124232419.png)

![](image/Pasted%20image%2020260124232434.png)



# 4 Cleansing tasks


Data Cleansing Tasks – Profiling
 Data source discovery 
     Metadata
     UDDI / matchmaking
 Schema discovery
     Schema matching and mapping
     Profiling for metadata (keys, foreign keys, data types, ...)
 Data discovery
     Column-level: Null-values, domains, patterns, value distributions / histograms 
     Table-level: Data mining, rules


Data Cleansing Tasks – Cleaning
 Extraction from sources
     Technical and syntactic obstacles
 Transformation
     Schematic obstacles
 Standardization
     Syntactic and semantic obstacles
 Duplicate detection
     Similarity functions 
     Algorithms
 Data fusion / consolidation 
     Semantic obstacles
 Loading into warehouse / presenting to user


Integration and Cleansing: ETL
![](image/Pasted%20image%2020260124232733.png)



Human Interaction is Needed
 Components to implement
     Wrappers for technical heterogeneity
     Schema integration based on correspondences 
     Similarity measure for schema elements
     Similarity measure for records
 Knobs to turn
     Thresholds for similarity measures
      Partition size / window size
 Expert guidance
     Rule selection / rule specification 
     Schema matching
     Duplicate detection
     Data fusion


# 5 IQ anecdotes

轶事，趣闻；传闻