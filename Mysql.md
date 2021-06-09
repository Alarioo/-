# 目录
- [目录](#目录)
- [函数](#函数)
  - [1.排名函数](#1排名函数)
- [Tips](#tips)
***
# 函数
## 1.排名函数
语法:函数( ) Over (Partition by 分组字段 Order by 排序字段 Desc/asc) as 排序后字段名  
- ROW_NUMBER()  
连续排名且不存在排名相同情况

|名词  |分数  |
|---------|---------|
|1     |    100     |
|2     |    100  |
|3     |      99   |
- rank()  
允许存在排名相同情况且名次不一定连续

|名词  |分数  |
|---------|---------|
|1     |    100     |
|1     |    100  |
|3     |      99   |
- DENSE_RANK()  
允许存在排名相同情况且名次连续

|名词  |分数  |
|---------|---------|
|1     |    100     |
|1     |    100  |
|2     |      99   |
- NTILE(i)  
  将有序分区中的行分发到指定数目i的组中
    
LeetcodeSql:https://leetcode-cn.com/problems/department-highest-salary/submissions/  
```
SELECT 
    t.Name AS Department,t.Employee ,t.Salary 
    FROM (  
        SELECT 
            d.Name,
            e.Name AS Employee,  
            e.Salary, 
            RANK() OVER (Partition BY e.DepartmentId ORDER BY Salary DESC) AS r
        FROM Employee AS e, Department d  WHERE e.DepartmentId=d.Id  
        ) AS t 
    WHERE t.r=1
```
参考链接:https://blog.csdn.net/shaiguchun9503/article/details/82349050  
# Tips
- where条件中生成的表不用赋名(As)  
  ```
    SELECT 
        DISTINCT Num AS ConsecutiveNums 
    FROM LOGS 
    WHERE (Id+1, Num) IN (SELECT * FROM LOGS) 
          AND (Id+2, Num) IN (SELECT * FROM LOGS)
  ```