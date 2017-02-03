### Oracle
> 创建于 2017-01-26
#### SQL执行顺序
```sql
5. SELECT
1. FROM
2. WHERE
3. GROUP BY
4. HAVING
6. ORDER BY
```

#### SELECT 基础
1. 导入sql文件
```sql
-- D盘下得test.sql文件
@d:/test.sql;
```

2. 消除重复行
```sql
SELECT DISTINCT job FROM emp;
```
3. Oracle 数据内容区分大小写
4. 范围查询

  ```sql
SELECT * FROM emp WHERE sal >=1500 AND sal <=3000;
-- 对应的 BETWEEN ... AND 效率高
SELECT * FROM emp WHERE sal BETWEEN 1500 AND 3000;
  ```
5. 判断空值

 ```sql
 -- 判断comm不为空的行
SELECT * FROM emp WHERE comm IS NOT NULL;
-- 判断 comm是空的行
SELECT * FROM emp WHERE comm IS NULL;
 ```
6. IN操作符

 ```sql
 SELECT * FROM emp WHERE empno=7369 OR empno=7566 OR empno=7788 OR empno=9999;
 -- 性能好
 SELECT * FROM emp WHERE empno IN (7369, 7566, 7788, 9999);
 -- 不在这个范围的
 SELECT * FROM emp WHERE empno NOT IN (7369, 7566, 7788, 9999);
 -- NOT IN 范围中不能出现null，会出现无结果的现象
 ```

7. 数据排序

 ```sql
-- ASC默认不写升序 ， DESC降序
-- 按工资由高到低进行排序
SELECT * FROM emp ORDER BY sal DESC;
-- 多个条件进行排序, order by 执行顺序在最后面，可以使用前面的别名
SELECT * FROM emp ORDER BY sal DESC , hiredate ASC;
 ```

8. 模糊查询

 ```sql
 -- 名字不存在字母 R的记录
 SELECT * FROM emp WHERE ename NOT LIKE '%R%';
 ```
9. 字符串函数
  * 大小写函数
```sql
-- 转小写 、 转大写
SELECT LOWER('Hello'), UPPER('Hello') FROM dual;
-- 实际应用
SELECT * FROM emp WHERE ename = UPPER('smith');
```
 * 首字母大写函数
 ```sql
 -- 除了首字母变为大写之外，其余字母都是小写 Helloworld
 SELECT INITCAP('HelloWorld') FROM dual;
 -- 可以在字段上使用函数
 SELECT INITCAP(ename) FROM emp;
 ```
 * 长度函数
 ```sql
 SELECT LENGTH(ename) FROM emp;
 -- 查找员工名字长度5的记录
 SELECT * FROM emp WHERE LENGTH(ename)=5;
 ```
 * 字符串替换
 ```sql
 -- 将所有雇员姓名之中的字母A替换为 _
 SELECT REPLACE(ename, UPPER('a'), '_') FROM emp;
 -- 消除空格
 SELECT REPLACE('hello world', ' ', '') FROM dual;
 ```
 * 截取字符串
 ```sql
 -- 从指定位置截取到字符串末尾, 截取 nihao
 SELECT SUBSTR('helloworldnihao', 11) FROM dual;
 -- 截取指定位置的内容, 截图world
 SELECT SUBSTR('helloworldnihao', 6, 5) FROM dual;
 -- 截取雇员名字的后三位字符，只有Oracle支持负索引
 SELECT ename, SUBSTR(ename, -3) FROM emp;
 ```
10. 数值函数
 * 四舍五入
 ```sql
 SELECT ROUND(78915.67823), ROUND(78915.67823, 2), ROUND(78915.67823, -2), ROUND(78985.67823, -2), ROUND(-15.65) FROM dual;   
 -- 结果依次是 78916    78915.68   78900   79000   -16
 ```
 * 截取小数, 不进位
 ```sql
 SELECT TRUNC(78915.67823), TRUNC(78915.67823, 2), TRUNC(78915.67823, -2), TRUNC(78985.67823, -2), TRUNC(-15.65) FROM dual;
 -- 结果依次是 78915   78915.67   78900   78900   -15
 ```
 * 求模
 ```sql
 -- 结果是1
 SELECT MOD(10, 3) FROM dual;
 ```
