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

### Open server for remote access

Create a remote user

```sql
> CREATE USER 'user'@'%' IDENTIFIED BY 'yourpassword';
> GRANT ALL ON *.* TO 'user'@'%';
> FLUSH PRIVILEGES;
```

Edit my.cnf

    $ sudo vim /usr/local/etc/my.cnf

Then edit bind-address to 0.0.0.0

```ini
[mysqld]
bind-address = 0.0.0.0
mysqlx-bind-address = 127.0.0.1
```

```

```
