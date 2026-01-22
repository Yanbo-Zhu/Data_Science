

# 1 Usability of views


|问题|含义|关键点|
|---|---|---|
|**When are views usable?**|视图能否被用来重写查询？|表重叠、属性足够、谓词关系对|
|**When are views useful?**|视图使用后是否带来价值？|更快的物化视图优化、更多数据或属性（LAV）|

----


▪ Question 1: When are views usable?
“视图（view）可用”指的是：
一个查询是否能够用某个视图来重写（rewrite）或部分满足。
也就是说：
👉 视图能不能在查询处理中发挥作用？

视图能否用于重写某个查询，要检查：

- 表是否相关
- 属性是否够用
- 条件关系（弱/等价/强）

▪ Informally:
	▪ At least one relation should overlap with query
	▪ At least some attributes of the query should be there
	▪ Predicates are weaker or equal (equivalent rewriting)
	▪ Predicates are stronger (contained rewriting)

---


▪ Question 2: When are views useful?
	▪ When optimizing with MVs: Faster execution
	▪ During integration with LaV:
	▪ Additional tuples
	▪ Additional attributes
	
视图可用，不代表视图一定有“价值”。

视图有用意味着：  
👉 **使用视图可以提升性能或补充数据。**


1 
Materialized View = 结果提前算好存在数据库里。
用 MVs 优化：
- 查询更快
- 少算 join、grouping
📌 例子  
MV 已经预先算好了订单统计，查询时不用再做 expensive joins。

2 
在 LAV（Local-as-View）数据集成时有用
此处指在数据集成（Integration）场景下：  
全局 schema 的查询，用本地 views 来回答。

3
用途 1：提供额外的 tuples（额外的数据行）

如果视图包含了本地数据库中某些信息
→ 它可以给最终答案带来更多数据。

4
用途 2：提供额外的 attributes（额外的列）

如果不同视图里包含不同属性
→ 可以把多个视图合并来得到查询所需的全部属性。

例如：
View1: product_id, name
View2: product_id, price

两个视图组合才能回答：
SELECT name, price FROM ...


# 2 LaV – Example


```
Global Schema
▪ teaches(prof,course_id, sem, eval, univ) T
▪ attending(stud, course_id, sem) A
▪ supervises(prof, stud) S
```
- S（supervises）告诉我们：哪个教授监督哪个学生
- A（attending）告诉我们：哪个学生在哪个学期参加了什么课程
- T（teaches）告诉我们：教授在哪个学期教了哪个课程


```
Global Query
SELECT S.prof, S.stud, A.sem
FROM attending A, teaches T, supervises S
WHERE A.course_id = T.course_id
AND A.sem = T.sem
AND S.prof = T.prof
AND S.stud = A.stud
AND A.sem > „WS98“
```

- Find all professor with supervised students and semester since WS98 who have attended a course of the professor
- Remark: course_id is globally consistent


## 2.1 

```
Source 7:
CREATE VIEW StudProf AS
SELECT T.prof, A.stud, A.sem
FROM attending A, teaches T
WHERE A.course_id = T.course_id
AND A.sem = T.sem
AND A.sem >= „WS98“
```

```
Rewriting:
SELECT S.prof, S.stud, S.sem
FROM StudProf SP, supervises S
WHERE SP.prof = S.prof
AND SP.stud = S.stud
```
Caution: Rewriting still uses global relation S
Assumption here: Trivial view

解释它的逻辑：
- `StudProf` 已经替代了 `(attending A`, `teaches T)` 那部分 join
- 然而：
    - supervises S **没有对应的 view**
    - 因此 rewriting **仍然依赖 global relation S**
所以这里只能是 **partial rewriting**（部分重写），不是完整重写。

## 2.2 ##
Weaker condition
```
Source 8:
	CREATE VIEW StudProf2 AS
	SELECT T.prof, A.stud, A.sem
	FROM attending A, teaches T
	WHERE A.course_id = T.course_id
	AND A.sem = T.sem
	AND A.sem >= „WS97“
```


