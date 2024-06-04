![Toplologie](https://github.com/c-youness/ASA-VLAN-Port-Security-Line-Vty-/assets/114768920/98b97f21-da8d-4c80-bacc-74948208310e)

![image ASA](https://github.com/c-youness/ASA-VLAN-Port-Security-Line-Vty-/assets/114768920/d1b0c87f-6a85-420c-b54c-004515cfd172)

![TOPLOGIE](https://github.com/c-youness/ASA-VLAN-Port-Security-Line-Vty-/assets/114768920/1f1914de-aa20-4b0a-9fb8-2f2ff4f36d38)
![config pc server asa](https://github.com/c-youness/ASA-VLAN-Port-Security-Line-Vty-/assets/114768920/3fc29c4c-c16f-4052-904c-27968686cba6)

Objectives
=====
·        Configure basic ASA settings and interface security levels using CLI
--
·         Configure routing, address translation, and inspection policy using CLI
--
·         Configure DHCP, AAA, and SSH, DMZ,Static NAT ,ACLs
--

Configure ASA Settings and Interface Security Using the CLI
-------
  Configure the ASA hostname as CCNAS-ASA
--
ciscoasa(config)#hostname CCNAS-ASA
--
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
===> Configure the inside and outside interfaces.
------
Configure a logical VLAN 1 interface for the inside network (192.168.1.0/24) and set the security level to the highest setting of 100.
----

CCNAS-ASA(config)# interface vlan 1
--------
CCNAS-ASA(config-if)# nameif inside
--------
CCNAS-ASA(config-if)# ip address 192.168.1.1 255.255.255.0
--------
CCNAS-ASA(config-if)# security-level 100
--------
Create a logical VLAN 2 interface for the outside network (209.165.200.224/29), set the security level to the lowest setting of 0, and enable the VLAN 2 interface.
------------------

CCNAS-ASA(config-if)# interface vlan 2
-------------
CCNAS-ASA(config-if)# nameif outside
-------------
CCNAS-ASA(config-if)# ip address 209.165.200.226 255.255.255.248
-------------
CCNAS-ASA(config-if)# security-level 0
-------------

===> Configure Routing, Address Translation, and Inspection Policy Using the CLI
--

