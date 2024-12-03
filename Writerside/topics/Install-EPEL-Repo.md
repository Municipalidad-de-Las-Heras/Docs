# Install EPEL Repo

Note: EPEL 9 has two different release packages. If you are using RHEL 9, only install the epel-release package. If you are using CentOS Stream 9, install both the epel-release and epel-next-release packages.

**RHEL 9**
```bash
subscription-manager repos --enable codeready-builder-for-rhel-9-$(arch)-rpms
dnf install https://dl.fedoraproject.org/pub/epel/epel-release-latest-9.noarch.rpm
```
**RHEL 8**

```bash
subscription-manager repos --enable codeready-builder-for-rhel-8-$(arch)-rpms
dnf install https://dl.fedoraproject.org/pub/epel/epel-release-latest-8.noarch.rpm
```
**Bibliography:** [EPEL Documentation](https://docs.fedoraproject.org/en-US/epel/getting-started/)