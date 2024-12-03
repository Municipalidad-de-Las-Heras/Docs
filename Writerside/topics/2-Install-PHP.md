# 2. Install PHP Programing Language

## Step 1: Install/Enable REMI Repository

Command to enabled the CRB repository:
```bash
subscription-manager repos --enable codeready-builder-for-rhel-9-x86_64-rpms
```
## Step 2: Install/Enable EPEL 9 Repository

Command to install the EPEL repository configuration package:

```bash
dnf install https://dl.fedoraproject.org/pub/epel/epel-release-latest-9.noarch.rpm
```

## Step 3: Install REMI configuration package

Command to install the Remi repository configuration package:

```bash
dnf install https://rpms.remirepo.net/enterprise/remi-release-9.rpm
```

## Step 4: Install PHP

You want a single version which means replacing base packages from the distribution

Packages have the same name than the base repository, ie php-*

Some common dependencies are available in remi-safe repository, which is enabled by default

PHP version 8.4.0 packages are available for RHEL 9 in remi-modular repository

Command to enable the module stream for 8.4, if a version is already installed this will also upgrade/downgrade it:

```bash
dnf module switch-to php:remi-8.4
```

If no version is installed, command to install the php stream default profile:

```bash
dnf module install php:remi-8.4
```

Command to install additional packages (xxx for SAPI or extension name):

```bash
dnf install php-xxx
```

Command to install testing packages:

```bash
dnf --enablerepo=remi-modular-test install php-xxx
```

Command to check the installed version and available extensions:

```bash
php --version
php --modules
```