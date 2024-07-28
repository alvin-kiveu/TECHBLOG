# CONFIGURING PPTP VPN IN UBUNTU 

PPtP VPN is a protocol that is used to establish a connection between a client and a server. It is used to create a secure connection to a remote network. In this tutorial, we will learn how to configure a PPTP VPN in Ubuntu.

## Prerequisites

- A server running Ubuntu.

## Step 1: Install PPTP Server

1. Update the package list.

```bash
sudo apt update && sudo apt upgrade
```

### INSTALL PPTP VPN SERVER

T o install PPTP VPN server on Ubuntu we need to install pptpd package. To do this open terminal and execute the following command:

```bash
apt-get install pptpd -y
```

```bash
update-rc.d pptpd defaults
```


### CONFIGURE PPTP VPN SERVER

Once the installation is complete we need to configure the PPTP server. To do this open terminal and edit the file /etc/pptpd.conf

```bash
nano /etc/pptpd.conf
```

Add the following lines in end of file:

```bash
localip 172.20.1.1
remoteip 172.20.1.2-254
```

Where localip is IP address of your VPN server and remoteip are IPs that will be assigned to clients that connect to it.

The Internet Assigned Numbers Authority (IANA) has assigned several address ranges to be used by private networks

Address ranges to be use by private networks are

Class A: 10.0.0.0 to 10.255.255.255

Class B: 172.16.0.0 to 172.31.255.255

Class C: 192.168.0.0 to 192.168.255.25

An IP address within these ranges is therefore considered non-routable, as it is not unique.


Next 

```bash
nano /etc/ppp/pptpd-options
```

Add the following lines in end of file:

```bash
ms-dns 8.8.8.8
ms-dns 8.8.4.4
```

Where ms-dns
is DNS server that we want to use. In this case we are using Google DNS servers.

Next we need to configure authentication. To do this open terminal and edit the file /etc/ppp/chap-secrets

```bash
nano /etc/ppp/chap-secrets
```

Add the following lines in end of file:


Format of this file is:

```bash
[Username] [Service] [Password] [Allowed IP Address]
```

if you want the ip address to be assigned automatically then use * in place of IP address.

format:

```bash
username * password *
```

Example:
  
```bash
alvo * s3cRet *
```


Where test is username, pptpd is service, test is password and * means that user can connect from any IP address.

And if you want to assign a specific IP / static address to a user then use the following format:

format:

```bash
test pptpd test ipaddress
```

Example:

```bash
alvo pptpd s3cRet 172.1.1.2
```

Restart the pptpd service

```bash
service pptpd restart
```

Where test is username, pptpd is service, test is password and ipaddress is the IP address that you want to assign to the user.

Next we need to configure IP forwarding. To do this open terminal and edit the file /etc/sysctl.conf

```bash
nano /etc/sysctl.conf
```

Uncomment the following line:

```bash
net.ipv4.ip_forward=1
```

Check if its okay by executing the following command:

```bash
sysctl -p
```


Next we need to configure firewall. To do this open terminal and execute the following commands:

```bash
iptables -I INPUT -p tcp --dport 1723 -m state --state NEW -j ACCEPT
```

```bash
iptables -I INPUT -p gre -j ACCEPT
```

```bash
iptables -t nat -I POSTROUTING -o eth0 -j MASQUERADE
```

Now Make you have change the ip adress according to your server ip address range that you have set in /etc/pptpd.conf before executing the command below.

```bash
iptables -I FORWARD -p tcp --tcp-flags SYN,RST SYN -s 172.20.1.0/24 -j TCPMSS  --clamp-mss-to-pmtu
```

## RESTART PPTP VPN SERVER

Now we have configured PPTP VPN server. To make these changes take effect we need to restart the PPTP server. To do this open terminal and execute the following command:

```bash
service pptpd restart
```


##  TO INSTALL PPTP WITH SCRIPT



```bash
apt-get install pptpd -y
update-rc.d pptpd defaults
echo "localip 172.1.1.1" >> /etc/pptpd.conf
echo "remoteip 172.1.1.2-254" >> /etc/pptpd.conf
echo "ms-dns 8.8.8.8" >> /etc/ppp/pptpd-options
echo "ms-dns 8.8.4.4" >> /etc/ppp/pptpd-options
echo "user101 pptpd test101 172.1.1.2" >> /etc/ppp/chap-secrets
service pptpd restart
echo "net.ipv4.ip_forward=1" >> /etc/sysctl.conf
sysctl -p
iptables -I INPUT -p tcp --dport 1723 -m state --state NEW -j ACCEPT
iptables -I INPUT -p gre -j ACCEPT
iptables -t nat -I POSTROUTING -o eth0 -j MASQUERADE
iptables -I FORWARD -p tcp --tcp-flags SYN,RST SYN -s 172.1.1.0/24 -j TCPMSS  --clamp-mss-to-pmtu
```

### ADD A PPPTP USER WITH SCRIPT

```bash
echo "username pptpd password ipaddress" >> /etc/ppp/chap-secrets
```

REPLACE username, password and ipaddress with your own values.

### HOW TO READ THE USER PPTP ACCOUT WITH PHP
  
```php
<?php
$file = fopen("/etc/ppp/chap-secrets", "r") or exit("Unable to open file!");
//Output a line of the file until the end is reached
while(!feof($file))
{
echo fgets($file). "<br>";
}
fclose($file);
?>
```

### HOW TO READ THE USER PPTP ACCOUT WITH BASH
  
```bash
cat /etc/ppp/chap-secrets
```

### GINING PERMITION

Reading 

```bash
sudo chmod a+r /etc/ppp/chap-secrets
```

Writting 

```bash
sudo chmod a+w /etc/ppp/chap-secrets
```


If you find this project useful, please consider making a donation. Any funds donated will be used to help further development on this project.

[![Donate](https://img.shields.io/badge/Donate-PayStack-brightgreen)](https://paystack.com/pay/oqwdgv9xck)



## Step 2: Configure PPTP Server
