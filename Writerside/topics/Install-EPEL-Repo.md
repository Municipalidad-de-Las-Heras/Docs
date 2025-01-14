# Install EPEL Repo

To enable the EPEL repository on your system, install the appropriate release package as described below. These packages contain the repository configuration and public package signing key.

## EL10
### CentOS Stream 10

```bash
dnf config-manager --set-enabled crb
dnf install https://dl.fedoraproject.org/pub/epel/epel-release-latest-10.noarch.rpm
```

## EL9

EPEL 9 has two different release packages. If you are using RHEL 9, only install the epel-release package. If you are using CentOS Stream 9, install both the epel-release and epel-next-release packages.

### CentOS Stream 9

```bash
dnf config-manager --set-enabled crb
dnf install https://dl.fedoraproject.org/pub/epel/epel{,-next}-release-latest-9.noarch.rpm
```

### RHEL 9

```bash
subscription-manager repos --enable codeready-builder-for-rhel-9-$(arch)-rpms
dnf install https://dl.fedoraproject.org/pub/epel/epel-release-latest-9.noarch.rpm
```

### Other RHEL 9 compatible distributions

EPEL 9 officially targets CentOS Stream 9 and RHEL 9. EPEL 9 packages will also likely work with other distributions that target RHEL 9 compatibility. We cannot list specific instructions for all such distributions, but in general, the steps needed should look similar to the steps for RHEL 9. First enable the distribution’s equivalent of the CRB repository, then install the epel-release package.

```bash
dnf config-manager --set-enabled crb
dnf install https://dl.fedoraproject.org/pub/epel/epel-release-latest-9.noarch.rpm
```

## EL8

### RHEL 8

```bash
subscription-manager repos --enable codeready-builder-for-rhel-8-$(arch)-rpms
dnf install https://dl.fedoraproject.org/pub/epel/epel-release-latest-8.noarch.rpm
```

### Other RHEL 8 compatible distributions


EPEL 8 officially focuses on RHEL 8. EPEL 8 packages will also likely work with other distributions that target RHEL 8 compatibility. We cannot list specific instructions for all such distributions, but in general the steps needed should look similar to the steps for RHEL 8. First enable the distribution’s equivalent of the PowerTools/CRB repository, then install the epel-release package.

```bash
dnf config-manager --set-enabled powertools
dnf install https://dl.fedoraproject.org/pub/epel/epel-release-latest-8.noarch.rpm
```

**Bibliography:** [EPEL Documentation](https://docs.fedoraproject.org/en-US/epel/getting-started/)