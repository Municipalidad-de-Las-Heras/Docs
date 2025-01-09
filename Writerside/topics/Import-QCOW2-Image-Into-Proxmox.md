# Import QCOW2 Image Into Proxmox

In this guide, we will see how to import QCOW2 into Proxmox VE hypervisor and how to create a virtual machine using the QCOW2 image in Proxmox.

## Introduction
Some OSes, and firewalls or network appliances are shipped only in QCOW2 format.

For those wondering, QCOW, stands for QEMU copy-on-write, is the default storage format for virtual disks of QEMU/KVM instances.

Using the QCOW2 images, we can instantly create and run new virtual machines with hypervisor.

## Step 1: Create a Directory to Store QCOW2 Images
First, we need to create a directory to store the QCOW2 images. I am going to create a directory called "qcow" under the Proxmox default storage directory.

```bash
sudo mkdir /var/lib/vz/template/qcow
```

Please note that you can save the images on any location of your choice.

## Step 2: Copy the QCOW2 Images to Proxmox Storage Directory
Download and copy the QCOW2 image to the directory that you created earlier. For the purpose of this guide, I will be using FreeBSD 12.3 QCOW2 image file.

```bash
sudo cp Software/FreeBSD\ 12\ Qcow2/FreeBSD-12.3-RELEASE-amd64.qcow2 /var/lib/vz/template/qcow/
```

You can verify if the image is really copied or not.

```bash
ls -l -h /var/lib/vz/template/qcow/
```

Sample Output:

```bash
total 3.2G
-rw-r--r-- 1 root root 3.2G Jun 13 16:17 FreeBSD-12.3-RELEASE-amd64.qcow2
```

![imageoutput.png](imageoutput.png "Copy QCOW2 Image To Proxmox Storage")

## Step 3: Create a VM Without OS
Log in to the Proxmox Web UI dashboard by navigating to `https://ip-address:8006` URL.

Right click on your Proxmox node and click `Create VM` option from the context menu.

![image.png](image.png)

Enter the name of the VM. Also make a note of the VM ID (i.e. 107 in my case). The ID will be auto-created based on the existing number of available VMs. We are going to need the VM ID when we attach the QCOW2 image to the VM. Click OK to continue.

![image_2.png](image_2.png)

Next choose "Do not use any media" option. Because we already have a pre-installed OS in the QCOW2 image, right? Yes! Also choose the guest type and version. There is no entry for Unix guest OS in Proxmox, so I simply selected "Other". If you use import a Linux Qcow2 image, choose guest type as "Linux" and Kernel as "6.x-2.6 Kernel".

![image_3.png](image_3.png)

Choose the graphics card, firmware and SCSI controller settings for your VM. usually, the default values are sufficient. I will go with default values.

![image_4.png](image_4.png)

Enter the size for the virtual machine's disk. Here, I will keep the default size i.e. 32 GB. Also make sure you've chosen the disk format as "QEMU image format" as shown in the following screenshot.

![image_5.png](image_5.png)

Enter the CPU details such as the number of sockets and cores.

![image_6.png](image_6.png)

Enter the RAM size for your VM. here, I have given 2 GB.

![image_7.png](image_7.png)

Enter network details. Mostly the default settings will work just fine. If you wish to change the network settings (E.g. enable or disable firewall), do it as you wish.

![image_8.png](image_8.png)

You will see the summary of the VM's settings. Review them and if you're OK with it, click Finish to create the VM. Or click "Back" button and change the settings as you wish.

![image_9.png](image_9.png)

We just created a VM without OS. It is time to attach the QCOW2 image to the VM.

## Step 4: Import QCOW2 Image into Proxmox Server
Before importing the QCOW2 into your Proxmox server, make sure you've the following details in hand.

1. Virtual machine's ID,
2. Proxmox storage name,
3. Location of the Proxmox QCOW2 image file.
4. If you don't have them or don't know where to find them, just open your Proxmox web UI dashboard. On the left pane, you will see the virtual machine's IDs and the storage name.

![image_10.png](image_10.png)

Here, my FreeBSD 12 VM id is "107" and Proxmox storage name is "local". And the directory path where I saved the QCOW2 image is /var/lib/vz/template/qcow/ (Please refer Step 2.).

Change into the `/var/lib/vz/template/qcow/` directory:

```bash
cd /var/lib/vz/template/qcow/
```

Now, import the QCOW2 image into the Proxmox server using command:


```bash
sudo qm importdisk 107 FreeBSD-12.3-RELEASE-amd64.qcow2 local
```

Replace the VM id (107) and storage name (local) with your own.

Sample Output:

```bash
importing disk 'FreeBSD-12.3-RELEASE-amd64.qcow2' to VM 107 ...
Formatting '/var/lib/vz/images/107/vm-107-disk-1.raw', fmt=raw size=5369626624 preallocation=off
transferred 0.0 B of 5.0 GiB (0.00%)
transferred 52.7 MiB of 5.0 GiB (1.03%)
[...]
transferred 5.0 GiB of 5.0 GiB (100.00%)
transferred 5.0 GiB of 5.0 GiB (100.00%)
Successfully imported disk as 'unused0:local:107/vm-107-disk-1.raw'
```

![image_11.png](image_11.png)

We imported the virtual disk to Proxmox. Now go back to the Proxmox web UI dashboard and attach the virtual disk to the VM.

## Step 5: Attach QCOW2 Virtual Disk to VM
Click on the Virtual machine that you created in step 3. In my case, it is FreeBSD 12 VM. Select "Hardware" tab. On the right hand side, you will the newly imported QCOW2 disk as unused disk. Select the unused disk and then click "Edit" button.

![image_12.png](image_12.png)

Choose the bus type as "VirtIO Block" to get best disk I/O performance and hit "Add" button.

![image_13.png](image_13.png)

You will now see a newly disk with VirtIO as bus type has been attached to the VM.

![image_14.png](image_14.png)

Great! We successfully attached a new disk to the Proxmox VM.

## Step 6: Change the Boot Order
To make the VM to boot from the newly added disk, we must change the boot order.

Select Virtual machine -> Options -> Boot Order.

![image_15.png](image_15.png)

To boot from the new disk, it must be on top in the boot order window. Select the newly added VirtIO disk and drag it to the top. Make sure you checked the tick box to enable the disk. Click "OK" to save.

![image_16.png](image_16.png)

Now start the virtual machine. It should boot from the new disk.

![image_17.png](image_17.png)




