# Mikrotik Static Configuration

This tutorial will guide you on how to configure a Mikrotik router to work as a static router. A static router is a device that forwards data packets between computer networks. It uses static routing tables to determine the best path for data packets to travel from the source to the destination.


### Step 1: Configure the Mikrotik Router

1. Open the Winbox application and log in to your Mikrotik router.

2. Go to the Open New Terminal window and enter the following commands to configure the static router:

a. Add a static route to the router:

```bash
/ip route add dst-address=1.1.1.1/32 gateway=192.168.88.1
```

b. Verify the static route:

```bash
/ip route print
```

c. Use the following commands to add a new static user with a specific IP address:

```bash
/ip dhcp-server lease add address=192.168.88.100 mac-address=XX:XX:XX:XX:XX:XX comment="Static User"
```


d. Add the Static User to the Bridge:

```bash
/interface bridge port add bridge=StaticBridge interface=ether2
```

e. Add the Static User to the Bridge:

```bash
/interface bridge port add bridge=StaticBridge interface=ether2
```

f. Assign the Static IP to the Bridge:

```bash
/ip address add address=192.168.88.1/24 interface=StaticBridge
```


g. Verify the Static User:

```bash
/ip dhcp-server lease print
```




