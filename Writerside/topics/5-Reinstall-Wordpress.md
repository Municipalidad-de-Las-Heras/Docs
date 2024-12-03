# 5. Reinstall Wordpress

## Step 1: Make a Backup (Optional but Recommended)
Before making any changes, it is recommended to make a backup of your current installation, just in case you need to recover something later.

```bash
tar -czvf wordpress_backup.tar.gz /var/www/html/wordpress
mysqldump -u wordpress_user -p wordpress_db > backup_db.sql
```

## Step 2: Delete WordPress Files

Go to the folder where your WordPress installation is installed. Usually, it is located in `/var/www/html/wordpress` or a similar path.

```bash
cd /var/www/html
rm -rf wordpress/*
```

This command will delete all WordPress files. Make sure there are no files you want to keep.

## Step 3: Delete Database
If you want to delete the current database and create a new one, you can do so from the MariaDB console:

```bash
mysql -u root -p
```

Next, run the following commands:

```sql
DROP DATABASE wordpress_db;
CREATE DATABASE wordpress_db;
```

## Step 4: Download and Install WordPress

Download the latest version of WordPress:

```bash
cd /var/www/html/wordpress
wget https://wordpress.org/latest.tar.gz
tar -xzvf latest.tar.gz --strip-components=1
```

This will unzip WordPress directly into the `wordpress` folder.

## Step 5: Configure wp-config.php

Copy the sample configuration file and edit it:

```bash
cp wp-config-sample.php wp-config.php
nano wp-config.php
```

Configure the database details:

```bash
define('DB_NAME', 'wordpress_db');
define('DB_USER', 'wordpress_user');
define('DB_PASSWORD', 'password');
define('DB_HOST', 'localhost');
```

## Step 6: Set Permissions

Make sure the permissions for your files and folders are correct:

```bash
chown -R apache:apache /var/www/html/wordpress
find /var/www/html/wordpress -type d -exec chmod 755 {} \;
find /var/www/html/wordpress -type f -exec chmod 644 {} \;
```

## Step 7: Install WordPress

Now, open your browser and go to the URL of your WordPress installation (e.g. http://your_domain/wordpress). You should see the WordPress installation screen. Follow the instructions to complete the installation.





