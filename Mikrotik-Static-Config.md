# Mikrotik Static Configuration

This tutorial will guide you on how to configure a Mikrotik router to work as a static router. A static router is a device that forwards data packets between computer networks. It uses static routing tables to determine the best path for data packets to travel from the source to the destination.



### Step 1: Configure the Mikrotik Router


Open the Winbox application and log in to your Mikrotik router.

Go to the Open New Terminal window 

a. Create a Bridge Interface for Static IF you don't have one IF you have one you can skip this step:

```bash
/interface bridge add name=UMS_Bridge_Static
```

b. Assign the IP address to the bridge interface

```bash
/ip address add address=192.168.4.1/24 interface=StaticBridge
```

c. Create a DHCP pool that will define the range of IP addresses to be provided.

```bash
/ip pool add name=StaticPool ranges=192.168.4.10-192.168.4.100
```
d. Create a DHCP network entry that includes the gateway, DNS, and other options.

```bash
/ip dhcp-server network add address=192.168.4.0/24 gateway=192.168.4.1 dns-server=8.8.8.8
```

**Note:** Remember to edit the Netmask to 24 if you are using a different subnet.

e. Set up the DHCP server on the interface connected to the AP..

```bash
/ip dhcp-server add name=CloudTikdhcp interface=StaticBridge address-pool=StaticPool disabled=no
```

f. add a client ad assign to A SPECIFIC Speed

 (i) Client 1
   
```bash
/queue simple add name=Client1 target=192.168.4.10/32 max-limit=5M/5M
```

 (ii) Client 2

```bash
/queue simple add name=Client2 target=192.168.4.11/32 max-limit=5M/5M
```


g. If you want to assign a static IP address to a specific client, you can do so by creating a DHCP leasec add link it to the MAC address of the client.

  (i) Client 1

```bash
/ip dhcp-server lease add address=192.168.4.10 mac-address=XX:XX:XX:XX:XX:XX comment="Client1"
```

  (ii) Client 2

```bash
/ip dhcp-server lease add address=192.168.4.11 mac-address=YY:YY:YY:YY:YY:YY comment="Client2"
```


## hOW TO CONNECT THE AP(ACCESS POINT) WITH INTERNET USING STATIC ROUTER

For client 1 usert the following configuration

```bash
IP Address: 192.168.4.10
Subnet Mask: 255.255.255.0
Gateway: 192.168.4.1
DNS: 8.8.8.8
```

#Conclusion


This tutorial has shown you how to configure a Mikrotik router to work as a static router. By following these steps, you can set up a static router that forwards data packets between computer networks. This will help you to create a stable and reliable network that can handle a large amount of traffic. If you have any questions or need further assistance, please feel free to ask in the comments section below.





















