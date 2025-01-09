# How to unlock a Proxmox VM

From time to time, you'll come across the need to kill a lock on your Proxmox server. Fear not, in today's guide we'll discuss the various lock errors you may face and how to unlock a Proxmox VM.

## Proxmox Locked VM Errors

The "VM is locked" error is the most common circumstance in which you may want to kill a VM lock. This error has a lot of variants including:

+ Error: VM is locked (backup)
+ Error: VM is locked (snapshot)
+ Error: VM is locked (clone)

![image_33.png](image_33.png)

As you can see, they all share the same "Error: VM is locked" root, but with a suffix that indicates the task that initiated the lock, whether that task be a backup, a snapshot, or clone. This can be useful for determining IF you should clear the lock. (i.e. if the backup job is still running, you probably shouldn't clear the lock and just let the backup process complete).

`can’t lock file '/var/lock/qemu-server/lock-<VMID>.conf' – got timeout`

This is another common error, often seen when you're trying to shutdown/stop a virtual machine or when qm unlock fails (see below).

## Proxmox Unlock VM Methods
There are two main ways of clearing a lock on a Proxmox VM:

1. Using the qm unlock command and
2. Manually deleting the lock.

### 1. Using qm unlock command

The qm unlock command should be your first choice for unlocking a Proxmox VM.

First, find your VM ID (it's the number next to your VM in the Proxmox GUI). If you're not using the WebGUI, you can get a list of your VM IDs with:

```bash
cat /etc/pve/.vmlist
```

Unlock the VM/kill the lock with:

```bash
qm unlock <VMID>
```

Now, this may not always work, and the command may fail with:

```bash
trying to acquire lock...
cant lock file '/var/lock/qemu-server/lock-<VMID>.conf' - got timeout
```

In that case, let's move on to plan B: manually deleting the lock.

### 2. Manually deleting the lock

If you received the error message, `can't lock file '/var/lock/qemu-server/lock-<VMID>.conf' - got timeout`, you can fix this by manually deleting the lock at that location. Simply put, you can just run the following command:

```bash
rm /var/lock/qemu-server/lock-<VMID>.conf
```

This should be a last resort. It's generally not a great practice to go around killing locks, but sometimes you have no choice.