Rewriting
```
SELECT S.prof, S.stud, S.sem
FROM StudProf2 SP, supervises S
WHERE SP.prof = S.prof
AND SP.stud = S.stud
AND SP.sem >= „WS98
```

Rewriting is contained/equivalent.


## 2.3 ##

```
Source 9:
CREATE VIEW StudProf3 AS
SELECT T.prof, A.stud, A.sem
FROM attending A, teaches T
WHERE A.course_id = T.course_id
AND A.sem = T.sem
AND A.sem >= „WS99“    // Stronger Condition
```

Rewriting is contained but not equivalent
⇒ OK for integration, but inappropriate
for optimization with MVs

Rewriting
```
SELECT SP.prof, S.stud, S.sem
FROM StudProf3 SP, supervises S
WHERE SP.prof = S.prof
AND SP.stud = S.stud
```


Rewriting is contained but not equivalent
⇒ OK for integration, but inappropriate for optimization with MVs


## 2.4 

T.prof is missing

```
Source 10:
CREATE VIEW StudProf4 AS
SELECT A.stud, A.sem
FROM attending A, teaches T
WHERE A.course_id = T.course_id
AND A.sem = T.sem
AND A.sem >= „WS98“
```


```
Rewriting
SELECT S.prof, S.stud, S.sem
FROM StudProf4 SP, supervises S, teaches T
WHERE S.prof = T.prof
AND SP.stud = S.stud
```

Cumbersome: T has to be joined again

## 2.5 ##

A.sem = T.sem missing

```
Source 11:
CREATE VIEW StudProf5 AS
SELECT A.stud, A.sem, T.prof
FROM attending A, teaches T
WHERE A.course_id = T.course_id
AND A.sem >= „WS98“
```



```
Rewriting
SELECT S.prof, S.stud, A.sem
FROM StudProf5 S, teaches T, supervises S
WHERE S.sem = T.sem
AND S.prof = T.prof
AND S.stud = A.stud
```

Cumbersome: T has to be joined again. T, because T.sem of Source 11 is not exported.  就是说 
CREATE VIEW StudProf5 AS SELECT A.stud, A.sem, T.prof   中没有 Select  T.sem

## 2.6 总结 

▪ A view V is usable for an equivalent rewriting of query Q, if
	▪ Every relation in V corresponds to some relation in Q.
	▪ Conditions in Q (Joins and selections) on relations in V have to be either directly in V or Weaker - the constrained attributes in Q have to be exported.
▪ V projects no attributes out, which are needed in Q and are not available through other sources (other views and relations).
▪ Usefulness depends on DBMS and extension.
	▪ Indices, etc.
什么叫“view 对等重写是可用的（usable for _equivalent rewriting_）？”

1 
意思是：
> 你可以用这个 view 完整地替换 query 中涉及的关系，并得到与原 query **完全相同的结果**。

等价重写（equivalent rewriting）要求：
- 替换后结果必须和原始 query 的结果 100% 一样
- 不能丢 tuple，也不能多 tuple
所以必须检查 view 是否包含 query 所需的全部信息（属性、关系、条件）。



2

|名称|定义|特点|例子|
|---|---|---|---|
|Base Relation|实际物理表|存在数据，可直接查询|`Students`|
|Global Relation|数据集成全局表|逻辑表，可能虚拟|`Global_Student_Course`|
- Base relation → 物理的表
- Global relation → 逻辑的全局表（整合视角下的表）

2.1
**“Every relation in V corresponds to some relation in Q.”**

解释：  
View V 中出现的每个 base relation（或 global relation）必须在 query Q 里也出现。

否则：
- view 包含了 Q 不需要的东西 → 不能保证等价结果
- 可能引入额外 tuple 或错误的 join 语义

✔️ Example（可用）  
Q 用了 attending，teaches  
V 也只是 combining attending 和 teaches → OK

❌ Example（不可用）  
V 里面还有 enroll 或 course（Q 不需要）  
→ 引入额外信息，不再等价

**Base Relation（基础关系）**
- **定义**：数据库中最基本的表（table），不是视图（view）。
- **特点**：
    - 存在实际的物理数据
    - 可以直接存储和查询
    - 作为其他视图或查询的基础


