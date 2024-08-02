# Mikrotik Static Configuration

This tutorial will guide you on how to configure a Mikrotik router to work as a static router. A static router is a device that forwards data packets between computer networks. It uses static routing tables to determine the best path for data packets to travel from the source to the destination.



### Step 1: Configure the Mikrotik Router


Open the Winbox application and log in to your Mikrotik router.

Go to the Open New Terminal window 

a. Create a Bridge Interface for Static IF you don't have one IF you have one you can skip this step:

```bash
/interface bridge add name=UMS_Bridge_Static
```

# Add a Static IP Address to the Bridge Interface:

```bash
/ip address add address=41.18.1.1/24 interface=UMS_Bridge_Static
```


# # Configure NAT
  
  ```bash
/ip firewall nat add chain=srcnat out-interface=UMS_Bridge_Static action=masquerade
```



# Create simple queue for 10 Mbps user

```bash
/queue simple add name=10mbps_user target=41.18.1.10/32 max-limit=10M/10M
```


# Create simple queue for 4 Mbps user

```bash
/queue simple add name=4mbps_user target=41.18.1.11/32 max-limit=4M/4M
```


## Assing bridge to the ports

```bash
/interface bridge port add bridge=UMS_Bridge_Static interface=ether2
```

You can assign the bridge to the ports you want to use for the static router.

### Step 2: Add One Computer to the Network

Connect a computer to the Mikrotik router using an Ethernet cable.

Configure the computer's network settings to use the following IP address:


```bash
IP Address: 41.18.1.2
Subnet Mask: 255.255.255.0
Gateway: 41.18.1.1
DNS Server: You can use a public DNS server like 8.8.8.8 (Google DNS) or any DNS server of your choice.
```
















