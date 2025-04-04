# How to Fix A Proxmox VM In An Error State

## 1. Disable the VM in HA Manager:

```bash
   ha-manager set vm:104 --state disabled
```

## 2. Confirm the VM is not running:

```bash   
ps -eaf | grep [q]emu-kvm | grep 104
```

You've already done this, and it seems like the VM isn’t running.


## 3. Check and Fix the Error:

Analyze the VM's log for any issues or errors that might have occurred:

```bash
cat /var/log/pve/qemu-server/104.log
```

Ensure the underlying issue is fixed before trying to start the VM again to prevent it from entering an error state once more.


## 4. Enable and Start the VM:

```bash   
ha-manager set vm:104 --state enabled
qm start 104
```

Now, try to start the VM and verify its state by using:

```bash
qm list | grep 104
```

## Additional Notes:
+ Ensure that there are enough resources (CPU, RAM, Disk space) available on the node to start the VM.
+ Make sure the storage where the VM's disk is located is accessible and has enough space.
+ Check the Proxmox cluster's state and confirm that the nodes see each other correctly and that the quorum is not lost.
+ If the issue persists, it might be worth examining the VM's configuration file for any inconsistencies or issues.
+ Ensure your Proxmox and its packages are updated to avoid running into known issues that might have been fixed in newer versions.
+ Remember to always have a good backup strategy in place to safeguard against data loss, especially when dealing with virtual machine management and troubleshooting.