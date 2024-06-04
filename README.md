
![TOPLOGIE](https://github.com/c-youness/DHCP-AAA-SSH-DMZ-Static-NAT-and-ACLs/assets/114768920/e548efa6-8851-41c0-b33c-26ac305e32b0)
![config pc server asa](https://github.com/c-youness/DHCP-AAA-SSH-DMZ-Static-NAT-and-ACLs/assets/114768920/d078fbb2-1886-4a14-9fcd-f5195dc8d21f)

Objectives
=====
==> Configure basic ASA settings and interface security levels
===
==> Configure routing, address translation, and inspection policy 
==
==> Configure DHCP, AAA, and SSH,DMZ, Static NAT, and ACLs
==


Configure ASA Settings and Interface Security Using the CLI
-------
===>  Configure the ASA hostname as CCNAS-ASA
--
ciscoasa(config)#hostname CCNAS-ASA
--
===> Configure the domain name
 ---
CCNAS-ASA(config)#domain-name ccnasecurity.com
--
===> Configure the enable mode password.
-------
CCNAS-ASA(config)# enable password ciscoenpa55
--
===> Set the date and time
----
CCNAS-ASA(config)#clock set 14:30:00 Jun 4 2024
--------
Configure the inside and outside interfaces
-------
Configure a logical VLAN 1 interface for the inside network (192.168.1.0/24) and set the security level to the highest setting of 100.
===
CCNAS-ASA(config)# interface vlan 1
===
CCNAS-ASA(config-if)# nameif inside
===
CCNAS-ASA(config-if)# ip address 192.168.1.1 255.255.255.0
===
CCNAS-ASA(config-if)# security-level 100
===

Create a logical VLAN 2 interface for the outside network (209.165.200.224/29), set the security level to the lowest setting of 0, and enable the VLAN 2 interface.
==

CCNAS-ASA(config-if)# interface vlan 2
==
CCNAS-ASA(config-if)# nameif outside
==
CCNAS-ASA(config-if)# ip address 209.165.200.226 255.255.255.248
==
CCNAS-ASA(config-if)# security-level 0
==

Configure Routing, Address Translation, and Inspection Policy Using the CLI
===

CCNAS-ASA(config)# route outside 0.0.0.0 0.0.0.0 209.165.200.225
==

Configure address translation using PAT and network objects.
==
CCNAS-ASA(config)# object network inside-net
==
CCNAS-ASA(config-network-object)# subnet 192.168.1.0 255.255.255.0
==
CCNAS-ASA(config-network-object)# nat (inside,outside) dynamic interface
==
CCNAS-ASA(config-network-object)# end
==

===> Modify the default MPF application inspection global service policy.
--
Create the class-map, policy-map, and service-policy. Add the inspection of ICMP traffic to the policy map list using the following commands
====
CCNAS-ASA(config)# class-map inspection_default
====
CCNAS-ASA(config-cmap)# match default-inspection-traffic
====
CCNAS-ASA(config-cmap)# exit
====
CCNAS-ASA(config)# policy-map global_policy
====
CCNAS-ASA(config-pmap)# class inspection_default
====
CCNAS-ASA(config-pmap-c)# inspect icmp
====
CCNAS-ASA(config-pmap-c)# exit
====
CCNAS-ASA(config)# service-policy global_policy global
---

--
==> Configure DHCP, AAA, and SSH
==
Configure the ASA as a DHCP server.
---
Configure a DHCP address pool and enable it on the ASA inside interface.
-
CCNAS-ASA(config)# dhcpd address 192.168.1.5-192.168.1.36 inside
-
(Optional) Specify the IP address of the DNS server to be given to clients.
--
CCNAS-ASA(config)# dhcpd dns 209.165.201.2 interface inside
--
==>Enable the DHCP daemon within the ASA to listen for DHCP client requests on the enabled interface (inside).
==
CCNAS-ASA(config)# dhcpd enable inside
--
===> Configure AAA to use the local database for authentication
---
 Define a local user named admin by entering the username command. Specify a password of adminpa55.
---
CCNAS-ASA(config)# username admin password adminpa55
---
Configure AAA to use the local ASA database for SSH user authentication.
---
CCNAS-ASA(config)# aaa authentication ssh console LOCAL
---

==> Configure remote access to the ASA
--
CCNAS-ASA(config)# crypto key generate rsa modulus 1024
--
==> Configure the ASA to allow SSH connections from any host on the inside network (192.168.1.0/24) and from the remote management host at the branch office (172.16.3.3) on the outside network. 
---
CCNAS-ASA(config)# ssh 192.168.1.0 255.255.255.0 inside
---
CCNAS-ASA(config)# ssh 172.16.3.3 255.255.255.255 outside
---
CCNAS-ASA(config)# ssh timeout 10
---


Establish an SSH session from PC-C to the ASA (209.165.200.226). Troubleshoot if it is not successful.
==
PC> ssh -l admin 209.165.200.226
===
===> Establish an SSH session from PC-B to the ASA (192.168.1.1). Troubleshoot if  it is not successful.

PC> ssh -l admin 192.168.1.1
===

 ==> Configure a DMZ, Static NAT, and ACLs
 ==
 Configure the DMZ interface VLAN 3 on the ASA.
-- 
CCNAS-ASA(config)# interface vlan 3
===
CCNAS-ASA(config-if)# ip address 192.168.2.1 255.255.255.0
===
CCNAS-ASA(config-if)# no forward interface vlan 1
===
CCNAS-ASA(config-if)# nameif dmz
--
CCNAS-ASA(config-if)# security-level 70
---
==> Assign ASA physical interface E0/2 to DMZ VLAN 3 and enable the interface.
---
CCNAS-ASA(config-if)# interface Ethernet0/2
==
CCNAS-ASA(config-if)# switchport access vlan 3
===
Configure a network object named dmz-server and assign it the static IP address of the DMZ server (192.168.2.3). While in object definition mode, use the nat command to specify that this object is used to translate a DMZ address to an outside address using static NAT, and specify a public translated address of 209.165.200.227.
---
CCNAS-ASA(config)# object network dmz-server
---
CCNAS-ASA(config-network-object)# host 192.168.2.3
---
CCNAS-ASA(config-network-object)# nat (dmz,outside) static 209.165.200.227
---
CCNAS-ASA(config-network-object)# exit
===

Configure an ACL to allow access to the DMZ server from the Internet.
==
Configure a named access list OUTSIDE-DMZ that permits the TCP protocol on port 80 from any external host to the internal IP address of the DMZ server. Apply the access list to the ASA outside interface in the “IN” direction
===
CCNAS-ASA(config)# access-list OUTSIDE-DMZ permit icmp any host 192.168.2.3
===
CCNAS-ASA(config)# access-list OUTSIDE-DMZ permit tcp any host 192.168.2.3 eq 80
===
CCNAS-ASA(config)# access-group OUTSIDE-DMZ in interface outside
===

