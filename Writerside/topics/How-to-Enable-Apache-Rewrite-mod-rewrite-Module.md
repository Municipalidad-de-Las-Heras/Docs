# How to Enable Apache Rewrite (mod_rewrite) Module

Apache’s mod_rewrite is one of the most powerful modules available for URL manipulation. With mod_rewrite, you can redirect URLs, rewrite URLs to make them cleaner, and much more. It is particularly useful for implementing SEO-friendly URL structures on your website. In this article, we will walk you through how to enable mod_rewrite in Apache on both Debian-based and RHEL-based systems.

## Step 1: Check if mod_rewrite is Already Enabled
   Before enabling mod_rewrite, it’s a good idea to check if it’s already active:

```bash
apache2ctl -M | grep rewrite
```

OR

```bash
httpd -M | grep rewrite
```

If you see `rewrite_module (shared)`, then mod_rewrite is already enabled.

## Step 2: Enabling mod_rewrite

Now enable the mod_rewrie module in Apache web server based on your operating system.

### On Debian-based Systems (like Ubuntu)

Use these steps on Debian-based systems like, Ubuntu, Debian, Linux Mint systems.

Install Apache (if not already installed):

```bash
sudo apt update
sudo apt install apache2
```

Enable mod_rewrite:

```bash
sudo a2enmod rewrite
```

Restart Apache to apply changes:

```bash
sudo systemctl restart apache2
```

### On RHEL-based Systems (like CentOS)

Use these steps on RHEL-based systems like Fedora, CentOS, Scientific Linux, Amazon Linux and RedHat systems.

Install Apache (if not already installed):

```bash
sudo yum install httpd
```

Enable mod_rewrite:

The module is typically enabled by default on RHEL-based systems. If not, you can manually load it by editing the Apache configuration. Edit the main Apache configuration file using a text editor like vi or nano:

```bash
sudo nano /etc/httpd/conf/httpd.conf
```

Then, ensure that the following line is present and not commented out:

```bash
LoadModule rewrite_module modules/mod_rewrite.so
```

Restart Apache to apply changes:

```bash
sudo systemctl restart httpd
```

## Step 3: Configuring .htaccess for mod_rewrite

For mod_rewrite rules to work from `.htaccess` files, you must ensure that the directory configurations allow for overrides.

In the Apache configuration file (usually `/etc/apache2/apache2.conf` on Debian-based systems or `/etc/httpd/conf/httpd.conf` on RHEL-based systems), find the section for your website’s document root and modify the `AllowOverride` directive:

```bash
<Directory /var/www/html>
    AllowOverride All
</Directory>
```

After making changes, always remember to restart Apache.

## Step 4: Testing mod_rewrite

To ensure that mod_rewrite is working correctly, you can set up a basic rule in an `.htaccess` file:


In your document root (e.g., `/var/www/html`), create or edit the `.htaccess` file:

```bash
nano /var/www/html/.htaccess
```

Add the following content:

```systemd
RewriteEngine On
RewriteRule ^hello\.html$ welcome.html [R=302,L]
```

Now, create a `welcome.html` file:

```bash
echo "Welcome, TecAdmin!" > /var/www/html/welcome.html
```

Accessing `http://your_server_ip/hello.html` should now redirect you to `http://your_server_ip/welcome.html`.

## Conclusion
Enabling and configuring mod_rewrite in Apache can greatly improve the flexibility and SEO-friendliness of your website URLs. Make sure to plan and test your rewrite rules carefully, as mistakes can result in inaccessible pages or infinite redirect loops.