# Network-Sniffing-Basics-Course-Cybrary

This page showcases a walkthrough of Cybrary's Network Sniffing Basics Course in the context of Offensive Security.

![image](https://github.com/gervguerrero/Network-Sniffing-Basics-Course-Cybrary/assets/140366635/bbedaed2-2100-4992-9cc7-2599900e8928)

## Tools & Technologies Used 

- Ubuntu (Linux)
- Wireshark
- ARP Spoofing (arpspoof tool)
- Network Sniffing
- Man In the Middle Atttack (MITM)
- Passive Collection 
- Red Teaming
- Reconnaissance and Network Mapping
- Password and Credential Harvesting
- Social Engineering
  
https://app.cybrary.it/browse/virtual-lab/network-sniffing-basics

## Scenario 1

The scenario is that you are a Red Team Penetration Tester who gets physical access into a client's corprate building, and finds a sensitive communications closet with a network switch exposed. 
You plug an ethernet cable into the the switch into your portable laptop, and due to poor security controls gain access to the physical network gain access via DHCP. 

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


