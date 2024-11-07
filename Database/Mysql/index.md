```sql
show grants for 'web'@'localhost';
grant create on snippetbox.* to 'web'@'localhost';
grant index on snippetbox.* to 'web'@'localhost';
```

mysql.server start

mysql.server stop

mysqld_safe

For killing the mysqld_safe you need to run mysqladmin -u root -p shutdown in another terminal or after hold CTRL + Z

ALTER USER 'root'@'localhost' IDENTIFIED BY 'yournewpassword';
