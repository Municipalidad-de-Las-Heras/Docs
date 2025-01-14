# Backup and Restore VMs in Proxmox

In this tutorial, we will explore how to backup and restore VMs in Promox. As a prerequisite, ensure that you already have a virtual machine running on your Proxmox instance.

## Step 1: Poweroff the VM in Proxmox

First off, you need to shut down your virtual machine before taking a backup of your VM. On the Proxmox dashboard, right-click on your virtual machine and select the `Shutdown` option.

![image_70.png](image_70.png)

Next, click `Yes` to poweroff the VM.

![image_71.png](image_71.png)

Your virtual machine will power off shortly.

## Step 2: Create Proxmox VM Backup

The next step is to prepare the virtual machine backup. So click `Backup`. Then head over and click the `Backup now` tab.

![image_72.png](image_72.png)

On the pop-up that appears, select your backup storage. In our case, we have a secondary hard drive labeled `backup` with a capacity of 52.52GB.

![image_73.png](image_73.png)

While you can still create the backup on the proxmox server itself, it’s recommended to save it in a different location so that in case something goes wrong with the server, the backup copy will still be recoverable.

Be sure to go with ZSTD compression, which is preselected by default owing to its high performance and click `Backup`.

![image_74.png](image_74.png)

The backup process of the virtual machine will start, and this will be displayed on the task viewer pop-up as shown. Once complete, you will see the `TASK OK` notification as indicated.

![image_75.png](image_75.png)

The backup will be displayed as a compressed file as shown. The backup file is saved in the destination you provided as the backup location.

![image_76.png](image_76.png)

At this point, you can safely remove the VM by clicking on its icon, then `More` and finally `Remove`.

![image_77.png](image_77.png)

Provide the VM ID – In this case, 100 – and click `Remove`.

![image_78.png](image_78.png)

## Step 3: Restore Proxmox Virtual Machine

With the backup file in place, you can restore the virtual machine to its original state using the following simple steps. Head over to the backup drive and click on the backup file, then click the `Restore` button as shown.

![image_79.png](image_79.png)

On the pop-up dialogue box, click `Restore` to start the VM restoration process.

![image_80.png](image_80.png)

A task viewer will open detailing the restoration process as shown. This takes a few seconds to a minute, so be patient.

![image_81.png](image_81.png)

Once completed, you will see the `TASK OK` notification. Close the pop-up to exit.

![image_82.png](image_82.png)

On the Proxmox dashboard, you should now see your restored virtual machine. To start using your VM, right-click and select `Start`.

![image_83.png](image_83.png)

**Bibliography:** [https://www.tecmint.com/proxmox-backup-restore-vm/](https://www.tecmint.com/proxmox-backup-restore-vm/)