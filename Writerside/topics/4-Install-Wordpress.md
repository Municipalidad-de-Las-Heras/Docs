# 4. Install WordPress

## Step 1: Creating a WordPress Database

We will begin by creating the database for the WordPress installation, which is used to store all the files during and after the installation.

So, log in to the MariaDB database:

```bash
sudo mysql -u root -p
```

Once on the MariaDB shell, create the database and database user and grant all the privileges to the database user.

```bash
CREATE DATABASE wordpress_db;
GRANT ALL ON wordpress_db.* TO 'wordpress_user'@'localhost' IDENTIFIED BY 'StrongPassword';
```

Save the changes and exit the MariaDB prompt.

```bash
FLUSH PRIVILEGES;
exit;
```

![wordpress1.png](wordpress1.png)

## Step 2: Download and Install WordPress in RHEL

With the WordPress database in place, the next course of action is to download and configure WordPress. At the time of publishing this guide, the latest WordPress version is 5.9.1.

To download WordPress, use the wget command to download the binary file from the official site.

```bash
wget https://wordpress.org/latest.tar.gz
```

![wordpress2.png](wordpress2.png)

Next, extract the tarball file:

```bash
tar -xvf latest.tar.gz
```

Next, we are going to copy the wp-config-sample.php file to wp-config.php from where WordPress derives its base configuration. To do that run:

```bash
cp wordpress/wp-config-sample.php wordpress/wp-config.php
```

Next, edit the wp-config.php file.

```bash
nano wordpress/wp-config.php
```

Modify the values to correspond to your database name, database user, and password as indicated in the image shown.

![wordpress3.png](wordpress3.png)

Save the changes and exit the configuration file.

Next, copy the WordPress directory to the document root:

```bash
sudo cp -R wordpress /var/www/html/
```

Be sure they assign the necessary directory ownership and permissions as follows:

```bash
sudo chown -R apache:apache /var/www/html/wordpress
sudo chcon -t httpd_sys_rw_content_t /var/www/html/wordpress -R
sudo chmod -Rf 775  /var/www/html
```

## Step 3: Create Apache WordPress VirtualHost File
We also need to create a configuration file for WordPress in order to point client requests to the WordPress directory. We will create the configuration file as shown:

```bash
sudo nano /etc/httpd/conf.d/wordpress.conf
```

Copy and paste the lines below to the configuration file.

```apache
<VirtualHost *:80>
ServerAdmin admin@localhost
DocumentRoot /var/www/html/wordpress

<Directory "/var/www/html/wordpress">
Options Indexes FollowSymLinks
AllowOverride all
Require all granted
</Directory>

ErrorLog /var/log/httpd/wordpress_error.log
CustomLog /var/log/httpd/wordpress_access.log common
</VirtualHost>
```

Save and exit the configuration file.

To apply the changes, restart Apache.

```bash
sudo systemctl restart httpd
```

## Step 4: Configure SELinux for WordPress
In most cases, RHEL 8 comes with SELinux enabled. This can be a hindrance, especially during the installation of web applications. As such, we need to configure the right SELinux context to the `/var/www/html/wordpress` directory.

```bash
sudo semanage fcontext -a -t httpd_sys_rw_content_t "/var/www/html/wordpress(/.*)?"
```

For the changes to come into effect, execute:

```bash
sudo restorecon -Rv /var/www/html/wordpress
```

Then reboot your system.

**_NOTE:_ Before you reboot, ensure that Apache and MariaDB services are enabled so that they can start automatically on boot.**

```bash
sudo systemctl enable httpd
sudo systemctl enable mariadb
```

By default, SELinux denies communications to other pages on the network. To solve this, you have to change some SELinux policies for the Apache Server.

```bash
sudo setsebool -P httpd_can_network_connect 1
```

Now SELinux will permit WordPress to make outgoing network connections to check for updates and install plugins.

To ensure that WordPress does not block the network connection, we will add the following line to the `wp-config.php` file:

```bash
sudo nano /var/www/html/wordpress/wp-config.php
```

```php
define('WP_HTTP_BLOCK_EXTERNAL', false);
```

Then reboot your system.

## Step 5: Finalize WordPress Installation
The last step is to complete the installation from a web browser. Launch your browser and browse your server’s IP address:

```http
http://server-IP-address
```

On the first page, select your preferred installation language and click `Continue`.

![wordpress 4.png](wordpress 4.png)

In the next step, fill in your Site’s details.

![wordpress 5.png](wordpress 5.png)

Then scroll down and click `Install WordPress`.

![wordpress 6.png](wordpress 6.png)

And in flash, WordPress installation will be complete! To log in, click the `Login` button.

![image.png](wordpress 7.png)

On the login screen, provide the username and password and click `Log In`.

![wordpress 8.png](wordpress 8.png)

This ushers you to the WordPress dashboard as shown. From here, you can customize your website with rich and elegant themes and plugins.

![wordpress 9.png](wordpress 9.png)

And that is it! You have successfully installed WordPress on RHEL 9.

## Appendix 1: File Permissions

If you get any 403 Forbidden error, or any other permissions error, or if you have changed the location of the WordPress root folder, run these commands in the console:

```bash
sudo chgrp -R GROUP ./
sudo chown -R USER:GROUP ./
find ./ -type d -exec chmod 755 -R {} \;
find ./ -type f -exec chmod 644 {} \;
```

And this:

```bash
chmod 644 /var/www/html
chmod 755 /var/www/html
```

And the perrmission problem was resolved!

