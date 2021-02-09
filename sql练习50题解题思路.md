

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

- *查询" 01 "课程比" 02 "课程成绩高的学生的信息及课程分数：*

**思路：**

1. 首先从course表中查询出01，02课程的sid，score信息：

   ```
   select score,sid from sc where cid=01;
   select score,sid from sc where cid=02;
   ```

2. 从上式结果集对student表左连接

   ```
   select s.sid,s.sname,s.sage from Student s right join (
   select c1.sid from 
   (select Cid,score,sid from sc where cid=01) as c1,
   (select Cid,score,sid from sc where cid=02) as c2 
   where c1.sid=c2.sid AND c1.score>c2.score
   ) sc on sc.sid=s.sid;
   ```

   

- *查询同时存在" 01 "课程和" 02 "课程的情况：*