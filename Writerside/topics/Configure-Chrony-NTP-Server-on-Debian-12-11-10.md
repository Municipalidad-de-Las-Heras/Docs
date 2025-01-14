# Configure Chrony NTP Server on Debian 12/11/10

NTP (Network Time Protocol) is a protocol that synchronizes clocks across a network to provide accurate time. Clocks can be found on personal computers, network routers, servers, and other devices. NTP-enabled programs can typically act as both a client and a server, as well as perform peer-to-peer communication. To disseminate time, NTP uses a hierarchical system. Servers at the top of the hierarchy are connected to reference clocks. This is essential in a communication system so that the computers may communicate seamlessly.

## Benefits of NTP Time Synchronization
Here are the advantages of NTP:

+ It synchronizes the devices’ internet connections.
+ It increases the level of security within the building.
+ It’s used in Kerberos and other authentication systems.
+ It offers network acceleration, which aids in the diagnosis of issues.
+ Used in file systems where network synchronization is challenging.

## Get to Understand Chrony
The chrony daemon (chronyd) is a lot better than ntpd. It can keep accurate time on systems with busy networks or systems that are unavailable for extended periods of time, as well as virtualized systems. Furthermore, it synchronizes the system clock faster than ntpd and may be simply setup to work as a local time-server. Most distributions recommend using the chrony service for software clock synchronization, with a few exceptions.

## Step 1: Setting Correct Timezone
On system timedatectl command can be used to get information about the system clock and its settings, as well as enable and stop time synchronization services.

It has the following syntax:

```bash
timedatectl [OPTIONS…] {COMMAND}
```

When you use the timedatectl command by default, it displays the current date and time settings in the system:

```bash
timedatectl
Local time: Wed 2023-08-30 06:28:36 EDT
Universal time: Wed 2023-08-30 10:28:36 UTC
RTC time: Wed 2023-08-30 10:28:36
Time zone: America/New_York (EDT, -0400)
System clock synchronized: yes
NTP service: active
RTC in local TZ: no
```

### Set Time and Date with timedatectl
As per the syntax of timedatectl command, you can set time and date as follows:

```bash
sudo timedatectl set-time '2023-08-30 13:55:00'
```

### List Available Timezones
timedatectl will display a list of available time zones:

```bash
timedatectl list-timezones
Africa/Abidjan
Africa/Accra
Africa/Algiers
Africa/Bissau
Africa/Cairo
Africa/Casablanca
Africa/Ceuta
Africa/El_Aaiun
Africa/Johannesburg
Africa/Juba
Africa/Khartoum
Africa/Lagos
Africa/Maputo
Africa/Monrovia
Africa/Nairobi
Africa/Ndjamena
Africa/Sao_Tome
Africa/Tripoli
Africa/Tunis
Africa/Windhoek
America/Adak
America/Anchorage
America/Araguaina
America/Argentina/Buenos_Aires
America/Argentina/Catamarca
America/Argentina/Cordoba
America/Argentina/Jujuy
America/Argentina/La_Rioja
America/Argentina/Mendoza
America/Argentina/Rio_Gallegos
America/Argentina/Salta
America/Argentina/San_Juan
America/Argentina/San_Luis
....
```

### Set the Timezone
timedatectl can be used to set the correct timezone as follows, i.e my correct timezone is Africa/Nairobi.

```bash
sudo timedatectl set-timezone Africa/Nairobi
```

We can confirm the changes as follows:

```bash
timedatectl
Local time: Wed 2023-08-30 13:29:55 EAT
Universal time: Wed 2023-08-30 10:29:55 UTC
RTC time: Wed 2023-08-30 10:29:55
Time zone: Africa/Nairobi (EAT, +0300)
System clock synchronized: yes
NTP service: active
RTC in local TZ: no
```

## Step 2: Install Chrony NTP Server on Debian

Chrony NTP server can be installed on Debian as follows:

```bash
sudo apt update && sudo apt install chrony
```

Installation output:

