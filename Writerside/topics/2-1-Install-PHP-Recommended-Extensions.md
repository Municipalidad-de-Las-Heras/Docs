# 2.1 Install PHP Recommended Extensions

## Install ImageMagick (Image Manipulation) Tool

To use the ImageMagick tool with PHP or Perl programming language, you will need to install ImageMagick with the Imagick PHP extension for PHP and ImageMagick-Perl extension for Perl.

Imagick is a simple PHP extension for creating and modifying images using the ImageMagick API program. There is a confusion in name, as people think that ImageMagick and Imagick both are the same, but you can use ImageMagick without Imagick extension but you need both installed on your machine to use and run it.

### Installing ImageMagick from Repository
First, install following prerequisite php-pear, php-devel and gcc packages to compile the Imagick PHP extension.

```bash
dnf install php-pear php-devel gcc 
```

Once you’ve installed php-pear, php-devel, and gcc packages, you may now install ImageMagick software for PHP and Perl support using dnf command.

```bash
dnf install ImageMagick ImageMagick-devel ImageMagick-perl
```

IMPORTANT: ImageMagick is not available in CentOS/RHEL 8, and it has been replaced with GraphicsMagick instead, which is a fork of ImageMagick.

To install GraphicsMagick on CentOS/RHEL 8, run the following command:

```bash
dnf install GraphicsMagick GraphicsMagick-devel GraphicsMagick-perl
```

Next, verify that ImageMagick has been installed on your system by checking its version.

```bash
convert --version
```

CentOS/RHEL 8 users, can run the following command to verify the version of GraphicsMagick installed on the system.

```bash
gm version
```

### Installing ImageMagick 7 from Source Code
To install ImageMagick from source, you need a proper development environment with a compiler and related development tools. If you don’t have the required packages on your system, install development tools as shown:

```bash
yum groupinstall 'Development Tools'
yum -y install bzip2-devel freetype-devel libjpeg-devel libpng-devel libtiff-devel giflib-devel zlib-devel ghostscript-devel djvulibre-devel libwmf-devel jasper-devel libtool-ltdl-devel libX11-devel libXext-devel libXt-devel lcms-devel libxml2-devel librsvg2-devel OpenEXR-devel php-devel
```

Now, download the latest version of the ImageMagick source code using the following wget command and extract it.

```bash
wget https://www.imagemagick.org/download/ImageMagick.tar.gz
tar xvzf ImageMagick.tar.gz
```

Configure and compile the ImageMagick source code. Depending on your server hardware specs, this may take some time to finish.

```bash
cd ImageMagick*
./configure
make
make install
```

Verify that the ImageMagick compile and install were successful.

```bash
magick -version
```

### Install Imagick PHP Extension

Next, compile the Imagick for PHP extension. To do, simply run the following ‘pecl‘ command. It will install ImageMagick and imagick PHP extension module `imagick.so` under `/usr/lib/php/modules` directory. If you are using a 64-bit system, the module directory path would be `/usr/lib64/php/modules`.

Note: It will ask you to provide Imagemagick installation prefix, simply hit enter to auto-detect.

```bash
pecl install imagick 
```

Now, add the `imagick.so` extension to `/etc/php.ini` file.

```bash
echo extension=imagick.so >> /etc/php.ini
```

Next, restart Apache webserver.

```bash
systemctl restart httpd
```

Verify the Imagick PHP extension by running the following command. You will see the Imagick extension similar to below.

```bash
php -m | grep imagick
```

### Install GMagick PHP Extension

Run the following commands to compile and install GMagick PHP Extension.

```bash
cd /usr/local/src
wget https://pecl.php.net/get/gmagick
tar xfvz gmagick
cd gmagick-*
phpize
./configure
make
make install
```

Now, add the `gmagick.so` extension to `/etc/php.ini` file.

```bash
echo extension=gmagick.so >> /etc/php.ini
```

Next, restart the Apache webserver.

```bash
systemctl restart httpd
```

Verify gmagick PHP extension by running the following command.

```bash
php -m | grep gmagick
```

Alternatively, you can create a file called `phpinfo.php` under website root directory (ex: `/var/www/html/`).

```bash
nano /var/www/html/phpinfo.php
```

Add the following code.

```php
<?php

     phpinfo ();
?>
```

Open your favorite web browser and type `http://localhost/phpinfo.php` or
`http://ip-addresss/phpinfo.php` and verify the extension.

![phpinfo.png](phpinfo.png)

![phpinfo2.png](phpinfo2.png)



















