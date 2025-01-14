# Change DNS Servers

The syntax is as follows for the `/etc/resolv.conf`. Use the `cat/bat` or `more/less` command to view the file:

```bash
cat /etc/resolv.conf
```

Outputs:

```bash
nameserver {IP-OF-THE-DNS-1}
nameserver {IP-OF-THEISP-DNS-SERVER-2}
nameserver {IP-OF-THEISP-DNS-SERVER-3}
```

You can add maximum three DNS name servers.

## Change your DNS servers

Login as the root, enter:

```bash
sudo nano /etc/resolv.conf
```

Modify or enter nameserver as follows:

```
# IPv4 dns
nameserver 1.1.1.1
nameserver 8.8.8.8
# IPv6 dns
nameserver 2606:4700:4700::1111
```

Save and close the file. The `1.1.1.1 (IPv4)` and `2606:4700:4700::1111 (IPv6)` are Cloudflare DNS and `8.8.8.8` is Google DNS IP address. Of course you can add your ISP’s or your corporate DNS IPv4/IPv6 servers too. To test DNS configuration type any one of the following dig command or host command:

```bash
host google.com
host suse.com
dig google.com
ping google.com
nslookup www.cybercitib.biz
```

Sample outputs:

```bash
google.com has address 72.14.207.99
google.com has address 64.233.187.99
google.com has address 64.233.167.99
google.com mail is handled by 10 smtp4.google.com.
google.com mail is handled by 10 smtp1.google.com.
google.com mail is handled by 10 smtp2.google.com.
google.com mail is handled by 10 smtp3.google.com.
```

Suppose you see valid output such as an actual IP address or can ping to a remote server via hostname using the ping command, which means that the DNS is working for you. Also, make sure you have valid default gateway setup if you see the timeout error.

## Modern Linux distros

Most modern Linux distros use systemd and NetworkManager. For example, Linux distros such as Red Hat Enterprise Linux (RHEL), Debian, Ubuntu, Fedora, Arch, and many other distros use the nmcli command to configure networking, including the DNS. NetworkManager on these distros dynamically updates the `/etc/resolv.conf` file with the DNS nameservers settings. In other words, if you set DNS using the above method, they will vanish after reboot. Hence, you need to use the nmcli command.

### Step 1: Get device list

Try:

```bash
nmcli device status
```

Filter output using the grep/egrep command #

```bash
nmcli device status | grep -i ethernet
nmcli device status | grep -i wireguard
nmcli device status | grep -i wifi
```

The following outputs indicate that Ethernet device named “enp0s31f6” connected using “Wired connection 1” profile.

```bash
DEVICE        TYPE       STATE        CONNECTION
enp0s31f6     ethernet   connected    Wired connection 1
```

### Step 2: Setting up IPv4 or IPv6 DNS

Set the IPv4 and IPv6 DNS server addresses for “Wired connection 1” profile as follows:

```bash
sudo nmcli connection modify "Wired connection 1" ipv4.dns "1.1.1.1"
sudo nmcli connection modify "Wired connection 1" ipv6.dns "2606:4700:4700::1111"
```

Want to set up multiple IPV4 and IPV6 dns address in a single go? Try:

```bash
sudo nmcli connection modify "Wired connection 1" ipv4.dns "1.1.1.1 8.8.8.8"
sudo nmcli connection modify "Wired connection 1" ipv6.dns "2606:4700:4700::1111 2001:4860:4860::8888"
```

### Step 3: Activate DNS changes

After that up the device again to reload changes:

```bash
sudo nmcli connection down "Wired connection 1"
sudo nmcli connection up "Wired connection 1"
```

### Step 4: Verification

Use the host command or dig command utility to verify that name resolution works. For instance:

```bash
host cyberciti.biz
host nixcraft.com
```

Again, use the ping command:

```bash
ping -c4 www.cyberciti.biz
```

You can use the resolvectl command as follows to view status. For example:

```bash
Link 2 (enp0s31f6)
Current Scopes: DNS          
DefaultRoute setting: yes          
LLMNR setting: yes          
MulticastDNS setting: no           
DNSOverTLS setting: no           
DNSSEC setting: no           
DNSSEC supported: no           
Current DNS Server: 1.1.1.1
DNS Servers: 8.8.8.8
1.1.1.1
DNS Domain: ~.  
```

And that is how you set DNS on Linux.

## Summing up
BSD family of the operating system will use the `/etc/resolv.conf` including non-systemd Linux distros such as Alpine Linux. However, a systemd-based Linux distro comes with `NetworkManager`, and you need to use the `nmcli` command to set up the correct IPv4 or IPv6. For more info read the documentation using the man command or `--help` option command as follows:

```bash
man 5 resolv.conf
man nmcli #<--Linux+Systemd+NetworkManger only
man dig
man host
man ping
```

**Bibliography:** [https://www.cyberciti.biz/faq/howto-linux-bsd-unix-set-dns-nameserver/](https://www.cyberciti.biz/faq/howto-linux-bsd-unix-set-dns-nameserver/)