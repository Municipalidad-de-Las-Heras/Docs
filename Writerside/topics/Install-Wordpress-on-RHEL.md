# Install Wordpress on RHEL

## 1. Install Apache Server

[Bibliography](https://www.geeksforgeeks.org/install-apache-web-server-in-linux/)





## 3. Install MariaDB Server

MariaDB is a popular database server, used in many environments. The installation is simple and requires just a few steps as shown.

```bash
dnf install mariadb-server mariadb
```

Once the installation is complete, enable MariaDB (to start automatically upon system boot), start the web server and verify the status using the commands below.

```bash
systemctl enable mariadb
systemctl start mariadb
systemctl status mariadb
```

![mariadb_daemon.png](mariadb_daemon.png)

Finally, you will want to secure your MariaDB installation by issuing the following command.

```bash
mysql_secure_installation
```

You will be asked a few different questions regarding your MariaDB installation and how you would like to secure it. You can change the database root user password, disable the test database, disable anonymous users, and disable root login remotely.

Here is an example:

![mariadb_example.png](mariadb_example.png)

Once secured, you can connect to MySQL and review the existing databases on your database server by using the following command.

```bash
mysql -e "SHOW DATABASES;" -p
```
![mariadb2.png](mariadb2.png)