**Global Relation（全局关系）**
- **定义**：在 **数据集成（data integration / global schema）** 中，用来表示 **整合后的全局 schema 中的表**。
- **特点**：
    - 它不一定在物理上存在（可能是虚拟的）
    - 是**全局 schema**的一部分，定义整个系统的逻辑视图
    - 查询通常写在全局关系上，然后通过视图映射到本地数据源（local sources）
- **例子**：  
    在一个集成系统中：
- 本地数据库有：
    - `DB1.Students`（学号、姓名）
    - `DB2.Enrollments`（学号、课程）
- 全局 schema 定义了： `Global_Student_Course(stud_id, name, course_id)`

# 3 基本概念

## 3.1 **Containment 类型**
1. **Q1 ⊆ Q2** → Q1 被 Q2 包含
2. **Q1 ≡ Q2** → Q1 和 Q2 等价（双向包含）
3. **Q1 ⊄ Q2** → 不包含，Q2 无法覆盖 Q1

**与 View Rewriting 的关系**

在 **Local-as-View (LAV)** 方法中：

- 想用视图 V 重写 query Q
- 需要检查 **containment**：
    - 每个视图提供的数据是否包含 query 需要的 tuple
    - 确定哪些视图可以组合成 query 的等价答案
> 也正是你之前看到的“containment tests”指数级问题来源。


## 3.2 Query relations

**Query relation**s 指 **一个查询（query）中使用到的表或关系**。

```
SELECT S.prof, S.stud, A.sem
FROM attending A, teaches T, supervises S
WHERE A.course_id = T.course_id
  AND A.sem = T.sem
  AND S.prof = T.prof
  AND S.stud = A.stud
  AND A.sem > 'WS98';

```

查询用到的表（relations）：
1. `attending A` → query relation    
2. `teaches T` → query relation
3. `supervises S` → query relation

> 所以这里一共有 **3 个 query relations**。

# 4 Bucket Algorithm (BA)


 

▪ Problem:
	▪ Exponential number of containment tests
	▪ Checking each containment is NPC
		▪ There are efficient algorithms
		▪ Combinations are usually not big (number of relations)

1.**组合爆炸（Exponential number of containment tests）**
- 当 query 有多张表（relations），要用多个视图来重写 query 时.  
- 每个 query relation 都可能对应多个 candidate view
- 所有可能组合的数量会 **呈指数增长**
- 检查每个组合是否能覆盖 query （containment test）非常耗时

> 举例：  
> Query 有 4 个 relation，每个 relation 有 5 个候选视图  
> → 组合数量 = 5⁴ = 625  
> → 随着关系数增加，组合数量指数级增长

2 Containment 检查是 NPC（NP-complete）
- 检查 view 能否覆盖 query 的条件（equivalence / containment）
- 理论上属于 NP-complete 问题
- 虽然有一些高效启发式算法（heuristics / practical algorithms）可以处理实际情况
- 并且通常 query 涉及的 relation 数量 **不大**，所以在实践中可行

----

▪ Idea for further improvement:
	▪ Reduction of number of combinations through smart candidate selection
	▪ Every relation of the Query receives a bucket.
	▪ Step 1: Add to each bucket all views that are usable for the relation
	▪ Step 2: Check all Query rewriting combinations with exactly one view per bucket

步骤 1：为每个 query relation 建立 bucket
- 每个 query relation 对应一个 bucket
- bucket 里放 **所有可用的视图（usable views）**，即这个 relation 可以用的候选视图


步骤 2：生成组合
- 每个组合取 **每个 bucket 中的一个视图**
- 组合数量 = ∏ (每个 bucket 中视图数)
- 检查这些组合是否能覆盖 query（containment test）

> 优势：
> - 不用检查所有可能的视图子集
> - 只考虑 **每个 relation 至少用一个视图** 的组合
> - 减少了指数级爆炸的规模


