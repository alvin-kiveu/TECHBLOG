## Port Forwarding on Ubuntu

Port forwarding is a technique used to allow external devices to access services on a private network by directing traffic from a specific port on your public IP address to a specific port on a device within your internal network. It is often used to enable access to servers, applications, or services running on your network that would otherwise be inaccessible from the outside world.


## How Port Forwarding Works


Port forwarding works by creating a mapping between a port on your public IP address and a port on a device within your internal network. When an external device sends traffic to the public IP address and port that is being forwarded, the router or firewall that is performing the port forwarding will redirect the traffic to the specified internal device and port.


For example, if you have a web server running on port 80 on a device within your internal network and you want to make it accessible from the internet, you can set up port forwarding to direct traffic from port 80 on your public IP address to the internal IP address of the web server.


Check if IP Forwarding is Enabled:

```bash
sysctl net.ipv4.ip_forward
```

If the output is `net.ipv4.ip_forward = 0`, IP forwarding is disabled. To enable it, run:

```bash
sudo sysctl -w net.ipv4.ip_forward=1
```

To make the change persistent across reboots, edit the `/etc/sysctl.conf` file and add the following line:

```bash
net.ipv4.ip_forward = 1
```

Save the file and apply the changes by running:

```bash
sudo sysctl -p
```

## IPTABLES

iptables is a command-line utility in Linux that allows you to configure the packet filtering rules of the Linux kernel's built-in firewall. It is used to set up, maintain, and inspect the tables of IP packet filter rules in the Linux kernel.

How to Install iptables:

```bash
sudo apt install iptables
```

### Port Forwarding with iptables

To forward traffic from a specific port on your public IP address to a specific port on a device within your internal network, you can use the following iptables command:

```bash
sudo iptables -t nat -A PREROUTING -p tcp --dport <public_port> -j DNAT --to-destination <internal_ip>:<internal_port>
```

Replace `<public_port>` with the port on your public IP address that you want to forward, `<internal_ip>` with the internal IP address of the device you want to forward traffic to, and `<internal_port>` with the port on the internal device that you want to forward traffic to.


For example, to forward traffic from port 80 on your public IP address to port 80 on an internal device with IP address

```bash
sudo iptables -t nat -A PREROUTING -p tcp --dport 80 -j DNAT --to-destination
```

To view the current iptables rules, you can use the following command:

```bash
sudo iptables -t nat -L
```

To save the iptables rules so that they persist across reboots, you can use the following command:

```bash
sudo iptables-save > /etc/iptables/rules.v4
```

To load the saved iptables rules on boot, you can add the following line to the `/etc/rc.local` file:

```bash
iptables-restore < /etc/iptables/rules.v4
```

## UFW

UFW (Uncomplicated Firewall) is a user-friendly front-end for managing iptables firewall rules. It is designed to be easy to use and provides a simplified interface for configuring firewall rules on Ubuntu systems.

How to Install UFW:

```bash
sudo apt install ufw
```

### Port Forwarding with UFW

To forward traffic from a specific port on your public IP address to a specific port on a device within your internal network using UFW, you can use the following command:

```bash
sudo ufw route allow proto tcp from any to any port <public_port> forward <internal_ip>:<internal_port>
```

Replace `<public_port>` with the port on your public IP address that you want to forward, `<internal_ip>` with the internal IP address of the device you want to forward traffic to, and `<internal_port>` with the port on the internal device that you want to forward traffic to.


For example, to forward traffic from port 80 on your public IP address to port 80 on an internal device with IP address

```bash
sudo ufw route allow proto tcp from any to any port 80 forward
```

To view the current UFW rules, you can use the following command:

```bash
sudo ufw status
```

To save the UFW rules so that they persist across reboots, you can use the following command:

```bash
sudo ufw reload
```

## Conclusion

Port forwarding is a useful technique for enabling external devices to access services on your private network. By setting up port forwarding, you can make servers, applications, or services running on your network accessible from the internet. Whether you use iptables or UFW, port forwarding can be configured to direct traffic from a specific port on your public IP address to a specific port on a device within your internal network.















