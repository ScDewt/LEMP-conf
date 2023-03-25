# Install
```bash
sudo apt-get install mysql-server
sudo mysql
````

Come up with a password of sufficient complexity and do not lose it
```bash
mysql> ALTER USER 'root'@'localhost' IDENTIFIED WITH mysql_native_password BY 'YOUR_PASSWORD';
mysql> exit
````

```bash
sudo mysql_secure_installation
````

# Create user and db to site
```mysql
-- create common user
CREATE USER 'yourUserName'@'localhost' IDENTIFIED BY 'yourPassword';
GRANT ALL ON *.* TO 'yourUserName'@'localhost';
flush privileges;

-- create specific user to specific db
CREATE DATABASE yourDbName;
CREATE USER 'yourUserName'@'localhost' IDENTIFIED BY 'yourPassword';
GRANT ALL ON yourDbName.* TO 'yourUserName'@'localhost';
flush privileges;
```

# Tuning

```bash
# key_buffer_size variable controls the amount of memory available for the MySQL index buffer. 
# The higher this value, the more memory available for indexes and the better the performance. 
# Typically, you would want to keep this value near 25 to 30 percent of the total available memory on the server.
key_buffer_size         = 128M
# Limit the largest possible packet that can be transmitted to or from a MySQL 8.0 server or client
max_allowed_packet      = 16M
# The thread_stack is the stack size for each thread. 
# If the stack size is too small, it limits the complexity of SQL statements, the recursion depth of 
#    stored procedures and other memory-consuming actions. 
# MySQL 5.7 defaults the stack size to 192KB on 32-bit platforms and 256KB on 64-bit systems. 
# MariaDB 10.2 adjusted this value several times. MariaDB 10.2.0 used 290KB, 10.2.1 used 291KB and 10.2.5 used 292KB.
thread_stack            = 512K
# The optimal value of thread_cache_size, at which Threads_created is kept at an acceptable level, 
# ranges from 8 to 100, depending on the load and amount of memory.
thread_cache_size       = 16
```


# Links
- https://github.com/Releem/awesome-mysql-performance
- https://habr.com/ru/post/555222/
- https://highload.today/query_cache_size-parametr-v-mysql/