11. 日期函数（Oracle特色）
 * 时间戳
 ```sql
 -- 时间， 时间戳
 SELECT SYSDATE, SYSTIMESTAMP FROM dual;
 ```
 * 月份函数
 ```sql
 -- 计算员工雇佣日期到现在的年份
 SELECT ename, hiredate, TRUNC(MONTHS_BETWEEN(SYSDATE, hiredate)/12) years FROM emp;
 -- 增加四个月
 SELECT ADD_MONTHS(SYSDATE, 4) FROM dual;
 -- 查询雇佣所在月倒数第二天雇佣的雇员信息 LAST_DAY() 所在月份的最后一天
 SELECT ename, hiredate, LAST_DAY(hiredate), LAST_DAY(hiredate)-2 FROM emp WHERE LAST_DAY(hiredate)-2=hiredate;
 -- 计算下一个周一
 SELECT NEXT_DAY(SYSDATE, 'Monday') FROM dual;
 ```
12. 转换函数
 * 转字符串函数
 ```sql
 -- 时间转字符串
 SELECT TO_CHAR(SYSDATE, 'yyyy-mm-dd') FROM dual;
 SELECT TO_CHAR(SYSDATE, 'yyyy-mm-dd hh24:mi:ss') FROM dual;
 -- 转货币格式 L代表本地货币 ￥789234.....
 SELECT TO_CHAR(789234253245234, 'L999,999,999,999,999') FROM dual;
 ```
 * 转日期函数
```sql
SELECT TO_DATE('2016-09-28', 'yyyy-mm-dd') FROM dual;
```
* 转数字函数
```sql
-- 结果是 3
SELECT TO_NUMBER('1') + TO_NUMBER('2') FROM dual;
-- 一般这么使用
SELECT '1'+'2' FROM dual;
```
13. 通用函数（Oracle特色）
* 处理NULL函数
```sql
-- 处理NULL值，为null的属性变为0，不为NULL的默认不变。
SELECT ename, job, sal, comm, NVL(comm, 0), (sal+NVL(comm, 0))*12 income FROM emp;
```
* 多数值判断函数
```sql
SELECT ename, job,
DECODE(job, 'CLERK', '办事员', 'SALESMAN', '销售', 'MANAGER', '经理', 'ANALYST', '分析师', 'PRESIDENT', '董事长') jobchina
FROM emp;
```
#### 复杂查询
>  确定要使用的数据表， 确定已知的关联字段。
1. 消除笛卡儿积
```sql
SELECT * FROM emp, dept WHERE emp.deptno = dept.deptno;
```
2. 内连接

只显示满足条件的属于内连接。
3. 外连接

