# How to Install Chrony NTP to Synchronize Time in RHEL9/8

Network Time Protocol (NTP) is a networking protocol that synchronizes time and date settings across computer systems in a network. It is responsible for maintaining accurate time and date settings in computer systems in order for them to run critical tasks such as cron jobs, shell scripts, and real-time applications.

NTP has since been replaced by chronyd, a networking daemon that is an implementation of the Network Time Protocol. The Chronyd service synchronizes the system clock with online NTP servers or an on-premise NTP server.

Chronyd is tailored to function in unfavorable networking environments such as in heavily congested networks and intermittent network connections. It records impressive time accuracy within a few milliseconds for systems synchronized over the internet and tens of microseconds for computers on a LAN.

## Step 1: Configuring Timezone in RHEL

To start off with the installation of Chrony NTP, head over to your RHEL server and update the package lists.

```bash
sudo dnf update
```

Since our mission is to configure our server as the NTP server from which client systems will derive their time and date settings, the next step is to ensure that the timezone settings are accurate

To display the system’s timezone, including current time and date settings, run the command:

```bash
timedatectl
```

The command displays time and timezone information. In the output below, you can see that I am in the Africa/Nairobi timezone.

![image_18.png](image_18.png)

In Linux, timezones are stored in the /usr/share/zoneinfo directory. There are a total of 423 available time zones as of the time of writing this guide.

To list all the available timezones, use the timedatectl command.

```bash
timedatectl list-timezones
```

![image_19.png](image_19.png)

To view the current timezone setting for your RHEL server, run the following command:

```bash
ls -l /etc/localtime
```
![image_20.png](image_20.png)

To set or change the timezone for your server, use the following syntax

```bash
sudo timedatectl set-timezone <timezone>
```

For example, if your current region is London, set the time zone as shown.

```bash
sudo timedatectl set-timezone Europe/London
```

## Step 2: Install Chrony NTP Server on RHEL
   With the correct timezone configured, install Chrony as shown.

```bash
sudo dnf install chrony -y
```

When the installation is complete, start and enable the Chronyd service to start on system startup.

```bash
sudo systemctl start chronyd
sudo systemctl enable chronyd
```

Also, be sure to confirm that the Chrony daemon is running as follows.

```bash
sudo systemctl status chronyd
```

![image_21.png](image_21.png)

## Step 3: Configure Chrony NTP Server on RHEL
Once the installation of Chrony is complete, you need to make a few tweaks to the default configuration /etc/chrony.conf file that contains the NTP settings.

In this file, I’m going to specify the NTP pools in my region (Kenya) using NTP Server Pool.

The NTP homepage provides a comprehensive list of global NTP pools which are used by hundreds of millions of systems around the globe. You can select your preferred NTP pools from the regions provided.

Therefore, access the file as shown.

```bash
sudo vim /etc/chrony.conf
```

Comment on the first NTP pool and specify your preferred list of NTP pools as shown.

```systemd
server 3.ke.pool.ntp.org
server 0.africa.pool.ntp.org
server 1.africa.pool.ntp.org
```

![image_22.png](image_22.png)

Save the changes and exit the configuration file. Next, enable NTP synchronization

```bash
sudo timedatectl set-ntp true
```

To apply the changes made, restart the chronyd service.

```bash
sudo systemctl restart chronyd
```

To track how the chronyd daemon is tracking, execute the following command.

```bash
chronyc tracking
```

In your terminal, you will see statistics about system time and time offset as shown.

![image_23.png](image_23.png)

## Step 4: Configure Client to Synchronize Time with Chrony NTP

Next, you need to allow clients on your network access to the server to synchronize time and date settings. Once again, head back to the configuration file.

```bash
sudo vim /etc/chrony.conf
```

Append this line to specify the network subnet of which the NTP server is a part. Here, my network subnet is 192.168.2.0/24.

```bash
allow 192.168.2.0/24
```

![image_24.png](image_24.png)

Save the changes made and exit the configuration file.

Next, verify that your NTP server is using online NTP servers for time synchronization.

```bash
chronyc sources
```

![image_25.png](image_25.png)

For detailed output, use the -v flag as shown.

```bash
chronyc sources -v
```

![image_26.png](image_26.png)

If you have Firewalld running, consider allowing NTP traffic by running the following commands.

```bash
sudo firewall-cmd  --add-service=ntp --permanent
sudo firewall-cmd --reload
```

## Step 5: Configure Chrony NTP Client to Sync Time with NTP Server
   The next thing to do is to configure a client system to receive time and date settings from the NTP server. Ensure that the client is in the same timezone as the NTP server. In our case, this will be Africa/Nairobi.

You can easily configure the timezone on the command line as follows.

```bash
sudo timedatectl set-timezone Africa/Nairobi
```

Of course, be sure to replace Africa/Nairobi with your time zone.

On the client, ensure that Chrony is installed as shown.

```bash
sudo dnf install chrony
```

Once installed, start and enable the Chrony daemon.

```bash
sudo systemctl start chronyd
sudo systemctl enable chronyd
```

Next, we need to configure the client to sync date and time settings from the NTP server. As such, modify the configuration file:

```bash
sudo nano /etc/chrony.conf
```

Comment out the pool address and add the following line where the IP address corresponds to that of the NTP server.

```bash
server 192.168.2.106
```

![image_27.png](image_27.png)

Save the changes made and exit the configuration file.

Then start the NTP synchronization.

```bash
sudo timedatectl set-ntp true
```

To apply the changes made, restart the Chronyd service.

```bash
sudo systemctl restart chronyd
```

Then verify the time synchronization as follows.

```bash
sudo chronyc sources
```

From the output below, It’s apparent that the client is obtaining time and date settings from the NTP server.

![image_28.png](image_28.png)

Back at the server, you can verify the NTP clients. You should see the IP address of the NTP client listed as shown below.

```bash
sudo chronyc clients
```

![image_29.png](image_29.png)