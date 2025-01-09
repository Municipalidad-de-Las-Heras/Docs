# 2.1 Install PHP Recommended Extensions

## Install REMI Repository

Command to enabled the CRB repository:
```bash
subscription-manager repos --enable codeready-builder-for-rhel-9-x86_64-rpms
```

Command to install the EPEL repository configuration package:

```bash
dnf install https://dl.fedoraproject.org/pub/epel/epel-release-latest-9.noarch.rpm
```

Command to install the Remi repository configuration package:

```bash
dnf install https://rpms.remirepo.net/enterprise/remi-release-9.rpm
```

## Install ImageMagick (Image Manipulation) Tool

To use the ImageMagick tool with PHP or Perl programming language, you will need to install ImageMagick with the Imagick PHP extension for PHP and ImageMagick-Perl extension for Perl.

Imagick is a simple PHP extension for creating and modifying images using the ImageMagick API program. There is a confusion in name, as people think that ImageMagick and Imagick both are the same, but you can use ImageMagick without Imagick extension but you need both installed on your machine to use and run it.

### Installing ImageMagick from Repository
First, install following prerequisite php-pear, php-devel and gcc packages to compile the Imagick PHP extension.

```bash
sudo dnf install php-pear php-devel gcc 
```

Once you’ve installed php-pear, php-devel, and gcc packages, you may now install ImageMagick software for PHP and Perl support using dnf command.

```bash
sudo dnf install ImageMagick ImageMagick-devel ImageMagick-perl
```

Next, verify that ImageMagick has been installed on your system by checking its version.

```bash
convert --version
```

### Installing ImageMagick 7 from Source Code
To install ImageMagick from source, you need a proper development environment with a compiler and related development tools. If you don’t have the required packages on your system, install development tools as shown:

```bash
sudo dnf groupinstall 'Development Tools'
sudo dnf -y install bzip2-devel freetype-devel libjpeg-devel libpng-devel libtiff-devel giflib-devel zlib-devel ghostscript-devel djvulibre-devel libwmf-devel jasper-devel libtool-ltdl-devel libX11-devel libXext-devel libXt-devel lcms-devel libxml2-devel librsvg2-devel OpenEXR-devel php-devel
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

#### Install from PECL
Next, compile the Imagick for PHP extension. To do, simply run the following ‘pecl‘ command. It will install ImageMagick and imagick PHP extension module `imagick.so` under `/usr/lib/php/modules` directory. If you are using a 64-bit system, the module directory path would be `/usr/lib64/php/modules`.

Note: It will ask you to provide Imagemagick installation prefix, simply hit enter to auto-detect.

```bash
sudo pecl install imagick 
```

#### Install from REMI Repository
If for some reason the installation from pecl fails, we can install the extension from the REMI repository:

```bash
sudo dnf install php84-php-pecl-imagick-im7 
```

Now, add the `imagick.so` extension to `/etc/php.ini` file.

```bash
echo extension=imagick.so >> /etc/php.ini
```

Next, restart Apache webserver.

```bash
sudo systemctl restart httpd
```

Verify the Imagick PHP extension by running the following command. You will see the Imagick extension similar to below.

```bash
php -m | grep imagick
```

![phpinfo.png](phpinfo.png)

## Install PHP Zip Extension

### Install from package manager

Use DNF to install the PHP Zip extension:

```bash
sudo dnf install php84-php-pecl-zip libzip-devel
```

#### Add the extension to your PHP configuration file

```bash
echo "extension=zip.so" | sudo tee -a /etc/php.ini
```

#### Check the Installation

After installation, check if the Zip extension is active:

Run in the terminal:

```bash
php -m | grep zip
```

If you see `zip` in the output, the extension is active.
You can also create a PHP file with this content:

```php
<?php
if (extension_loaded('zip')) {
    echo "ZIP extension is installed.";
} else {
    echo "ZIP extension is not installed.";
}
?>
```

Save this as `check_zip.php` and run it in your browser or command line to confirm the installation.

### Install using PECL (PHP Extension Community Library)

PECL offers another way to install PHP extensions. To use PECL for installing the ZIP extension:

#### Install PECL

```bash
sudo dnf install php-pear libzip-devel
```

#### Install the ZIP extension through PECL

```bash
sudo pecl install zip
```

#### Add the extension to your PHP configuration

```bash
echo "extension=zip.so" | sudo tee -a /etc/php.ini
```

Restart your web server to apply the changes.

## Install Intl PHP Extension

As you have php-commom from remi repositories, you need to get php-intl from remi also.

```bash
sudo dnf install php84-php-intl
```

#### Add the extension to PHP configuration file

```bash
echo "extension=intl.so" | sudo tee -a /etc/php.ini
```

## Add PHP extensions to path

If you installed the extensions from the REMI repository and the system still does not recognize them, you must create a symbolic link to the php path. This happens because the REMI packages are installed in a different location by default than the one where php is installed. You must do this with all the extensions, and it is done in the following way:

```bash
sudo ln -s /opt/remi/php84/root/usr/lib64/php/modules/xxx.so /usr/lib64/php/modules/xxx.so
```

For example:

```bash
sudo ln -s /opt/remi/php84/root/usr/lib64/php/modules/intl.so /usr/lib64/php/modules/intl.so
```

This will create a symbolic link, so every time the package is updated, php will recognize the updated library without needing to do anything











