

# sql练习50题解题思路

## 数据表介绍：

分为三个表：student表(s代表)、course表(c代表)、教师表(t代表)、sc学生课程分数表

### 学生表：

| sid  | sname | sage | ssex |
| ---- | ----- | ---- | ---- |
|      |       |      |      |
|      |       |      |      |
|      |       |      |      |

### 课程表：

| cid  | cname | tid  |
| ---- | ----- | ---- |
|      |       |      |
|      |       |      |

### 教师表：

| tid  | tname |
| ---- | ----- |
|      |       |
|      |       |

### 成绩表：

| sid  | cid  | score |
| ---- | ---- | ----- |
|      |      |       |
|      |      |       |

## 题目1：

- ***查询" 01 "课程比" 02 "课程成绩高的学生的信息及课程分数：***

**思路：**

1. 首先从course表中查询出01，02课程的sid，score信息：

   ```sql
   select score,sid from sc where cid=01;
   select score,sid from sc where cid=02;
   ```

2. 从上式结果集对student表左连接

   ```sql
   select s.sid,s.sname,s.sage from Student s right join (
   select c1.sid from 
   (select Cid,score,sid from sc where cid=01) as c1,
   (select Cid,score,sid from sc where cid=02) as c2 
   where c1.sid=c2.sid AND c1.score>c2.score
   ) sc on sc.sid=s.sid;
   ```

   

- ***查询同时存在" 01 "课程和" 02 "课程的情况(不存在时显示为 null )：***

  **思路**：

  1. 同时查出01课程和02课程的sid,cid;

     ```sql
     select sid as sid_1,cid as cid_1 from sc where cid=01
     
     select sid as sid_2,cid as cid_2 from sc where cid=02
     ```

     

  2. 根据sid相同这个条件来确定查询的数据，由于优先查询01，可以left join；

     ```sql
     select * from (select sid as sid_1,cid as cid_1 from sc where cid=01) a left join (select sid as sid_2,cid as cid_2 from sc where cid=02) b on a.sid_1=b.sid_2;
     ```

     

- ***查询同时存在01和02课程的情况：***

  **思路：**

  和上题区别：去除null项

  ```sql
  select * from (select sid as sid_1,cid as cid_1 from sc where cid=01) a, (select sid as sid_2,cid as cid_2 from sc where cid=02) b where a.sid_1=b.sid_2;
  ```

- ***查询选择了02课程但没有01课程的情况*：**

  查询02课程，再通过not in排除

  ```sql
  select sid,cid from sc where cid=02 and sid not in (select sid from sc where cid=01);
  ```

  

## 题目2：

- **查询平均成绩大于等于 60 分的同学的学生编号和学生姓名和平均成绩：**

  使用group by进行分组，常随着“每”关键字出现，例如每个部门的人数，可以用

  ```sql
  select id,count(*) from 表格 group by id;
  ```

  关于group by的知识点：

  1. group by先排序再分组；

  2. where后不能跟聚合函数（avg,count,sum等），使用having代替where进行筛选；因为聚合函数是结果集确定的条件下，进行列运算，而where是水平方向的筛选，结果集未确定；

  3. select后面的字段必须出现**在group by后面**，或者**被聚合函数包裹**，否则会因为违反报错sql_mode 的 only_full_group_by 报错

     ```
     which is not functionally dependent on columns in GROUP BY clause; this is incompatible with sql_mode=only_full_group_by
     ```

  现在可以开始解题：

  1. 先对sc表，通过group by sid对学生进行分组，然后通过having进行条件筛选：

     ```sql
     select sc.sid,AVG(sc.score) from sc group by sc.sid having AVG(sc.score)>=60;
     ```

  2. 通过sid得到名字，可以直接join student表：

     ```sql
     select student.Sname,sc.sid,AVG(sc.score) from sc LEFT JOIN student on sc.SId=student.SId group by sc.sid,student.sname having AVG(sc.score)>=60;
     ```

​	