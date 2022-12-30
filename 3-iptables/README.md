# This repo is about iptables
* Netfilter is a packet filtering inside a linux kernel
* Netfilter and iptables are often combind with single expression netfilter/iptables
* iptables belongs to user-space used to configure Netfilter




iptables use tables to organize its rules.\
 wihtin each tables, rules are further organize  with in seprate chains. \
 rules are placed with in a specific chain of a specific table.
 
 
## iptables chain

* INPUT ----> use for filter incoming packets. our host is destination.
* OUTPUT ----> used for filter outgoing packets, our host is source

* FORWARD ---> used for filter route packet, our host is a router 
* PREROUTING --->  use for DNAT (Port forwarding)

* POSTROUTING ---> use for SNAT (Masquerade)




