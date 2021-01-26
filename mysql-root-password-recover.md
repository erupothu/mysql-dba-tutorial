

### Linux Machin open terminal
> sudo service mysql stop
> sudo mysqld_safe --skip-grant-tables --skip-syslog --skip-networking

- login to mysql without password
> mysql -u root
- change password
> UPDATE mysql.user SET authentication_string=PASSWORD('password') WHERE User='root';
> FLUSH PRIVILEGES;
- quit mysql safe mode
> mysqladmin shutdown
> sudo service mysql start

-- reset password
> UPDATE mysql.user SET Password=PASSWORD('[password]') WHERE User='[username]';
> FLUSH PRIVILEGES;
