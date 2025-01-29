# Setup QDevice

In this tutorial, we’re going to look at how to create a 2-node cluster in Proxmox. There is so much functionality that you use when you configure a cluster in Proxmox, but in order for them to work properly, there really need to be three full nodes.

However, many users will be able to run all of their services on two devices, and adding a third device can increase the overall costs for the device and the electricity costs running it 24/7. In situations like this, creating a 2-node cluster makes sense, but you really need a third vote in order to ensure that everything works properly.

The third vote is extremely important in this, as it ultimately will determine exactly which node is down or disconnected (as two nodes will basically point fingers at each other).

## What Type of Device Do You Need for Quorum?

Quorum is used to ensure that the majority of the nodes in a cluster are online, with the overall goal being having three total votes. The QDevice we’ll be setting up today will act as the third vote without actually running the Proxmox OS.

This ensures that if one of the two devices goes down, this device will stay up to vote and maintain Quorum. For three nodes, two total votes are required to maintain functionality, which is why this is so important.

Before you look at the steps below, you need to actually configure the Proxmox Cluster. This is the most important step, but rather than using three nodes, you’ll stop at two. The steps below will configure the third device to vote.

## Step 1: Configuring Corosync on the QDevice

First, install the two necessary components by running the command below:

```bash
sudo apt install corosync-qnetd -y && sudo apt install corosync-qdevice -y
```

![image_104.png](image_104.png)

## Step 2: Configuring Both Nodes in Proxmox
Now that we’ve configured the device you’re using, we need to add this device to both Nodes. Before you can do that, you have to install this package on each node by accessing the shell and running the command below. After that’s done, you can add the device below.

```bash
apt install corosync-qdevice
```

Open Proxmox, select the first Node, and then open the Shell for that node. Run the command below substituting the IP address of the device we configured above.

```bash
pvecm qdevice setup [IP ADDRESS]
```

After the device connects, you’ll have to sign in to it using the root password your QDevice. As soon as you do, it’ll be configured on that device.

After you do, you can run the command below to list out the nodes and you should see one listed as Qdevice. This will be the device that will act as the third vote. You do not need to run this on both nodes, but I’d suggest checking both to ensure they display the QDevice.

```bash
pvecm nodes
```

![image_105.png](image_105.png)

At this point, the nodes are configured and the QDevice will act as the final vote, so if one of the Proxmox nodes goes down, everything will continue to function as expected.

**Bibliography:** [https://www.wundertech.net/how-to-create-a-2-node-cluster-in-proxmox/](https://www.wundertech.net/how-to-create-a-2-node-cluster-in-proxmox/)