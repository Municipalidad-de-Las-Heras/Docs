# Setup Proxmox Repositories

Proxmox VE uses APT as its package management tool like any other Debian-based system.

Proxmox VE automatically checks for package updates on a daily basis. The root@pam user is notified via email about available updates. From the GUI, the Changelog button can be used to see more details about an selected update.

## Repositories in Proxmox VE

Repositories are a collection of software packages, they can be used to install new software, but are also important to get new updates.

**Note:** You need valid Debian and Proxmox repositories to get the latest security updates, bug fixes and new features.

APT Repositories are defined in the file `/etc/apt/sources.list` and in `.list` files placed in `/etc/apt/sources.list.d/`.

## Repository Management

Since Proxmox VE 7, you can check the repository state in the web interface. The node summary panel shows a high level status overview, while the separate Repository panel shows in-depth status and list of all configured repositories.

Basic repository management, for example, activating or deactivating a repository, is also supported.

![image_84.png](image_84.png)

### Sources.list
In a `sources.list` file, each line defines a package repository. The preferred source must come first. Empty lines are ignored. A # character anywhere on a line marks the remainder of that line as a comment. The available packages from a repository are acquired by running apt-get update. Updates can be installed directly using `apt`, or via the GUI `Node → Updates`.

File `/etc/apt/sources.list`

```
deb http://deb.debian.org/debian bookworm main contrib
deb http://deb.debian.org/debian bookworm-updates main contrib

# security updates
deb http://security.debian.org/debian-security bookworm-security main contrib
Proxmox VE provides three different package repositories.
```

## Proxmox VE Enterprise Repository

This is the recommended repository and available for all Proxmox VE subscription users. It contains the most stable packages and is suitable for production use. The pve-enterprise repository is enabled by default:

File `/etc/apt/sources.list.d/pve-enterprise.list`

```
deb https://enterprise.proxmox.com/debian/pve bookworm pve-enterprise
```