* 左外连接
```sql
SELECT * FROM emp, dept WHERE emp.deptno = dept.deptno(+);
```
* 右外连接
```sql
SELECT * FROM emp, dept WHERE emp.deptno(+) = dept.deptno;
```
* 全外连接
* 标准SQL
```sql
-- 交叉连接（笛卡儿积）相当于不加WHERE子句的多表查询
SELECT * FROM emp CROSS JOIN dept;
-- 自然连接 根据关联字段名匹配，相当于内连接
SELECT * FROM emp NATURAL JOIN dept;
-- 自然连接 字段名不一致
SELECT * FROM emp JOIN dept USING(deptno);
-- 自然连接 自己指定条件
SELECT * FROM emp e JOIN dept d ON(e.deptno = d.deptno);
-- 左外连接
SELECT * FROM emp e LEFT OUTER JOIN dept d ON(e.deptno = d.deptno);
-- 右外连接
SELECT * FROM emp e RIGHT OUTER JOIN dept d ON(e.deptno = d.deptno);
-- 全外连接
SELECT * FROM emp e FULL OUTER JOIN dept d ON(e.deptno = d.deptno);
```
4. 数据集合操作(数据结构必须相同)
* 取消重复元素
```sql
SELECT * FROM emp
UNION
SELECT * FROM emp WHERE deptno = 10;
```
* 保留所有查询的元素 并集
```sql
SELECT * FROM emp
UNION ALL
SELECT * FROM emp WHERE deptno = 10;
```
* 只保留相同元素 交集
```sql
SELECT * FROM emp
INTERSECT
SELECT * FROM emp WHERE deptno = 10;
```
* 只保留不同元素 差集
```sql
SELECT * FROM emp
MINUS
SELECT * FROM emp WHERE deptno = 10;
```
#### 统计函数
```sql
-- 只有COUNT()函数在空表时会返回数据0，其余都是null，不显示。
SELECT COUNT(ename) 人数, TRUNC(AVG(sal),2) 员工平均工资, SUM(sal) 每月总支出 , MAX(sal) 最大工资, MIN(sal) 最小工资 FROM emp;
```
#### 分组查询
> 前提有重复数据
```sql
-- 显示按部门的人数，按升序进行排序
SELECT deptno, COUNT(deptno) 人数 FROM emp GROUP BY deptno ORDER BY deptno;
```
**注意：**
* 如果不使用GROUP BY子句，那么SELECT子句中只允许出现统计函数，其他任何字段不允许出现。
* 如果查询中使用了GROUP BY子句，那么SELECT 子句中只允许出现分组字段、统计函数，其他任何字段都不能出现。
* 统计函数允许嵌套，但是嵌套之后的SELECT子句里面只允许出现嵌套函数，而不允许出现任何字段，包含分组字段
```sql
-- 错误使用，出现了分组字段
SELECT deptno, MAX(AVG(sal)) FROM emp GROUP BY deptno;
```
#### 多表分组查询
```sql
SELECT d.dname, COUNT(e.empno), AVG(e.sal)
FROM emp e, dept d
WHERE e.deptno(+) = d.deptno
GROUP BY d.dname;
-- 查询每个部门的编号、名称、位置、部门人数、平均工资
-- 分析思路 1 先把表确定出来，然后把用到函数的先换成字段
SELECT d.deptno, d.dname, d.loc, e.ename, e.sal
FROM emp e, dept d
WHERE e.deptno(+) = d.deptno;
-- 分析思路 2 发现三个字段都重复，可以按三个字段分组, 并且把不是分组字段的字段换成函数
SELECT d.deptno, d.dname, d.loc, COUNT(e.ename), AVG(e.sal)
FROM emp e, dept d
WHERE e.deptno(+) = d.deptno
GROUP BY d.deptno, d.dname, d.loc;
```
* HAVING子句

> 必须和GROUP BY一起出现，不能单独出现。
针对分组后的数据进行筛选
```sql
SELECT job, AVG(sal)
FROM emp
GROUP BY job
HAVING AVG(sal) > 2000;
```
#### 子查询

* WHERE子句
> 子查询返回单行单列

```sql
-- 子查询返回单行单列， WHERE子句中不能出现统计函数
-- 查询公司工资最低的雇员信息
SELECT *
FROM emp
WHERE sal = (SELECT MIN(sal) FROM emp);
```
> 返回单行多列
```sql
-- 查询出与 scott工资相同，职位相同的所有雇员信息
SELECT *
FROM emp
WHERE (sal, job) = (SELECT sal, job FROM emp WHERE ename = 'SCOTT') AND ename != 'SCOTT';
```

