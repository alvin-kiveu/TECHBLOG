## Mikrotik PPPoE Configuration

This tutorial will guide you on how to configure a Mikrotik router to work as a PPPoE server. PPPoE (Point-to-Point Protocol over Ethernet) is a network protocol used to establish a direct connection between two nodes on an Ethernet network. It is commonly used by ISPs to manage customer connections.


### Step 1: Configure the Mikrotik Router

1. Open the Winbox application and log in to your Mikrotik router.

2. Go to the Open New Terminal window and enter the following commands to configure the PPPoE server:

a. Create a new Pool for the PPPoE server:


```bash
/ip pool add name=UMS-PPPoE-Pool ranges=172.14.0.2-172.14.3.254
```

b. Create a new PPPoE server profile:

```bash
/ppp profile add name=UMS-PPPoE-Profile local-address=172.14.0.1 remote-address=UMS-PPPoE-Pool
```

c. Create a new PPPoE server interface:

```bash
/interface pppoe-server server add service-name=UMS-PPPoE-Service default-profile=UMS-PPPoE-Profile disabled=no
```


If you using Radius server for authentication, you can add the following commands:

Add the Radius server authentication:

```bash
/radius add address=radius-server-ip secret=radius-secret service=hotspot timeout=3000ms
```

Replace `radius-server-ip` with the IP address of your Radius server and `radius-secret` with the secret key for the Radius server.


Allow the PPPoE server to use the Radius server for authentication:

```bash
/ppp profile set UMS-PPPoE-Profile use-radius=yes
```

if you want to use the Your Mikrotik router for authentication, you can just Proceed to the next step of adding PPPoE user, but for the Radius server you can add on your Radius server.

add the PPPoE Secret:

```bash
/ppp secret add name=pppoe-user password=pppoe-password service=ppp profile=UMS-PPPoE-Profile
```

Replace `pppoe-user` with the username you want to use for the PPPoE connection and `pppoe-password` with the password for the user.

You can add multiple users by repeating the command with different usernames and passwords.

And you can Give User Diffrent Mbps by adding the following command:

First Create the Profile for The specific Mbps:

```bash
/queue simple add name=UMS-PPPoE-Profile target=192.168.1.10/32 max-limit=10M/20M
```

Replace `

Then add the user to the profile:

```bash
/ppp secret set pppoe-user profile=UMS-PPPoE-Profile
```

Replace `pppoe-user` with the username you want to assign the profile to.

### Step 2: Test the PPPoE Connection


1. Connect a device to the Mikrotik router using an Ethernet cable.

2. Open the network settings on the device and configure it to use PPPoE with the username and password you created in the previous step.

3. Test the connection by trying to access the internet on the device.

If the connection is successful, you have configured the Mikrotik router to work as a PPPoE server.


### Conclusion

In this tutorial, you learned how to configure a Mikrotik router to work as a PPPoE server. This setup allows you to manage customer connections and provide internet access through a direct Ethernet connection. You can further customize the configuration by adding more users and profiles to meet your specific requirements.


### Support the Blog

If you find the tutorials and articles helpful, consider supporting the blog with a donation. Your contributions help maintain the site and keep the content flowing.




Happy Networking! :smiley:





