# Install NGINX

https://linuxconfig.org/how-to-install-nginx-on-linux

![03-how-to-install-nginx-on-linux.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/5c79b53d-c01d-47d7-a915-c2897cb781df/bc78b398-f633-4634-b80f-12411ec0554c/03-how-to-install-nginx-on-linux.png)

NGINX is one of the most popular web server suites deployed across the internet. It’s efficient, versatile, and works well on pretty much any [Linux distribution](https://linuxconfig.org/how-to-choose-the-best-linux-distro). Whether you need a local server for testing, or want to host a website for the masses, NGINX is easy to set up. It can also be used as a [reverse proxy server](https://linuxconfig.org/how-to-setup-nginx-reverse-proxy).

In this guide, we’ll be going through the step by step instructions to install NGINX on a variety of Linux distributions. We’ll also go over some basic usage commands, like how to start and stop the service. Keep reading to get NGINX setup on your own [Linux system](https://linuxconfig.org/linux-download).

NGINX is available in the official repositories of all Linux distributions. You can use the following commands to install NGINX on whichever distribution you’re running, by using the system’s [package manager](https://linuxconfig.org/comparison-of-major-linux-package-management-systems). After NGINX is installed, we’ll show you some basic commands that can help you manage the process.

### Install NGINX on Debian, Ubuntu, and Linux Mint

Open a terminal and use the following commands to install NGINX on [Debian](https://linuxconfig.org/debian-linux-download), [Ubuntu](https://linuxconfig.org/ubuntu-linux-download), [Linux Mint](https://linuxconfig.org/linux-mint-download), [Kali](https://linuxconfig.org/kali-linux-download), and other Debian or Ubuntu derivatives.

```bash
$ sudo apt update
$ sudo apt install nginx

```

### Install NGINX on Fedora, CentOS, and Red Hat

Open a terminal and use the following commands to install NGINX on [Fedora](https://linuxconfig.org/fedora-linux-download), [CentOS](https://linuxconfig.org/centos-linux-download), [Red Hat](https://linuxconfig.org/red-hat-linux-download), and other Fedora or Red Hat derivatives.

```bash
$ sudo dnf upgrade
$ sudo dnf install nginx

```

### Install NGINX on Arch Linux and Manjaro

Open a terminal and use the following commands to install NGINX on [Arch Linux](https://linuxconfig.org/arch-linux-download), [Manjaro](https://linuxconfig.org/manjaro-linux-download), and other Arch derivatives.

```bash
$ sudo pacman -Syu
$ sudo pacman -S nginx

```

## Manage NGINX

Most Linux distributions, including all from the previous section, will use [systemd](https://linuxconfig.org/how-to-use-systemctl-to-list-services-on-systemd-linux) to manage the NGINX service. Use the following commands to manage it on your system.

Check the status of NGINX (i.e. see if it’s running):

```bash
$ systemctl status nginx

```

Checking the status NGINX service

Start or stop NGINX:

```bash
$ sudo systemctl start nginx
AND
$ sudo systemctl stop nginx

```

Enable or disable NGINX from starting automatically upon system boot:

```bash
$ sudo systemctl enable nginx
AND
$ sudo systemctl disable nginx

```

[Reload or restart NGINX](https://linuxconfig.org/how-to-restart-nginx-on-linux) – reload will just reload configuration files, whereas restart will restart the service entirely:

```bash
$ sudo systemctl reload nginx
AND
$ sudo systemctl restart nginx

```

Check the NGINX configuration files for errors – especially helpful before committing changes in a production environment:

```bash
$ sudo nginx -t

```

## Closing Thoughts

In this tutorial, we saw how to install NGINX on a variety of popular Linux distributions. We also learned how to manage the service with systemd, and check the configuration files for syntax errors. These instructions should be enough to get the software up and running. You can continue with our other guides to setup NGINX as a web server or reverse proxy server.