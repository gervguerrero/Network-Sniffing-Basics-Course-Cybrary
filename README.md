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

## Scenario  

The scenario is that you are a Red Team Penetration Tester who gets physical access into a client's corprate building, and finds a sensitive communications closet with a network switch exposed. 
You plug an ethernet cable into the the switch into your portable laptop, and due to poor security controls gain access to the physical network.

**Lab Exercise Goals**

1. Determine the IP address of the router on this network.
2. Conduct passive sniffing until you see some ARP broadcasts – this will give you a target.
3. Conduct active sniffing via a MITM (Man-in-the-Middle) attack. Do this by poisoning the target’s ARP table so that it thinks you’re the router in this network.
4. Identify some credentials sent in plaintext, and use them to log in and retrieve some sensitive information. 
5. Identify another plaintext protocol, but with no credentials evident in the traffic - search for an alternate way in.
