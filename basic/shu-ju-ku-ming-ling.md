---
description: 各种常见数据库的基本命令
---

# ✔️ 数据库命令

## PostgreSQL

```bash
# 查版本
SELECT version();
# 查当前用户
SELECT user;
SELECT current_user;
SELECT session_user;
SELECT usename FROM pg_user;
SELECT getpgusername();
# 列出用户
SELECT usename FROM pg_user;
# 列出密码哈希
SELECT usename, passwd FROM pg_shadow;
# 查数据库管理员账户
SELECT usename FROM pg_user WHERE usesuper IS TRUE;
# 查权限
SELECT usename, usecreatedb, usesuper, usecatupd FROM pg_user;
# 查当前账户是否是超级用户
SHOW is_superuser; 
SELECT current_setting('is_superuser');
SELECT usesuper FROM pg_user WHERE usename = CURRENT_USER;
# 查数据库名
SELECT current_database();
# 列出数据库
SELECT datname FROM pg_database;
# 列出table
SELECT table_name FROM information_schema.tables;
# 列出列
SELECT column_name FROM information_schema.columns WHERE table_name='data_table';
# 
```









