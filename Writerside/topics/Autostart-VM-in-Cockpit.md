# Autostart VM in Cockpit

## List Available Virtual Machines
To start let’s list all available Virtual machines on our host system:

```bash
sudo virsh list --all
Id    Name                           State
----------------------------------------------------
-     server.linuxconfig.org            shut off
```

To check whether given virtual machine is already configured to start after host system’s boot run:

```bash
sudo virsh dominfo server.linuxconfig.org
Id:             -
Name:           server.linuxconfig.org
UUID:           df25d714-1c73-4b4a-b566-9d0a17295702
OS Type:        hvm
State:          shut off
CPU(s):         2
Max memory:     1048576 KiB
Used memory:    1048576 KiB
Persistent:     yes
Autostart:      disable
Managed save:   no
Security model: selinux
Security DOI:   0
```

Furthermore, to list all virtual machines already configured to autostart run:

```bash
sudo ls /etc/libvirt/qemu/autostart/
```

## Enable Virtual Machine automatic start
To enable the above KVM virtual machine to start automatically run the following linux command:

```bash
sudo virsh autostart server.linuxconfig.org
Domain server.linuxconfig.org marked as autostarted
```

If `virsh` command is not available/installed, to configure austostart simply create a new symbolic link within `/etc/libvirt/qemu/autostart/` directory using `ln` command. Example:

```bash
sudo ln -s /etc/libvirt/qemu/server.linuxconfig.org.xml /etc/libvirt/qemu/autostart/server.linuxconfig.org.xml
```

and reload hyper-visor if necessary:

```bash
sudo systemctl reload libvirtd
```

Confirm whether autostart is enabled:

```bash
sudo virsh dominfo server.linuxconfig.org
Id:             -
Name:           server.linuxconfig.org
UUID:           df25d714-1c73-4b4a-b566-9d0a17295702
OS Type:        hvm
State:          shut off
CPU(s):         2
Max memory:     1048576 KiB
Used memory:    1048576 KiB
Persistent:     yes
Autostart:      enable
Managed save:   no
Security model: selinux
Security DOI:   0
```

## Disable Virtual Machine autostart

To disable virtual machine autostart execute:

```bash
sudo virsh autostart --disable server.linuxconfig.org
Domain server.linuxconfig.org unmarked as autostarted
```

or simply use unlink command to remove virtual machine’s symbolic link from /etc/libvirt/qemu/autostart/ directory:

```bash
sudo unlink /etc/libvirt/qemu/autostart/server.linuxconfig.org.xml
```

## Bibliography

[https://linuxconfig.org/configuring-virtual-machine-automatic-start-on-redhat-linux-host#:~:text=Instructions%201%20List%20Available%20Virtual%20Machines%20To%20start,autostart%20To%20disable%20virtual%20machine%20autostart%20execute%3A%20](https://linuxconfig.org/configuring-virtual-machine-automatic-start-on-redhat-linux-host#:~:text=Instructions%201%20List%20Available%20Virtual%20Machines%20To%20start,autostart%20To%20disable%20virtual%20machine%20autostart%20execute%3A%20)