| 问题                              | 解决方案                                        |
| ------------------------------- | ------------------------------------------- |
| Query 有多张 relation → 所有视图组合指数增长 | 为每个 relation 建 bucket，只检查每个 bucket 选一个视图的组合 |
| Containment 检查 NP-complete      | 使用启发式算法或实际小规模 query 处理                      |
| 组合过多                            | 智能选择候选视图，减少不必要组合                            |


在 **view-based rewriting（基于视图的查询重写）** 中：
- 每个 query relation 可能对应多个 candidate view
- 为了生成重写组合，需要知道 **每个 query relation 的候选视图**
- 然后检查这些视图组合是否能覆盖整个 query

# 5 BA Exmaple 

```
Source 1:
CREATE VIEW V1 AS
SELECT A.stud, C.title, C.sem, C.course_id
FROM attending A, course C
WHERE A.course_id = C.course_id
AND C.course_id >= 500
AND A.sem >= WS98

Source 2:
CREATE VIEW V2 AS
SELECT A.stud, T.prof, A.sem, T.course_id
FROM attending A, teaches T
WHERE A.course_id = T.course_id
AND A.sem = T.sem

Source 3:
CREATE VIEW V3 AS
SELECT A.stud, A.course_id
FROM attending A
WHERE A.sem >= WS94


Source 4:
CREATE VIEW V4 AS
SELECT T.prof, C.course_id, C.title, A.sem
FROM attending A, course C, teaches T
WHERE A.course_id = C.course_id
AND A.course_id = T.course_id
AND T.sem = A.sem
AND A.sem >= WS97
```


```
V1 (stud, title, sem, course_id) :- A(stud,course_id,sem), C(course_id,title), course_id >= 500, sem >= WS98
V2 (stud, prof, sem, course_id) :- A(stud, course_id, sem), T(prof, course_id, sem)
V3 (stud, course_id) :- A(stud, course_id, sem), sem <= WS94
V4 (prof, course_id, title, sem) :- T(prof, course_id, sem), C(course_id, title), A(stud, course_id, sem), sem <=  WS97
```


```
Query Q:
SELECT A.stud, C.course_id, T.prof
FROM teaches T, attending A, course C
WHERE A.course_id = T.course_id
AND A.sem = T.sem
AND C.course_id = A.course_id
AND A.sem >= WS95 
AND course_id >= 300

Query Q:
Q (stud, course_id, prof) :- T(prof, course_id, sem), A(stud,course_id,sem), C(course_id, title), sem >= WS95, course_id >= 300
```

![[Pasted image 20251124203513.png]]

![[Pasted image 20251124203653.png]]

![[Pasted image 20251124203712.png]]


```
Query Q:
SELECT A.stud, C.course_id, T.prof
FROM teaches T, attending A, course C
WHERE A.course_id = T.course_id
AND A.sem = T.sem
AND C.course_id = A.course_id
AND A.sem >= WS95

Reformulated Query Q‘
V1,V2 unoin V2,V4:
SELECT V1.stud, V1.course_id, V2.prof
FROM V1, V2
WHERE V1.sem = V2.sem
AND V1.course_id = V2.course_id

UNION

SELECT V2.stud, V2.course_id, V2.prof
FROM V2, V4
WHERE V2.sem = V4.sem
AND V2.course_id = V4.course_id
AND V2.prof = V4.prof
AND V2.sem ≥ WS95
```


- Q‘ uses only views .
- Q‘ 属于  Q
- Q‘ is maximally contained in Q.


# 6 Bucket Algorithm Details


1 Create a bucket for each subgoal. Add views into buckets.
![[Pasted image 20251124203913.png]]


▪ A view is added to a bucket, if at least one subgoal of the view is “usable”. So:

For each bucket
▪ For each view:
	▪ For every subgoal of the view 
	▪ Check for ”usable“:
		▪ 1. Unifier: Mapping of all attributes in subgoal of Q on attributes of subgoals of V. I.e..: All query attributes have to appear and be exported.
		▪ 2. Compatibility: Predicates have to fit.


▪ For every view: Check the subgoals
▪ If at least one subgoal fits, add it to the bucket