```bash
Reading package lists... Done
Building dependency tree... Done
Reading state information... Done
Suggested packages:
networkd-dispatcher
The following packages will be REMOVED:
systemd-timesyncd
The following NEW packages will be installed:
chrony
0 upgraded, 1 newly installed, 1 to remove and 0 not upgraded.
Need to get 286 kB of archives.
After this operation, 433 kB of additional disk space will be used.
Do you want to continue? [Y/n] y
```

After that, start the Chrony service and enable to start on boot:

```bash
sudo systemctl start chrony.service && sudo systemctl enable chrony.service
```

Check the chrony service status:

```bash
systemctl status chronyd
● chrony.service - chrony, an NTP client/server
Loaded: loaded (/lib/systemd/system/chrony.service; enabled; vendor preset: enabled)
Active: active (running) since Wed 2023-08-30 13:41:11 EAT; 1min 8s ago
Docs: man:chronyd(8)
man:chronyc(1)
man:chrony.conf(5)
Process: 5458 ExecStart=/usr/sbin/chronyd $DAEMON_OPTS (code=exited, status=0/SUCCESS)
Main PID: 5460 (chronyd)
Tasks: 2 (limit: 3734)
Memory: 1.1M
CPU: 48ms
CGroup: /system.slice/chrony.service
├─5460 /usr/sbin/chronyd -F 1
└─5461 /usr/sbin/chronyd -F 1
```

## Step 3: Configure NTP server with Chrony
The /etc/chrony/chrony.conf is the default configuration file for the NTP server. Open this file in your preferred text editor and comment on the default pool before adding a list of NTP servers that are closest to your location.

```bash
sudo nano /etc/chrony/chrony.conf
```

Setting NTP pool servers in the configuration file:

```systemd
# Welcome to the chrony configuration file. See chrony.conf(5) for more
# information about usable directives.

# Include configuration files found in /etc/chrony/conf.d.
confdir /etc/chrony/conf.d

# Use Debian vendor zone.
# pool 2.debian.pool.ntp.org iburst
server 0.ke.pool.ntp.org
server 0.africa.pool.ntp.org
server 3.africa.pool.ntp.org
# Use time sources from DHCP.
sourcedir /run/chrony-dhcp

# Use NTP sources found in /etc/chrony/sources.d.
sourcedir /etc/chrony/sources.d

# This directive specify the location of the file containing ID/key pairs for
# NTP authentication.
keyfile /etc/chrony/chrony.keys
```

Activate NTP Service using the commands below:

```bash
sudo timedatectl set-ntp true
```

Then restart the chronyd service:

```bash
sudo systemctl restart chronyd
```

### Managing the chrony Service
Using the chronyc sources -v tool to look at source time-servers:

```bash
chronyc sources -v

.-- Source mode  '^' = server, '=' = peer, '#' = local clock.
/ .- Source state '*' = current best, '+' = combined, '-' = not combined,
| /             'x' = may be in error, '~' = too variable, '?' = unusable.
||                                                 .- xxxx [ yyyy ] +/- zzzz
||      Reachability register (octal) -.           |  xxxx = adjusted offset,
||      Log2(Polling interval) --.      |          |  yyyy = measured offset,
||                                \     |          |  zzzz = estimated error.
||                                 |    |           \
MS Name/IP address         Stratum Poll Reach LastRx Last sample               
===============================================================================
^- time.cloudflare.com           3   7   377    74  -4826us[-5147us] +/-   89ms
^* ntp0.icolo.io                 2   7   377    11  -1507us[-1852us] +/-   20ms
^- dbn-ntp.mweb.co.za            2   7   377    70  -1959us[-2282us] +/-  158ms
```

Using the following command to view software clock information:

