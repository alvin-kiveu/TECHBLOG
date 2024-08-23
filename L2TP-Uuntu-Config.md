## Features

- Access and manage Mikrotik devices using a web browser or Winbox


## Installation for L2TP VPN

1. Update the package list.

```bash
sudo apt update && sudo apt upgrade
```

2. Download the script vpn.sh

```bash
wget https://get.vpnsetup.net -O vpn.sh && sudo sh vpn.sh
```

3. Follow the instructions on the screen. user and password and ip address of the mikrotik device


IPsec VPN server is now ready for use!

Connect to your new VPN with these details:

```bash
Server IP: 18.17.185.89
IPsec PSK: bDuW9nqPJoPPjXL9jzhH
Username: vpnuser
Password: KXqjY8BWetDYEbCZ
```

Write these down. You'll need them to connect!

VPN client setup: https://vpnsetup.net/clients

================================================

================================================

IKEv2 setup successful. Details for IKEv2 mode:

VPN server address: 18.17.185.89
VPN client name: vpnclient

Client configuration is available at:
/root/vpnclient.p12 (for Windows & Linux)
/root/vpnclient.sswan (for Android)
/root/vpnclient.mobileconfig (for iOS & macOS)

Next steps: Configure IKEv2 clients. See:
https://vpnsetup.net/clients



4. View or update the IPsec PSK


```bash
sudo nano /etc/ipsec.secrets
```

you will view the following

```bash
%any  %any  : PSK "bDuW9nsdqPJoPPjXL9jzhH"
```

<<<<<<< HEAD
=======
The PSK is the string between the quotes. You can change it to whatever you like. Just make sure it's a long, random string. Save the file and exit the editor. 

%any Means that the PSK is used for all connections. If you want to use different PSKs for different clients, you can replace %any with the client's IP address. For example,



```bash
10.10.0.12 %any  : PSK "GDuW9nqPJoPPjXL9jzhHjuiDFuiHio"
```

>>>>>>> 9946e0a91ca99bf081f36087059fa8fa4177ac94


5. View or update the VPN user password

```bash
sudo nano /etc/ppp/chap-secrets
```

you will view the following

```bash
# Secrets for authentication using CHAP
# client        server  secret                  IP addresses
vpnuser         l2tpd   KXqjY8dBWetDYEbCZ        *
```


6. To assing a static IP address to the VPN user the ip range must be defined in the file /etc/ppp/options.xl2tpd ed ip range 10.10.0.1-10.10.3.254



Add a user and password to the file  and assing a specific IP address

```bash
sudo nano /etc/ppp/chap-secrets
```

```bash
# Secrets for authentication using CHAP
# client        server  secret                  IP addresses
vpnuser1         l2tpd   KXqjY8BWetDYEbCZ        10.10.0.10
vpnuser2         l2tpd   AnotherPassword          10.10.0.11
```

<<<<<<< HEAD


 Restart the VPN service

 
=======
 Restart the VPN service
>>>>>>> 9946e0a91ca99bf081f36087059fa8fa4177ac94

```bash
sudo service xl2tpd restart
```


8. Restart Services
After making these changes, restart the IPsec and L2TP services to apply the new configuration:

```bash
sudo systemctl restart xl2tpd
sudo systemctl restart ipsec
```


To make the VPN server start automatically at boot, run the following command:

```bash
sudo systemctl enable xl2tpd
sudo systemctl enable ipsec
```

<<<<<<< HEAD
Forward TCP port 443 on the VPN server to the IPsec/L2TP client at 192.168.42.10.

# Get default network interface name

```bash
netif=$(ip -4 route list 0/0 | grep -m 1 -Po '(?<=dev )(\S+)')
iptables -I FORWARD 2 -i "$netif" -o ppp+ -p tcp --dport 443 -j ACCEPT
iptables -t nat -A PREROUTING -i "$netif" -p tcp --dport 443 -j DNAT --to 192.168.42.10
```

HOW TO READ THE USER L2TP ACCOUT WITH PHP
=======
HOW TO READ THE USER PPTP ACCOUT WITH PHP
>>>>>>> 9946e0a91ca99bf081f36087059fa8fa4177ac94

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


Grant permissions to the file

Reading the file /etc/ppp/chap-secrets with PHP requires the www-data user to have read permissions on the file. You can grant these permissions with the following command:

```bash
sudo chmod a+r /etc/ppp/chap-secrets
```

Write the PHP code in a file called read.php and access it from the browser to see the content of the file /etc/ppp/chap-secrets

```bash
sudo chmod a+w /etc/ppp/chap-secrets
```