Please note that you need a valid subscription key to access the pve-enterprise repository. We offer different support levels, which you can find further details about at [https://proxmox.com/en/proxmox-virtual-environment/pricing](https://proxmox.com/en/proxmox-virtual-environment/pricing).

**Note:** You can disable this repository by commenting out the above line using a # (at the start of the line). This prevents error messages if your host does not have a subscription key. Please configure the pve-no-subscription repository in that case.

## Proxmox VE No-Subscription Repository

As the name suggests, you do not need a subscription key to access this repository. It can be used for testing and non-production use. It’s not recommended to use this on production servers, as these packages are not always as heavily tested and validated.

We recommend configuring this repository in `/etc/apt/sources.list`.

File `/etc/apt/sources.list`

```
deb http://ftp.debian.org/debian bookworm main contrib
deb http://ftp.debian.org/debian bookworm-updates main contrib

# Proxmox VE pve-no-subscription repository provided by proxmox.com,
# NOT recommended for production use
deb http://download.proxmox.com/debian/pve bookworm pve-no-subscription

# security updates
deb http://security.debian.org/debian-security bookworm-security main contrib
```

## Proxmox VE Test Repository

This repository contains the latest packages and is primarily used by developers to test new features. To configure it, add the following line to `/etc/apt/sources.list`:

`sources.list` entry for pvetest

```
deb http://download.proxmox.com/debian/pve bookworm pvetest
```

Warning	The pvetest repository should (as the name implies) only be used for testing new features or bug fixes.

## Ceph Squid Enterprise Repository

This repository holds the enterprise Proxmox VE Ceph 19.2 Squid packages. They are suitable for production. Use this repository if you run the Ceph client or a full Ceph cluster on Proxmox VE.

File `/etc/apt/sources.list.d/ceph.list`

```
deb https://enterprise.proxmox.com/debian/ceph-squid bookworm enterprise
```

## Ceph Squid No-Subscription Repository

This Ceph repository contains the Ceph 19.2 Squid packages before they are moved to the enterprise repository and after they where on the test repository.

**Note:** It’s recommended to use the enterprise repository for production machines.

File `/etc/apt/sources.list.d/ceph.list`

```
deb http://download.proxmox.com/debian/ceph-squid bookworm no-subscription
```

## Ceph Squid Test Repository

This Ceph repository contains the Ceph 19.2 Squid packages before they are moved to the main repository. It is used to test new Ceph releases on Proxmox VE.

File `/etc/apt/sources.list.d/ceph.list`

```
deb http://download.proxmox.com/debian/ceph-squid bookworm test
```

## Ceph Reef Enterprise Repository

This repository holds the enterprise Proxmox VE Ceph 18.2 Reef packages. They are suitable for production. Use this repository if you run the Ceph client or a full Ceph cluster on Proxmox VE.

File `/etc/apt/sources.list.d/ceph.list`

```
deb https://enterprise.proxmox.com/debian/ceph-reef bookworm enterprise
```

## Ceph Reef No-Subscription Repository

This Ceph repository contains the Ceph 18.2 Reef packages before they are moved to the enterprise repository and after they where on the test repository.

**Note:** It’s recommended to use the enterprise repository for production machines.

File `/etc/apt/sources.list.d/ceph.list`

```
deb http://download.proxmox.com/debian/ceph-reef bookworm no-subscription
```

## Ceph Reef Test Repository

This Ceph repository contains the Ceph 18.2 Reef packages before they are moved to the main repository. It is used to test new Ceph releases on Proxmox VE.

File `/etc/apt/sources.list.d/ceph.list`

```
deb http://download.proxmox.com/debian/ceph-reef bookworm test
```

## Ceph Quincy Enterprise Repository

This repository holds the enterprise Proxmox VE Ceph Quincy packages. They are suitable for production. Use this repository if you run the Ceph client or a full Ceph cluster on Proxmox VE.

File `/etc/apt/sources.list.d/ceph.list`

```
deb https://enterprise.proxmox.com/debian/ceph-quincy bookworm enterprise
```

## Ceph Quincy No-Subscription Repository

This Ceph repository contains the Ceph Quincy packages before they are moved to the enterprise repository and after they where on the test repository.

**Note:** It’s recommended to use the enterprise repository for production machines.

File `/etc/apt/sources.list.d/ceph.list`

```
deb http://download.proxmox.com/debian/ceph-quincy bookworm no-subscription
```

## Ceph Quincy Test Repository

This Ceph repository contains the Ceph Quincy packages before they are moved to the main repository. It is used to test new Ceph releases on Proxmox VE.

File `/etc/apt/sources.list.d/ceph.list`

```
deb http://download.proxmox.com/debian/ceph-quincy bookworm test
```

## Older Ceph Repositories
Proxmox VE 8 doesn’t support Ceph Pacific, Ceph Octopus, or even older releases for hyper-converged setups. For those releases, you need to first upgrade Ceph to a newer release before upgrading to Proxmox VE 8.

See the respective upgrade guide for details.

## Debian Firmware Repository

Starting with Debian Bookworm (Proxmox VE 8) non-free firmware (as defined by DFSG) has been moved to the newly created Debian repository component non-free-firmware.

Enable this repository if you want to set up Early OS Microcode Updates or need additional Runtime Firmware Files not already included in the pre-installed package pve-firmware.

To be able to install packages from this component, run editor `/etc/apt/sources.list`, append `non-free-firmware` to the end of each `.debian.org` repository line and run `apt update`.

## SecureApt
The Release files in the repositories are signed with GnuPG. APT is using these signatures to verify that all packages are from a trusted source.

If you install Proxmox VE from an official ISO image, the key for verification is already installed.

If you install Proxmox VE on top of Debian, download and install the key with the following commands:

```bash
sudo wget https://enterprise.proxmox.com/debian/proxmox-release-bookworm.gpg -O /etc/apt/trusted.gpg.d/proxmox-release-bookworm.gpg
```

Verify the checksum afterwards with the sha512sum CLI tool:

```bash
sudo sha512sum /etc/apt/trusted.gpg.d/proxmox-release-bookworm.gpg
7da6fe34168adc6e479327ba517796d4702fa2f8b4f0a9833f5ea6e6b48f6507a6da403a274fe201595edc86a84463d50383d07f64bdde2e3658108db7d6dc87 /etc/apt/trusted.gpg.d/proxmox-release-bookworm.gpg
```

or the md5sum CLI tool:

```bash
sudo md5sum /etc/apt/trusted.gpg.d/proxmox-release-bookworm.gpg
41558dc019ef90bd0f6067644a51cf5b /etc/apt/trusted.gpg.d/proxmox-release-bookworm.gpg
```

**Bibliography:** [https://pve.proxmox.com/wiki/Package_Repositories](https://pve.proxmox.com/wiki/Package_Repositories)