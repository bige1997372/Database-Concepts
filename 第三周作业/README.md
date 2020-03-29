### 题目一
#### 创建熟人表代码
```
creat table aquaintance
(friend1 varchar(10),
 friend2 varchar(20),
 class varchar(4));
 ```
#### 表截图
![image]()
### 题目二
#### 1.找出互不认识的⼈
```
with t as (
select f1 m from aquaintance
union 
select f2 from aquaintance
)
select t1.m m1, t2.m m2
from t t1, t t2
where t1.m<>t2.m
and not exists (select * from aquaintance where (f1=t1.m and f2=t2.m) or (f2=t1.m and f1=t2.m));
```
![image]()
#### 2.找出只在⼀个类别⾥出现的⼈。
```
with t as (
select f1 m,class from aquaintance
union 
select f2,class from aquaintance
)
select m from t t1 where not exists(select * from t where m=t1.m and class<>t1.class);
```
![image]()
#### 3.找出在所有类别⾥都有朋友的⼈。
```

```
![image]()
#### 4.找出每个类别⾥⾯朋友最多的⼈。
```
select class,fname
from (
        select class,f1 as fname,count(1) as num,
               row_number()over(partition by class order by count(1) desc) as ordr
        from ( select class,f1
               from   acquaintance
               union all
               select class,f2
               from   acquaintance
             ) a
        group by class,f1
     ) a
where ordr = 1;
select f2
from   acquaintance a
where  exists ( select 1
                from   acquaintance b
                where  a.f2 = b.f1
              )
```
![image]()
#### 5.找出在同⼀类别⾥⾯通过朋友⽽结识的其他朋友（朋友的朋友也是朋友）。 
#### 6.找出这样的⼈，通过他⽽结识的朋友对最多(p1和p2原本不相识，他们通过p3 结识，那么p3的连接度为1，找出连接度最⾼的⼈)。
```
with 
relation_temp as 
(
select f1,f2,class
from   acquaintance
union all
select f2,f1,class
from   acquaintance
),
relation as
(
select a.f1,
       a.f2 as mid,
       b.f2 
from   relation_temp a
       inner join relation_temp b
         on a.f2 = b.f1
)
select mid,count(1)/2 as relations --  连接度 只考虑直接认识
from (
        select *
        from   relation a
        where  a.f1 <> a.f2
     ) a
group by mid 
order by 2 desc
```
![image]()
#### 7.找出臭味相投的朋友，他们在所出现的所有类别⾥⾯都是朋友（并且不能有这 种情况，其中⼀个在某个类别⾥出现⽽另外⼀个不出现）
```
select f1,f2,class from aquaintance t1
where not exists (select * from aquaintance t2
where not exists(select * from aquaintance where class=t2.class 
and ((f1=t1.f1 and f2=t1.f2) or (f1=t1.f2 and f2=t1.f1))));
```
![image]()
