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
/ip address add address=192.168.1.1/24 interface=UMS_Bridge_Static
```


# # Configure NAT
  
  ```bash
/ip firewall nat add chain=srcnat out-interface=UMS_Bridge_Static action=masquerade
```



# Create simple queue for 10 Mbps user

```bash
/queue simple add name=10mbps_user target=192.168.1.10/32 max-limit=10M/10M
```


# Create simple queue for 4 Mbps user

```bash
/queue simple add name=4mbps_user target=192.168.1.11/32 max-limit=4M/4M
```


## Assing bridge to the ports

```bash
/interface bridge port add bridge=UMS_Bridge_Static interface=ether2
```

You can assign the bridge to the ports you want to use for the static router.

### Step 2: Configure the Static Routes

Go to the IP menu and select the Routes option.

Add a new route by clicking on the + sign.

Enter the destination IP address, gateway IP address, and the interface you want to use for the static route.

Click on the Apply button to save the changes.

### Step 3: Test the Static Router

To test the static router, connect two computers to the Mikrotik router.

Set the IP address of the first computer to the IP address of the Mikrotik router.

Set the IP address of the second computer to the IP address of the destination network.

Ping the destination IP address from the first computer to test the static router.

If the static router is configured correctly, you should be able to ping the destination IP address successfully.

That's it! You have successfully configured a Mikrotik router to work as a static router. You can now use the static router to forward data packets between computer networks.

```














