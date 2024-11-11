# Install Apache Server

https://www.geeksforgeeks.org/install-apache-web-server-in-linux/

If you're looking to **install Apache on Linux**, this guide will walk you through the steps required for different distributions, including **Ubuntu**, **Fedora**, and **RHEL**. The **Apache web server** is a popular choice for hosting websites and applications, known for its reliability and flexibility. Whether you're new to **how to install** an **Apache server** or need specific instructions for **Apache installation on Fedora** or **RHEL**, we have you covered with step-by-step instructions for each platform.

## How to Install Apache Web Server in Linux?

### Step 1: Check your Linux distribution

Use the following command to check which [Linux distribution](https://www.geeksforgeeks.org/what-are-linux-distributions/) you are using.

```bash
grep -E '^(VERSION|NAME)=' /etc/os-release
```

![Checking Linux distribution (Fedora)](https://media.geeksforgeeks.org/wp-content/uploads/20240228225408/image_2024-02-28_225406517.png)

Checking Linux distribution (Fedora)

### Step 2: Update Your System

**1. On Ubuntu/Debian-based systems:**

```bash
sudo apt update && sudo apt upgrade
```

**2. On Fedora-based systems:**

```bash
sudo dnf update -y
```

**3. On RHEL-based systems:**

```bash
sudo yum update -y
```

![Updating system (fedora)](https://media.geeksforgeeks.org/wp-content/uploads/20240228225517/image_2024-02-28_225516436.png)

Updating system (fedora)

### Step 3: Install Apache Web Server

**1. On Ubuntu/Debian-based systems:**

```bash
sudo apt install apache2 -y
```

**2. On Fedora-based systems:**

```bash
sudo dnf install httpd -y
```

**3. On RHEL-based systems:**

```bash
sudo yum install httpd -y
```

![Installing Apache web server](https://media.geeksforgeeks.org/wp-content/uploads/20240228230227/installing_httpd.png)

Installing Apache web server

### Step 4: Enable the Services

**1. On Ubuntu/Debian-based systems:**

```bash
sudo systemctl enable apache2
```

**2. On Fedora-based systems:**

```bash
sudo systemctl enable httpd.service
```

**3. On RHEL-based systems:**

```bash
sudo systemctl enable httpd.service
```

![Starting services for the Apache Web Server](https://media.geeksforgeeks.org/wp-content/uploads/20240228234527/image_2024-02-28_234526618.png)

Starting services for the Apache Web Server

### Step 5: Test the Server by Hosting a Simple Website

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

Now you can see the locally hosted website on your **localhost.**

![Testing the website on the local server](https://media.geeksforgeeks.org/wp-content/uploads/20240228232700/image_2024-02-28_232700244.png)

Testing the website on the local server

If the above-mentioned steps are performed correctly, Apache Web Server will run successfully! However, If it doesn't work, then you can uninstall [Apache Web Server](https://www.geeksforgeeks.org/how-to-configure-an-apache-web-server/) and start the installation again.

## How to Uninstall Apache Web Server?

**1. On Ubuntu/Debian-based systems**

```bash
sudo apt remove apache2
```

**2. On Fedora-based systems**

```bash
sudo dnf remove httpd
```

**3. On RHEL-based systems**

```bash
sudo yum remove httpd
```

Hence we have successfully uninstalled Apache Web Server in Linux!

## Conclusion

Installing the **Apache Web Server** on **Linux systems** like Ubuntu, Fedora, and RHEL is an easy task if you adhere to the proper procedures. After installing Apache, you can utilize its robust capabilities to efficiently host and control your websites. It's essential to perform routine upkeep and updates to maintain the security and efficiency of your Apache server. This article equips you with the necessary understanding to install and **configure Apache on your Linux machine**, making it simpler to deploy and manage [**web applications**](https://www.geeksforgeeks.org/what-is-web-app/).