# Install
```bash
# FPM is minimally needed, the rest as needed on the project
sudo apt-get install php8.1-fpm php8.1-bcmath php8.1-bz2 php8.1-cli php8.1-common php8.1-curl php8.1-gmagick php8.1-mbstring php8.1-memcached php8.1-mysql php8.1-opcache php8.1-tidy php8.1-xml php8.1-xsl
```

Install memcached if needed (e.g. for php sessions)
```bash
sudo apt-get install memcached
sudo apt-get install libmemcached-tools
sudo systemctl start memcached
```

# Tuning fpm
```bash
# check the limit of open files for the user www-data (!!NOT from under root)
ulimit -a -u www-data
# if the limit is less than 4096, then we increase it
sudo vim /etc/security/limits.conf

# add to the end of file
www-data         soft    nofile          8000
www-data         hard    nofile          16000

# restart service
sudo systemctl reload-or-restart systemd-logind.service

# again check (shows soft limit)
ulimit -a -u www-data
```

## Change /etc/php/8.1/fpm/php-fpm.conf
```bash
error_log = /var/log/php/error.log
```

## Change /etc/php/8.1/fpm/pool.d/www.conf
```bash
# Ensure only localhost can connect to PHP-FPM
listen.allowed_clients = 127.0.0.1

# static mode for maximum performance
pm = static
# pm.max_children = Total RAM dedicated to the web server / Max child process size
# if mysql/memcached are installed on the server, then their memory consumption must be taken into account.
# I take half of the free memory for php and then I test it with a load and adjust the value
pm.max_children = 20
# Depends on memory leaks.
# If memory leaks are large, decrease the value
# If memory leaks are small, increase the value
pm.max_requests = 500

# set slowlog near errorlog
slowlog = /var/log/php/slow.log
# set slowlog timeout for requests to be logged
request_slowlog_timeout = 5s
# set timeout for requests to be terminated
request_terminate_timeout = 10s
# set max of open files
rlimit_files = 8000
```

## Change /etc/php/8.1/fpm/php.ini
```bash
# mat time for php scripts
max_execution_time = 10
# max time for php scripts to read input data
max_input_time = 10
# take it for you app 
memory_limit = 128M
# for production - do not show everything except critical errors
error_reporting = E_ALL & ~E_DEPRECATED & ~E_STRICT & ~E_WARNING & ~E_NOTICE
# for development - show everything except notices
error_reporting = E_ALL & ~E_STRICT & ~E_NOTICE

# take it for you app
upload_max_filesize = 5M
max_file_uploads = 5

# timeout (in seconds) for socket based streams (default: 60)
default_socket_timeout = 10

# speed up work sessions with memcached
session.save_handler = memcached
session.save_path = "localhost:11211"
```

# Links
- https://haydenjames.io/php-fpm-tuning-using-pm-static-max-performance/
- https://www.php.net/manual/en/install.fpm.configuration.php
- 

