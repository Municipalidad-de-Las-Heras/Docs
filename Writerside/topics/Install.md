# 1. Install Apache Server

## Step 1: Update Your System

**On RHEL-based systems:**

```bash
sudo dnf update -y
```

![Updating system (fedora)](https://media.geeksforgeeks.org/wp-content/uploads/20240228225517/image_2024-02-28_225516436.png)

Updating system (fedora)

## Step 2: Install Apache Web Server

On RHEL-based systems:

```bash
sudo dnf install httpd -y
```

![Installing Apache web server](https://media.geeksforgeeks.org/wp-content/uploads/20240228230227/installing_httpd.png)

Installing Apache web server

## Step 3: Enable the Services

**On RHEL-based systems:**

```bash
sudo systemctl enable httpd.service
```

![Starting services for the Apache Web Server](https://media.geeksforgeeks.org/wp-content/uploads/20240228234527/image_2024-02-28_234526618.png)

Starting services for the Apache Web Server

## Step 4: Test the Server by Hosting a Simple Website

First, we will create a directory for our test website using the following command.

```bash
sudo mkdir /var/www/html/test_website/
```

Now we will add index.html for our test website along with some testing code using the following command.

```bash
echo '<html><head><title>Example</title></head><body><h1>GFG</h1><p>This is a test.</p></body></html>' | sudo tee /var/www/html/test_website/index.html
```

Now we will add the configuration file using the following command

```bash
sudo echo '<VirtualHost *:80>
    ServerName web.testingserver.com
    DocumentRoot /var/www/html/website
    DirectoryIndex index.html
    ErrorLog /var/log/httpd/example.com_error.log
    CustomLog /var/log/httpd/example.com_requests.log combined
</VirtualHost>' > /etc/httpd/conf.d/web.conf
```

Once we create the required config file and test the website, we will need to own the Apache website directory for permissions.

We will use the **chown** and [**chmod** commands](https://www.geeksforgeeks.org/what-does-chmod-x-do-and-how-to-use-it/) as follows

```bash
sudo chown -R apache:apache /var/www/html/test_website
sudo chmod -R 755 /var/www/html/test_website
```

Now you can see the locally hosted website on your **localhost**.

![Testing the website on the local server](https://media.geeksforgeeks.org/wp-content/uploads/20240228232700/image_2024-02-28_232700244.png)

Testing the website on the local server

If the above-mentioned steps are performed correctly, Apache Web Server will run successfully! However, If it doesn't work, then you can uninstall Apache Web Server and start the installation again.

## Step 5: Add firewall rules

To make our pages available to public, we will have to edit our firewall rules to allow HTTP requests on our web server using the following commands.

```bash
# firewall-cmd --permanent --zone=public --add-service=http
# firewall-cmd --permanent --zone=public --add-service=https
# firewall-cmd --reload
```

## How to Uninstall Apache Web Server?

**On RHEL-based systems**

```bash
sudo yum remove httpd
```

Hence, we have successfully uninstalled Apache Web Server in Linux!