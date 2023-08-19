# 👨‍💻 Network-Sniffing-Basics-Course-Cybrary 👨‍💻

This page showcases a walkthrough of Cybrary's Network Sniffing Basics Course in the context of Offensive Security.

Course: https://app.cybrary.it/browse/virtual-lab/network-sniffing-basics

![image](https://github.com/gervguerrero/Network-Sniffing-Basics-Course-Cybrary/assets/140366635/bbedaed2-2100-4992-9cc7-2599900e8928)

## Tools & Technologies Used 

- Ubuntu (Linux)
- Wireshark
- ARP Spoofing (arpspoof tool)
- Network Sniffing
- Man In the Middle Attack (MITM)
- Passive Collection 
- Red Teaming
- Reconnaissance and Network Mapping
- Password and Credential Harvesting
- Social Engineering
- Cookie Stealing/Cookie Exploitation
- Session Hijacking/Session Fixation
  
## Scenario 1

The scenario is that you are a Red Team Penetration Tester who gets physical access into a client's corporate building, and finds a sensitive communications closet with a network switch exposed. 
You plug an ethernet cable into the switch into your portable laptop, and due to poor security controls gain access to the physical network gain access via DHCP. 

**Lab Exercise Goals**

1. Determine the IP address of the router on this network.
2. Conduct passive sniffing until you see some ARP broadcasts – this will give you a target.
3. Conduct active sniffing via a MITM (Man-in-the-Middle) attack. Do this by poisoning the target’s ARP table so that it thinks you’re the router in this network.
4. Identify some credentials sent in plaintext, and use them to log in and retrieve some sensitive information. 
5. Identify another plaintext protocol, but with no credentials evident in the traffic - search for an alternate way in.

Running a custom script in the exercise with "clabnet" sets up the virtual machine to simulate network traffic:

![image](https://github.com/gervguerrero/Network-Sniffing-Basics-Course-Cybrary/assets/140366635/5b6667e8-7d04-4d64-8ec1-2bd01b01b829)

</br>

**1. Determine the IP address of the router on this Network**

Upon gaining access to the switch in the comms closet and plugging an ethernet cable from the switch to your personal laptop, DHCP provides the computer with an address and are able to see the router of the network in Ubuntu using the "route" command. The IP of the router is 192.168.0.1.

![image](https://github.com/gervguerrero/Network-Sniffing-Basics-Course-Cybrary/assets/140366635/b15f6b57-97c4-4efd-aff2-5a85d3621fab)

</br>

**2. Conduct passive sniffing until you see some ARP broadcasts – this will give you a target**

Using Wireshark to passively sniff/monitor the corporate network, a target victim can be identified by looking for ARP broadcasts to the router 192.168.0.1.

In this picture below, a victim IP of 192.168.0.10 can be seen making ARP broadcasts for the router 192.168.0.1:

![image](https://github.com/gervguerrero/Network-Sniffing-Basics-Course-Cybrary/assets/140366635/8528bb52-1116-438c-9f0f-3ffe80cff3d6)

</br>

**3. Conduct active sniffing via a MITM (Man-in-the-Middle) attack. Do this by poisoning the target’s ARP table so that it thinks you’re the router in this network.**

To prepare for ARP spoofing, the command "sysctl -w net.ipv4.ip_forward=1" is ran to enable IP forwarding on the attack machine:

![image](https://github.com/gervguerrero/Network-Sniffing-Basics-Course-Cybrary/assets/140366635/a0a8cbc4-7135-4d66-94ca-eac77505ebee)

Next is the actual ARP spoofing where the victim 192.168.0.10 will have a poisoned ARP table thinking the attacker machine's MAC address is the router's MAC address of 192.168.0.1

The command "arpspoof -i h1 -eth0 -t 192.168.0.10 192.168.0.1" will be used to specify the MAC address to inject (the one assigned to the h1-eth0 interface), the target whose ARP table you intend to poison (-t 192.168.0.10), and the IP address to associate with that MAC address (192.168.0.1):

![image](https://github.com/gervguerrero/Network-Sniffing-Basics-Course-Cybrary/assets/140366635/ae6d6354-cde6-42e5-824d-de24b5378a0e)

Looking at Wireshark after the change, activity from the victim 192.168.0.10 can be seen:

![image](https://github.com/gervguerrero/Network-Sniffing-Basics-Course-Cybrary/assets/140366635/9e76134a-7544-401f-a198-453e961787d1)


</br>

**4. Identify some credentials sent in plaintext, and use them to log in and retrieve some sensitive information.**

Looking in Wireshark, the protocols being passed on the network can be seen:

![image](https://github.com/gervguerrero/Network-Sniffing-Basics-Course-Cybrary/assets/140366635/f32331f6-1ac9-438f-ad1b-f0dff76dc4c2)

A plaintext protocol FTP can be seen in the listings. 

Focusing on FTP as a filter, the victim 192.168.0.10 exposes credentials user: ash, password: frisson-costson going to an FTP server of 192.168.1.200:

![image](https://github.com/gervguerrero/Network-Sniffing-Basics-Course-Cybrary/assets/140366635/3e1def99-92fd-4a21-8362-ce364d63fd1b)

</br>

![image](https://github.com/gervguerrero/Network-Sniffing-Basics-Course-Cybrary/assets/140366635/6734d484-75e2-44e5-9642-e10911a4c9ed)

</br>

![image](https://github.com/gervguerrero/Network-Sniffing-Basics-Course-Cybrary/assets/140366635/b2cbb04e-1804-4191-b04f-8db25412af7f)

</br>

Using the exposed credentials, the FTP server 192.168.1.200 is able to be used now:

![image](https://github.com/gervguerrero/Network-Sniffing-Basics-Course-Cybrary/assets/140366635/6a390f39-dfec-4836-8175-4c8ed9043b35)

Looking into the FTP server, a sensitive document "agreement.pdf" can be seen for use and is downloaded:

![image](https://github.com/gervguerrero/Network-Sniffing-Basics-Course-Cybrary/assets/140366635/e95e2f2d-2f9a-4cc1-8010-3cabac3b8546)

![image](https://github.com/gervguerrero/Network-Sniffing-Basics-Course-Cybrary/assets/140366635/d7b2f2a1-5d2c-4473-8ec0-9c40b2499859)

</br>

**5. Identify another plaintext protocol, but with no credentials evident in the traffic - search for an alternate way in.**

Unfortunately in this part of the lab, the script failed to simulate HTTP traffic in wireshark. This is how we would find another method of exploitation further into the network.

This is how the exercise should look while filtering for HTTP traffic:

![image](https://github.com/gervguerrero/Network-Sniffing-Basics-Course-Cybrary/assets/140366635/3c76a1ed-2e4d-476d-8cd4-c99080259192)

While my Wireshark caught all the other traffic, it saw no HTTP traffic:

![image](https://github.com/gervguerrero/Network-Sniffing-Basics-Course-Cybrary/assets/140366635/e06941a4-c269-426d-840a-c7ae1e924f8a)

The goal in this section is to follow a TCP stream of the insecure HTTP protocol with the client 192.168.0.10 going to a HTTP server of 192.168.1.100:

![image](https://github.com/gervguerrero/Network-Sniffing-Basics-Course-Cybrary/assets/140366635/da76ae6b-16ee-4188-a17b-4977f68b7bdd)

Using the TCP stream, a Session Cookie can be seen in the third line. From here the session cookie will be used as a Session Hijacking technique.

A Firefox plugin called "Cookie-Editor" is used to plug in the Session Cookie that was stolen, and will be used to gain the Session going to the 192.168.1.100 server:
(Note that the Server is down to the no HTTP bug in this exercise.)

![image](https://github.com/gervguerrero/Network-Sniffing-Basics-Course-Cybrary/assets/140366635/9b498a1c-84d7-45f3-ac80-64565e6f3bcf)

Below are the CTF-style questions from this lab:

![image](https://github.com/gervguerrero/Network-Sniffing-Basics-Course-Cybrary/assets/140366635/eada6667-f1d4-4cea-a248-d529fa27894b)

** Basic Security Remediations for this Lab**

- Use HTTPS to encrypt communication to prevent sniffing.
- Secure Cookie Handling with HTTP only flags and Secure Flags.
- Implement Secure Session Management with stronger session identifier algorithms, regenerate session IDs, create session timeouts,
- Use Secure FTP or FTP Secure
- Physical Port Security/Network Controls to stop unauthorized devices directly connecting by ethernet and gaining access via DHCP.
   
## Scenario 2 Walkthrough

From the Exercise:

You’ve just slid into a booth in the coffee shop in the target organization’s building and, to your surprise, it has an open Wi-Fi network! This is right around the time employees come in for lunch. In fact, it’s pretty happening right now, and more than a few are sitting at tables with laptops open. The airwaves should be pretty full right now – time to start sniffing and pulling some traffic out of thick air. You’re not here to eat or drink coffee; you’re here to sniff (passively).

**Part 1: Gather Information**

You’ll be seeking data from two protocols in this traffic.

One is the file-sharing protocol Server Message Block version 2 (SMB2). You’ll see a successful connection to a server and share (e.g., //server/share), and the username used to authenticate (e.g., jdoe).

![image](https://github.com/gervguerrero/Network-Sniffing-Basics-Course-Cybrary/assets/140366635/a3e9c0c8-d727-429f-8224-ba4b9c59387f)

![image](https://github.com/gervguerrero/Network-Sniffing-Basics-Course-Cybrary/assets/140366635/a525a69e-00de-413d-9fe3-806dbad26e1c)

This leaves you with ALMOST enough information to access the share yourself – you’re just missing a password. To that end…
You’ll notice an unencrypted chat protocol being used in this traffic. It should reveal a server (e.g., chat.purplefrogs.net), as well as a channel and channel key (e.g., #channel channelkey).

The unencrypted chat is Internet Relay Chat:

![image](https://github.com/gervguerrero/Network-Sniffing-Basics-Course-Cybrary/assets/140366635/05e9c0af-16e9-448c-8a10-a1c56f034703)

![image](https://github.com/gervguerrero/Network-Sniffing-Basics-Course-Cybrary/assets/140366635/57b6f6dd-7a29-4c27-8156-f941e5ff42c3)

![image](https://github.com/gervguerrero/Network-Sniffing-Basics-Course-Cybrary/assets/140366635/ddbe4a64-c9bb-4eb3-bb9d-7bea68bfb71a)

![image](https://github.com/gervguerrero/Network-Sniffing-Basics-Course-Cybrary/assets/140366635/db7cf962-7917-40e4-92df-7866bbb82eea)


This provides you with enough information to connect to the chat server and join the channel. You’ll also notice a username in this traffic, but you can ignore this – you don’t want to join as a user that’s currently active in this channel – doppelgangers are awkward and indiscrete. Plus, your goal is to impersonate the SMB2 user in order to social engineer their password.

![image](https://github.com/gervguerrero/Network-Sniffing-Basics-Course-Cybrary/assets/140366635/0ed520ff-3990-4daa-9432-3f4c9d192fe0)

At this point you should have:

![image](https://github.com/gervguerrero/Network-Sniffing-Basics-Course-Cybrary/assets/140366635/3d9eb46a-2b1a-4a78-b403-9a13cff5c528)

- Chat: irc.clab.net
- Channel: #devops azurediamond
- SMB Name: jgalt
- Server/Share: 192.168.1.200\lybrary

In the next part of this challenge, you will use this information to connect to the chat server, social engineer your way to a full set of credentials for the file share, and obtain the flag on the share.

**Part 2: Take Action**

Connect to the chat server and channel using the xchat client. To open xchat, execute xchat& in your Node: sta1 terminal. Then, specify the following details obtained in the previous section:

Nick name: username you identified from file-sharing traffic, e.g., jdoe

Networks: Add a new network called clabnet, then click Edit and add the server you discovered in the chat protocol traffic, e.g., chat.purplefrogs.net/6667. Then, close this window and click Connect.

Channel: After connecting, you’ll be prompted for the channel and channel key. Add these, separated by a space, e.g., #channel channelkey
Once you’ve connected to the channel, try saying “I forgot my password to <share>”, where share is the share you identified in the Server Message Block traffic, e.g., I forgot my password to myshare.
From the Node: sta1 terminal, execute smbclient //server/share -U <username> and then enter the password when prompted. Once connected, execute more flag.txt to get the flag for this section.

![image](https://github.com/gervguerrero/Network-Sniffing-Basics-Course-Cybrary/assets/140366635/e232dd3f-ab5d-47b3-8a3d-be4ac4f52cda)

Access to the chat:

![image](https://github.com/gervguerrero/Network-Sniffing-Basics-Course-Cybrary/assets/140366635/79e6ec58-7601-4958-a97f-76e042c710a5)

Prompt for #devops channel:

![image](https://github.com/gervguerrero/Network-Sniffing-Basics-Course-Cybrary/assets/140366635/119bfd83-92fc-4cf2-8308-3026af709af7)

Access to the #devops channel:

![image](https://github.com/gervguerrero/Network-Sniffing-Basics-Course-Cybrary/assets/140366635/7cb42758-f69b-473a-82fe-07fc6fb8db2c)

Once you’ve connected to the channel, try saying “I forgot my password to <share>”, where share is the share you identified in the Server Message Block traffic, e.g., I forgot my password to myshare.

Social Engineering in #devops channel:

![image](https://github.com/gervguerrero/Network-Sniffing-Basics-Course-Cybrary/assets/140366635/4824d805-12e3-4aff-befb-3b05a62af04d)

From the Node: sta1 terminal, execute smbclient //server/share -U <username> and then enter the password when prompted. Once connected, execute more flag.txt to get the flag for this section.

![image](https://github.com/gervguerrero/Network-Sniffing-Basics-Course-Cybrary/assets/140366635/21572d4d-1062-47e8-b368-f12d22d3917e)

Flag:

![image](https://github.com/gervguerrero/Network-Sniffing-Basics-Course-Cybrary/assets/140366635/a1247a01-629b-401c-992d-c5ddda9884a8)

CTF-style questions:

![image](https://github.com/gervguerrero/Network-Sniffing-Basics-Course-Cybrary/assets/140366635/1296f297-2ec0-46b7-afe7-ee65e906aa36)


![image](https://github.com/gervguerrero/Network-Sniffing-Basics-Course-Cybrary/assets/140366635/7889e642-40ea-48da-a036-22f8ac05b775)

**Basic Security Remediations for this Lab**
- Use SMB3 with encryption to prevent sniffing.
- Use encrypted chat services.
- Prevent putting passwords directly in chat applications. (Especially cleartext non encrypted chats.) Use other methods to send sensitive information and validate users before submitting. (Call Desk phone, use secure email, etc.) 
