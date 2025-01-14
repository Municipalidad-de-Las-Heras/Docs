# Create a VM with Cloud-Init setting in Proxmox

Let’s see how to create a VM, starting from a template, and using Cloud-Init to configure basic settings at the first run.

## What’s Cloud-Init?

Cloud-Init is a powerful tool for automating the initial configuration of virtual machines (VMs) in cloud environments. It is designed to customize a VM when it first boots by executing scripts and applying configurations based on user-supplied metadata. This enables dynamic provisioning of instances without manual intervention.

When a VM starts, Cloud-Init reads predefined configuration data, usually provided via a metadata service or directly through cloud images. With Cloud-Init, you can set up user accounts, install packages, configure networking, and more, making it an essential element for deploying infrastructure as code.

Create VM from template using the web interface
In Proxmox, creating a VM starting from a template is quite easy.
On the left bar, where all the VMs and containers are listed, locate the template you want to use as a base for your new VM, right-click on it and then click on Clone.
You then need to input a few settings:

1. Target Proxmox node. If you have a single node setup, there is not much choice :)
2. VM ID. If you have a pattern/convention on assigning ID to your computing resources, this where you do it.
3. Name of the VM you want to create
4. Resource Pool. This is optional, depends on whether you are using or not resource pool
Cloning Mode. If you followed the previous post on how to create template, here you can choose only Full Mode, so that the template disk will be fully copied in a new one.Template Cloning Settings

![image_30.png](image_30.png)

The VM is now a thing, but it’s not yet ready to be started. There are still some things that needs to be adjusted.
First you want to resize the disk.

1. On the left panel, click on the VM
2. On the VM setting list click on `Hardware`, then click on the disk you want to resize and then on top click on Disk `Action → Resize`

![image_31.png](image_31.png)

Now it’s time to create the Cloud-Init drive. Proxmox uses an ISO, mounted to a virtual drive to pass Cloud-Init define settings.

1. On the left panel, click on the VM
2. On the VM setting list, click on `Hardware`, then click on the disk you want to resize and then on top click on `Add → Cloud Init Drive`

![image_32.png](image_32.png)

3. Choose the drive where you want to store the Cloud-Init iso that will be used by this VM.
Cloud-Init drive is now ready, and you can set configuration parameters such use user, password, ssh keys and network config.

On the left panel click on the VM
On the VM setting list click on Hardware, then click on the disk you want to resize and then on top click on Cloud-Init
On the new screen you can set:
Username, related password and ssh authorized keys
DNS search domain and DNS servers
Wether to upgrade installed packages or not
The network configuration (Static IP/DHCP) for each network interface