Check for „appropriateness“:
1. Unifier: All query attributes have to appear
2. Compatibility: Predicates fit


## 6.1 Bucket 1: T (prof, course_id, sem)


![[Pasted image 20251124204241.png]]

![[Pasted image 20251124204318.png]]


![[Pasted image 20251124204327.png]]

![[Pasted image 20251124204336.png]]

## 6.2 Bucket 2: A(stud, course_id, sem)

![[Pasted image 20251124204528.png]]

![[Pasted image 20251124204603.png]]

![[Pasted image 20251124204611.png]]

![[Pasted image 20251124204617.png]]



## 6.3 Bucket 3: C(course_id, title)


![[Pasted image 20251124204626.png]]


![[Pasted image 20251124204632.png]]

![[Pasted image 20251124204638.png]]


![[Pasted image 20251124204645.png]]

![[Pasted image 20251124204712.png]]

## 6.4 Filtering Combinations

![[Pasted image 20251124204744.png]]


## 6.5 LaV – BA – Number of Combinations

39 Ziawasch Abedjan | FG D2IP | Data Integration– 07C Bucket Algorithm
▪ How many combinations?
▪ |B1| x |B2| x ...x |Bn| (n = |Q|)
▪ If m = Number of views: mn
▪ Important: Every combination delivers a potential susbset of the result.

A combination Q‘ = V1,...,Vk is a query rewriting of Q, if
▪ Q‘ 属于 Q
▪ (or Q‘, {additional predicates} 属于 Q)

![[Pasted image 20251124204821.png]]


## 6.6 

### 6.6.1 Contradiction Views 矛盾的 

![[Pasted image 20251124205229.png]]

```
Individual views are useful (with regard to the query), but incompatible: sem >= WS98 vs. sem =< WS97
```

### 6.6.2 Remove redundant joins
![[Pasted image 20251124205247.png]]


### 6.6.3 Find containment mapping
42 Ziawasch Abedjan | FG D2IP | Data Integration– 07C Bucket Algorithm
▪ Check Q  Q‘(stud, course_id, prof):-
V2(stud‘, prof, sem, course_id),
V1 (stud, title‘, sem, course_id),
V1 (stud‘, title, sem‘, course_id)

▪ How?
	▪ Q uses global relations, Q‘ uses views

▪ By „unfolding“:
Q‘ = Q‘‘(stud, course_id, prof):-
A(stud‘, course_id, sem), T(prof, course_id, sem),
A(stud,course_id,sem), C(course_id,title‘), course_id  500, sem  WS98

▪ And finding of a containment mapping

![[Pasted image 20251124205323.png]]




## 6.7 Final result: Combination of all combinations with UNION:


![[Pasted image 20251124205512.png]]






# 7 BA Analysis


▪ Completeness: Given Query Q and Views V1,...,Vm, BA finds a query rewriting Q‘ with views, if a rewriting exists?
▪ Proof:
	▪ According to [LMSS95] there is only a rewriting, if there is a rewriting of length|Q|.
		▪ Limitation: The query is not allowed to contain ≥,≤,<,>, predicates.
	▪ I.e., it is enough to check all combinations.
	▪ BA discards only combinations that are not usable

▪ Maximally contained
	▪ BA produces according to [LMSS95] maximally contained rewritings.


![[Pasted image 20251124205625.png]]

▪ **完备性（Completeness）**：给定查询 Q 和视图 V1,...,Vm，BA（Bucket Algorithm）是否能找到一个使用这些视图的查询重写 Q‘，如果这样的重写存在？  
▪ **证明**：  
 ▪ 根据 [LMSS95]，只有在存在长度为 |Q| 的重写时，才会存在重写。  
  ▪ **限制**：查询中不允许包含 ≥、≤、<、>、≠ 等谓词。  
 ▪ 即，只需要检查所有组合即可。  
 ▪ BA 仅会舍弃那些不可用的组合。

▪ **最大包含性（Maximally contained）**  
 ▪ 根据 [LMSS95]，BA 会生成**最大包含的重写**（maximally contained rewritings）。