> 子查询返回多行单列
```sql
-- 找出雇员工资和经理工资一样的员工 IN | NOT IN 不能有空行
SELECT *
FROM emp
WHERE sal IN (SELECT sal FROM emp WHERE job = 'MANAGER') ;

-- =ANY 功能和IN相同
SELECT *
FROM emp
WHERE sal =ANY (SELECT sal FROM emp WHERE job = 'MANAGER') ;

-- >ANY 比最小的内容要大
SELECT *
FROM emp
WHERE sal >ANY (SELECT sal FROM emp WHERE job = 'MANAGER') ;

-- <ANY 比最大的内容还要小的结果
SELECT *
FROM emp
WHERE sal <ANY (SELECT sal FROM emp WHERE job = 'MANAGER') ;

-- >ALL 比子查询中的结果值都要大
SELECT *
FROM emp
WHERE sal >ALL (SELECT sal FROM emp WHERE job = 'MANAGER') ;

-- <ALL 比子查询中结果值都要小
SELECT *
FROM emp
WHERE sal <ALL (SELECT sal FROM emp WHERE job = 'MANAGER') ;

-- exists （只要有行，不管是什么表或结果）只要子查询结果不为空，那么都表示满足
SELECT *
FROM emp
WHERE exists (SELECT comm FROM emp WHERE job = '不存在');
```
* HAVING子句
> 子查询返回单行单列，而且需要统计函数过滤
* FROM子句
> 子查询返回的多行多列
```sql
-- 查询各部门的平均工资，和人数 子查询性能高
SELECT d.deptno, d.dname, d.loc, temp.count, temp.avg
FROM dept d, (
	SELECT deptno, COUNT(empno) count, AVG(sal) avg
	FROM emp
	GROUP BY deptno ) temp
WHERE d.deptno = temp.deptno(+);

-- 多表查询性能差
SELECT d.deptno, d.dname, d.loc, COUNT(e.deptno), AVG(e.sal)
FROM emp e, dept d
WHERE e.deptno(+) = d.deptno
GROUP BY d.deptno, d.dname, d.loc

```
* SELECT子句
> 一般返回单行单列

#### 数据更新
- 复制表
```sql
CREATE TABLE myemp AS SELECT * FROM emp;
```
- 添加
```sql
INSERT INTO 表名称 [字段名, 字段名,...] VALUES (数据, 数据,...)
```
- 修改
```sql
UPDATE 表名称 SET 字段 = 内容, 字段 = 内容, ...[WHERE 条件]
```
- 删除
```sql
DELETE FROM 表名称 [WHERE 删除条件]
```

#### 数据伪列
* ROWNUM 行号
```sql
SELECT ROWNUM, empno, ename, job FROM myemp;

-- 只能查询第一行， 不能查询其他行
SELECT * FROM emp
WHERE deptno = 10 AND ROWNUM = 1;

-- 查询前10条记录
SELECT ROWNUM, empno, ename, job FROM myemp
WHERE ROWNUM <= 10;

-- 分页查询 6 ~ 10条记录
SELECT *
FROM (
	SELECT ROWNUM rn, empno, ename FROM emp WHERE ROWNUM <= 10
) temp
WHERE temp.rn > 5;

```
* 分页查询
```sql
-- currentPage 当前所在页； lineSize 每页显示的数据行
SELECT *
FROM (
	SELECT ROWNUM rn, 列,... FROM 表名称
	WHERE ROWNUM <= currentPage * lineSize
) temp
WHERE temp.rn > (currentPage-1)*lineSize;
```
* ROWID

数据列的地址

#### 常用数据类型
* 字符串

VARCHAER2 200个字以内使用
* 数字

NUMBER表示， 小数用 NUMBER(m, n), 其中n为小数位，m-n是整数位。

整数：可以用INT

小数：可以用FLOAT
* 日期

使用DATE

* 大文本数据

使用CLOB描述，最多可以存储4GB的文字信息

* 大对象数据

使用BLOB，保存图片、音乐、电源等二进制数据。最多可以保存4GB

#### 序列
```sql
-- 创建序列
CREATE SEQUENCE mque;
-- 查看序列
SELECT * FROM user_sequences;

-- 创建表并使用序列
CREATE TABLE mytab(
	id NUMBER,
	name VARCHAR2(50),
	CONSTRAINT pk_id PRIMARY KEY(id)
);
-- 使用序列生成自增长id
INSERT INTO mytab(id,name)VALUES(MYSEQ.nextval, 'zhangsan');

-- 删除序列
DROP SEQUENCE mque;

-- 设置序列的初始值
ALTER SEQUENCE 序列名 INCREMENT BY 100000;
```
