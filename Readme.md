# Multi-Site VOIP and Dial Peering Project

This project outlines a multi-site network incorporating both VOIP devices connected to PC's allowing both voice and data packets to be transmitted over the network via a collection of VLANs. This project will aim to showcase some of the basic competencies outlined within the CompTIA Network+ and Cisco CCNA certifications.

This network is based on the hub model for networks with a central router providing connectivity to all four sites. Each site contains a router, switch and a combination of IP phones and desktop PC's. Each router has a DHCP pool configured which allows the end user devices to be automatically assigned an IP address automatically. All four sites are interconnected with each other and routing is provided by the OSPF protocol.

## What is VOIP?
VOIP stands for Voice over Internet Protocol. This technology allows for phone and video calls to be made over the internet instead of via an analog phone network. VOIP services are great for organisations looking to set up telephony over their existing broadband networks. 

## What is OSPF?
OSPF stands for Open Shortest Path First. It is a common interior gateway protocol used for routing between internal networks. It is a link state protocol which attempts to find the best route based on throughput, reliability and round-trip time. Each link is considered to have its own cost and the lowest cost and the fastest path wins.

## Network Topology

## How it's Made
This project can be done entirely within Cisco Packet Tracer.

1. Construct your network topology as shown in the diagram. Use type 2811 for the router, type 2960 for the switch, and the default models for the IP phone (type 7960) and the PC. 
2. You will need to configure all routers with extra modules for serial connections to allow connectivity between sites.
3. Double click on the central router, navigate to the physical tab, locate the power switch and shut down the router, then find the 'NM4A/S' module and drag it onto the router. Then switch the router back on.
4. Double click on the router in Site A, navigate to the physical tab, locate the power switch and shut down the router, then find the 'WIC-1T' module and drag it onto the router. Then switch the router back on. Repeat for the remaining 3 sites.
5. Configure the switch and the routers for each of the sites.
6. Switch Commands:
- en, conf t, vlan 10, vlan 20, exit
- int range fa0/2-3, switchport mode access, switchport access vlan 20, switchport voice vlan 10
- exit, do wr
7. Router Commands:
-  `en`
-  `conf t`
-  `int fa0/1.10`
-  `encapsulation dot1Q 10`
-  `ip address 192.168.10.1 255.255.255.0`
- `exit`
- `int fa0/1.20`
- `encapsulation dot1Q 20`
- `ip address 192.168.20.1 255.255.255.0`
- `exit`
- `int se0/3/0`
- `ip address 1.1.1.1 255.255.255.0`
- `no shutdown, exit`
- `router ospf 1, network 192.168.10.0 0.0.0.255 a 0`
- `network 192.168.20.0 0.0.0.255 a 0`
- `network 1.1.1.0 0.0.0.255 a 0`
- `exit`
- `ip dhcp pool SiteA`
- `network 192.168.10.0 255.255.255.0`
- `default-router 192.168.10.1`
- `option 150 192.168.10.1`
- `ip dhcp pool Data-SiteA`
- `network 192.168.20.0 255.255.255.0`
- `default-router 192.168.20.1`
- `exit`
- `do wr`
8. Now to initialise the telephony service on the site router:
- `telephony-service`
- `max-dn 3`
`max-ephones 3`
- `ip source address 192.168.10.1 port 2000`
- `auto assign 1 to 2`
- `exit`
- `do wr`
9. Initialising dial-peering on the site router:
- `dial-peer voice 1 voip`
- `session-target 2.2.2.1`
- `destination-pattern 200.`
- `exit`
- `do wr`
10. Registering the E-Phones:
- `ephone-1`
- `number 1001`
- `ephone-2`
- `number 1002`
- `ephone-3`
- `number 1003`
11. Next, select each ip phone and insert the power adaptor into the back of the phone. The phones will begin to initialise. At the same time, navigate to the desktop tab in each of the PC's and enable DHCP in the IP Configuration application and an IP address should be assigned automatically.
12. Repeat in the same pattern for sites B, C and D.
13. On the central router, enter the following commands:
- `en`,
- `conf t`
- `int se1/0`
- `ip address 1.1.1.2 255.255.255.0`
- `exit`
- `int se1/1`
- `ip address 2.2.2.2 255.255.255.0`
- `exit`
- `int se1/2`
- `ip address 3.3.3.2 255.255.255.0`
- `exit`
- `int se1/3`
- `ip address 4.4.4.2 255.255.255.0`
- `exit`
- `int range se1/0-3`
- `no shutdown`
- `router ospf 1`
- `network 1.1.1.1 0.0.0.255 a 0`
- `network 2.2.2.1 0.0.0.255 a 0`
- `network 3.3.3.1 0.0.0.255 a 0`
- `network 4.4.4.1 0.0.0.255 a 0`
- `exit`
- `do wr`

## Reflections

 - Initially, DHCP was not working on site B, C and D after configuration but once dial-peering and OSPF was configured all systems were registering on the network normally
 - In order to ensure full interconnectivity across sites, dial-peering and OSPF must be configured individually on all 4 sites as well as on the host router or else some numbers will not dial
 - The project could be improved by automating the process of configuring the routers, switches and the IP phones. However, direct importation of configuration files is not supported on Cisco Packet Tracer so this would have to be done on bare metal hardware.

 















> Written with [StackEdit](https://stackedit.io/).
