# CONFIGURING L2TP VPN IN UBUNTU 

L2TP VPN is a protocol that is used to establish a connection between a client and a server. It is used to create a secure connection to a remote network. In this tutorial, we will learn how to configure an L2TP VPN in Ubuntu.


## Prerequisites

- A server running Ubuntu.

## Step 1: Install L2TP Server

1. Update the package list.

```bash
sudo apt update && sudo apt upgrade
```

### INSTALL REQUIRED PACKAGES

To install L2TP VPN server on Ubuntu we need to install the following packages. To do this open terminal and execute the following command:

```bash
sudo apt-get install strongswan xl2tpd ppp lsof -y
```

### CONFIGURE L2TP VPN SERVER

Once the installation is complete we need to configure the L2TP server. To do this open terminal and edit the file /etc/ipsec.conf

```bash
sudo nano /etc/ipsec.conf
```

Add the following configuration:

```bash
config setup
    charon {
        ikelifetime=60m
        keylife=20m
        rekeymargin=3m
        keyingtries=1
        authby=psk
    }

conn %default
    keyexchange=ikev1
    authby=psk
    keyingtries=1
    ikelifetime=60m
    keylife=20m
    rekeymargin=3m
    reauth=no
    dpdaction=clear
    dpddelay=35s
    dpdtimeout=150s
    dpdrestart=120s

conn L2TP-IPsec
    also=L2TP
    left=%defaultroute
    leftprotoport=17/1701
    right=%any
    rightprotoport=17/%any
    auto=add
```


Set up the IPsec secrets:


```bash
sudo nano /etc/ipsec.secrets
```

Add the following configuration:

```bash
: PSK "your_pre_shared_key"
```

Replace `your_pre_shared_key` with your own pre-shared key.

To get a random pre-shared key, you can use the following command:

```bash
openssl rand -base64 32
```

it will generate a random key that you can use as your pre-shared key just like this:

```bash
: PSK "X8b1Q6J6Q6J6Q6J6Q6J6Q6J6Q6Q6Q6J"
```

### CONFIGURE L2TP VPN SERVER

Once the installation is complete we need to configure the L2TP server. To do this open terminal and edit the file /etc/xl2tpd/xl2tpd.conf

```bash
sudo nano /etc/xl2tpd/xl2tpd.conf
```

Add the following configuration:

```bash
[global]
listen-addr = 0.0.0.0
ipsec saref = yes

[lns default]
ip range = 192.168.1.100-192.168.1.200
local ip = 192.168.1.1
length bit = 24
name = L2TP-Pool
ppp debug = yes
pppoptfile = /etc/ppp/options.xl2tpd
require chap = yes
refuse pap = yes
require authentication = yes
```

### CONFIGURE PPP OPTIONS

Next, we need to configure the PPP options. To do this open terminal and edit the file /etc/ppp/options.xl2tpd

```bash
sudo nano /etc/ppp/options.xl2tpd
```

Add the following configuration:

```bash
require-mschap-v2
refuse-pap
refuse-chap
refuse-mschap
name l2tpd
password mypassword123
ms-dns 8.8.8.8
ms-dns 8.8.4.4
```

Replace `mypassword123` with your own password.


### Set up PPP secrets:

Next, we need to configure the PPP secrets. To do this open terminal and edit the file /etc/ppp/chap-secrets

```bash
sudo nano /etc/ppp/chap-secrets
```

Add the following configuration:

```bash
# Secrets for authentication using CHAP
# client    server    secret            IP addresses
username    l2tpd    password123        *
```

Replace `username` with your own username and `password123` with your own password.

or add with terminal command:\

```bash
echo "username l2tpd password123 *" | sudo tee -a /etc/ppp/chap-secrets
```

### Enable IP Forwarding

To enable IP forwarding, open terminal and edit the file /etc/sysctl.conf

```bash
sudo nano /etc/sysctl.conf
```

Uncomment the following line:

```bash
net.ipv4.ip_forward=1
```

Apply the changes:

```bash
sudo sysctl -p
```

### Configure Firewall Rules

To allow traffic to pass through the

```bash
sudo iptables -A INPUT -p udp --dport 500 -j ACCEPT
sudo iptables -A INPUT -p udp --dport 4500 -j ACCEPT
sudo iptables -A INPUT -p udp --dport 1701 -j ACCEPT
sudo iptables -A INPUT -p esp -j ACCEPT
sudo iptables -A INPUT -p ah -j ACCEPT
sudo iptables -A INPUT -m policy --dir in --pol ipsec --proto esp -j ACCEPT
sudo iptables -A INPUT -m policy --dir out --pol ipsec --proto esp -j ACCEPT
sudo iptables -t nat -A POSTROUTING -o eth0 -j MASQUERADE
```

### Restart Services

To apply the changes, restart the services:

```bash
sudo systemctl restart strongswan
sudo systemctl restart xl2tpd
sudo systemctl enable strongswan
sudo systemctl enable xl2tpd
```

### Check the Status

To check the status of the services, run the following commands:

```bash
sudo systemctl status strongswan
sudo systemctl status xl2tpd
```






