# Add user to Sudoers

Answer updated to account for VirtualBox 7 and its unattended installations.

Defaults for Ubuntu are:

+ **Login to root account is disabled** 
+ **Members of group sudo are granted full sudo privileges** 
+ **A user account is created during installation and added to the sudo group** 
+ **When you're logged in into that account, you should be able to use the sudo command**

However, VirtualBox 7 will by default have "unattended installation" enabled, which sets up Ubuntu differently:

+ **Login to root account is enabled - default password is changeme**
+ **User account is created - default user/password are vboxuser/changeme**
+ **The user account is not a sudoer**


The slightly more complicated, but also more universal method is described below. It lets you make any user a sudoer by accessing the root account even if it's disabled. It works both for VMs and physical computers.

When the machine is booting, immediately start pressing Esc repeatedly. You should see the GRUB screen:

![image1.png](image1.png)

Using arrows and Enter select Advanced options for Ubuntu, then Ubuntu, with Linux [...] (recovery mode) (2nd option from the top). You should see something like this: (although older Ubuntu versions won't be so colorful)

![image2.png](image2.png)

Select root - Drop to root shell prompt. Execute this command to add yourUsername to the sudo group:

```bash
adduser yourUsername sudo
```