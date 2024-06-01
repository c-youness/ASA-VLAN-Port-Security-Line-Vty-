![toplologie](https://github.com/c-youness/ASA-VLAN-Port-Security-Line-Vty-/assets/114768920/de1a0395-eb54-43cd-9277-874540d644c7)


![Config interface Partie 1](https://github.com/c-youness/ASA-VLAN-Port-Security-Line-Vty-/assets/114768920/843fa7b6-c4ae-461f-97cf-50a870828b48)

Router 1
======
R1(config)#interface S 0/1/0
========
R1(config-if)#ip add 10.1.1.2 255.255.255.252
========
R1(config-if)#no shutdown
========
R1(config-if)#exit 
========
R1(config)#interface S 0/1/1
========
R1(config-if)#ip add 10.2.2.2 255.255.255.252
========
R1(config-if)#no shutdown
========

Router 2
======
R2(config)#interface S 0/1/0
======
R2(config-if)#ip add 10.1.1.1 255.255.255.252
======
R2(config-if)#no shutdown
======
R2(config-if)#exit
======
R2(config)#interface f 0/0
======
R2(config-if)#ip add 192.168.2.1 255.255.255.0
======
R2(config-if)#no shutdown
======
Router 3
======
R3(config)#interface S 0/1/0
======
R3(config-if)#ip add 10.2.2.1 255.255.255.252
======
R3(config-if)#no shutdown
======
R3(config-if)#exit 
======
R3(config)#interface f 0/0
======
R3(config-if)#ip add 172.16.3.1 255.255.255.0
======
R3(config-if)#no shutdown
======

ASA
===
==> configure the hostname and domain name
---
ciscoasa(config)#hostname ChalYouness
---
ChalYouness(config)#domain-name C.Youness.ma
---
configure the enable mode password
--
ChalYouness(config)#enable password YO@UN@ESS@2024
---
==> Set the date and time
---
ChalYouness(config)#clock set 15:30:00 Jun 1 2024
---------
ChalYouness(config)#inter vlan 1
---------
ChalYouness(config-if)#ip add 192.168.1.1 255.255.255.0
---------
ChalYouness(config-if)#nameif inside
---------
ChalYouness(config-if)#security-level 100
---------
ChalYouness(config-if)#exit
---------
ChalYouness(config)#inter vlan 2
---------
ChalYouness(config-if)#nameif  outside
---------
ChalYouness(config-if)#security-level 0
---------
ChalYouness(config-if)#ip add 209.165.200.224 255.255.255.248
---------