```bash
chronyc tracking
Reference ID    : A077D8CE (ntp0.icolo.io)
Stratum         : 3
Ref time (UTC)  : Wed Jan 12 10:38:20 2022
System time     : 0.000397244 seconds slow of NTP time
Last offset     : -0.000344687 seconds
RMS offset      : 0.001311006 seconds
Frequency       : 40.576 ppm slow
Residual freq   : -0.089 ppm
Skew            : 7.024 ppm
Root delay      : 0.033961684 seconds
Root dispersion : 0.004330266 seconds
Update interval : 128.7 seconds
Leap status     : Normal
```

As per the above output, we get the following:

+ Reference ID: The reference ID and name to which the computer is currently synced.
+ Stratum: Number of hops to a computer with an attached reference clock.
+ Ref time: This is the UTC time at which the last measurement from the reference source was made.
+ System time: Delay of system clock from synchronized server.
+ Last offset: Estimated offset of the last clock update.
+ RMS offset: Long term average of the offset value.
+ Frequency: This is the rate by which the system’s clock would be wrong if chronyd is not correcting it. It is provided in ppm (parts per million).
+ Residual freq: Residual frequency indicating the difference between the measurements from reference source and the frequency currently being used.
+ Skew: Estimated error bound of the frequency.
+ Root delay: Total of the network path delays to the stratum computer, from which the computer is being synced.
+ Leap status: This is the leap status that can have one of the following values – normal, insert second, delete second or not synchronized.

+ Using the command below to view time-server statistics:

```bash
chronyc sourcestats

Name/IP Address            NP  NR  Span  Frequency  Freq Skew  Offset  Std Dev
==============================================================================
time.cloudflare.com        22  11   25m     +2.171      5.208  -3405us  3028us
ntp0.icolo.io              20   9   22m     -0.076      5.819  -8208ns  2222us
dbn-ntp.mweb.co.za         21  13   23m     +2.843      6.363   +803us  2810us
```

## Step 4: Configure Chrony NTP Clients

### Configure Client to Synchronize Time with Chrony NTP

Next, you need to allow clients on your network access to the server to synchronize time and date settings. Once again, head back to the configuration file.

On Debian:
```bash
sudo nano /etc/chrony/chrony.conf
```

On RHEL:
```bash

sudo nano /etc/chrony.conf
```

Append this line to specify the network subnet of which the NTP server is a part.

```bash
allow 192.168.54.0/24
```

![image_49.png](image_49.png)

Save the changes made and exit the configuration file.

Next, verify that your NTP server is using online NTP servers for time synchronization.

```bash
chronyc sources
```

![image_50.png](image_50.png)

For detailed output use the -v flag as shown.

```bash
chronyc sources -v
```

![image_51.png](image_51.png)

If you have Firewalld running, consider allowing NTP traffic by running the following commands.

```bash
sudo firewall-cmd  --add-service=ntp --permanent
sudo firewall-cmd --reload
```

### Configure Chrony NTP Client to Sync Time with NTP Server

The next thing to do is to configure a client system to receive time and date settings from the NTP server. Ensure that the client is in the same timezone as the NTP server.

You can configure the timezone on the command line as follows.

```bash
sudo timedatectl set-timezone America/Argentina/Mendoza
```

On the client, ensure that Chrony is installed as shown.

On Debian:

```bash
sudo apt install chrony
```

On RHEL:

```bash
sudo dnf install chrony
```

Once installed, start and enable the Chrony daemon.

```bash
sudo systemctl start chronyd
sudo systemctl enable chronyd
```

Next, we need to configure the client to sync date and time settings from the NTP server. As such, modify the configuration file:

On Debian:
```bash
sudo nano /etc/chrony/chrony.conf
```

On RHEL:
```bash

sudo nano /etc/chrony.conf
```

Comment out the pool address and add the following line where the IP address corresponds to that of the NTP server.

```bash
server 192.168.54.81
```

![image_52.png](image_52.png)

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

From the output below, It’s noticeable that the client is getting time and date settings from the NTP server.

![image_53.png](image_53.png)

Back at the server, you can verify the NTP clients. You should see the IP address of the NTP client listed as shown below.

```bash
sudo chronyc clients
```

![image_54.png](image_54.png)

