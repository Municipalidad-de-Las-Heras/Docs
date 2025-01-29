# Add user to Sudoers group

First, add the user to the group of sudo by replacing the “username” with the actual username in the below command:

```bash
sudo usermod -aG sudo [username]
```

To verify the successful execution of the above command, list the groups in which “vboxuser” is a part:

```bash
groups vboxuser
```

**Bibliography:** [https://itslinuxfoss.com/add-user-sudoers-debian-12/](https://itslinuxfoss.com/add-user-sudoers-debian-12/)