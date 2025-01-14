# Change the IP Address of a Proxmox Clustered Node

First stop the cluster services

```bash
systemctl stop pve-cluster
systemctl stop corosync
```

Mount the filesystem locally

```bash
pmxcfs -l
```

Edit the network interfaces file to have the new IP information

Be sure to replace both the address and gateway

```bash
nano /etc/network/interfaces
```

Replace any host entries with the new IP addresses

```bash
nano /etc/hosts
```

Change the DNS server as necessary

```bash
nano /etc/resolv.conf
```

Edit the corosync file and replace the old IPs with the new IPs for all hosts

```bash
:%s/192\.168\.1\./192.168.2./g   <- vi command to replace all instances
```

BE SURE TO INCREMENT THE config_version: x LINE BY ONE TO ENSURE THE CONFIG IS NOT OVERWRITTEN

```bash
nano /etc/pve/corosync.conf
```

Edit the known hosts file to have the correct IPs

```bash
:%s/192\.168\.1\./192.168.2./g   <- vi command to replace all instances
```

```bash
nano/etc/pve/priv/known_hosts
```

If using ceph, edit the ceph configuration file to reflect the new network

```bash
:%s/192\.168\.1\./192.168.2./g   <- vi command to replace all instances
```

```bash
nano /etc/ceph/ceph.conf
```

If you want to be granular... fix the IP in /etc/issue

```bash
nano /etc/issue
```

Verify there aren't any stragglers with the old IP hanging around

```bash
cd /etc
grep -R '192\.168\.1\.' *
cd /var
grep -R '192\.168\.1\.' *
```

Reboot the system to cleanly restart all the networking and services

```bash
reboot now
```

## Bibliography

https://forum.proxmox.com/threads/change-cluster-nodes-ip-addresses.33406/
https://pve.proxmox.com/wiki/Cluster_Manager#_remove_a_cluster_node