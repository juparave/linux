# MySQL

### Create user

```sql
-- with plugin
CREATE USER 'username'@'host' IDENTIFIED WITH authentication_plugin BY 'password';
-- without plugin
CREATE USER 'username'@'host' IDENTIFIED BY 'password';
-- grant permissions
GRANT ALL PRIVILEGES ON database.* TO 'username'@'host';
```
