# INFO 341 Final Project RPi LAN/WAN

*Julia Zaratan, Bonnie Nguyen  
INFO 341  
4 June 2018*

## Final RPi LAN/WAN Deliverables
__Description of network:__
Our network has one ISP and one edge network. Both of them include their own mail servers, and both have private and public DNS zones. The ISP provides the edge network with connectivity to the internet through wlan0. There is BGP peering between the ISP and edge routers. 

__End state:__ We were able to get the ISP to send an email to gradebook.pi, although it did not get the response which might have had to do with the gradebook pi. We were able to connect our pi’s together using eth1 on the ISP and ping each other, in addition to dig each other’s servers (authoritative and mail). Some design decisions we had to make was configure the edge network’s wlan0 to work as an access point since we did not have an additional ethernet cable and adapter. The ISP provides internet to the edge network.  The edge pi was able to access mail through the ip addresses. However, we got stuck on getting the mail working properly on the edge pi: cannot access http://mail.sangria.pi or http://sangria.local.

__Summary of challenges__  
* Getting the rainloop container running on ISP
  + Kept getting the error that it could not cd into /var/www/rainloop
  + Had to delete all the other images that partially loaded that were randomly named
* Testing/knowing if it would work when connected
  + Didn’t have extra USB-ethernet adapters before making the edge a WAP
* Only had 2 team members
* Battery pack running out
* Eduroam internet had dns problems
  + Edited /etc/bind/named.conf.options and changed dnssec-validation to no
  + DNSSEC validation is disabled, and recursive server will behave in the "old fashioned" way of performing insecure DNS lookups.
* Changing “localhost” to 127.0.0.1 
* Match-destinations didn’t work, change to match-clients
* Getting internet to edge network through ISP
  + Had to advertise 0.0.0.0/0 on ISP and got it working
* Problems with edge mail
  + Sangria.local and mail.sangria.pi is not accessible
  + Tried rebooting, removing and adding back docker containers
  + Ran named-checkzone sangria.pi /etc/bind/zones/db.sangria.pi
  + Fixed error after figuring out sangria was spelled wrong
  + Looked at named.conf.local and checked DNS servers 

__Network Diagram__
![Network Diagram](https://github.com/bonngyn/info341/blob/master/network%20diagram.png)

__Configuration Summary__  
_ISP_  
Connections to external networks or devices
* Interface / IP associations
* Eth0 - connect to laptop
* Eth1 - connect to edge network
* Dummy0 - authoritative
* Dummy1 - 10.10.10.10 tld
* Dummy2 - mail

Containers or other sub-components
* Rainloop
* Postfix
* Dovecot
* dotpi

Service / IP associations
* 172.23.44.130: Authoritative
* 172.23.44.131: Mail
* 172.23.44.0/22: Network
* 172.23.44.129: BGP Router ID
* BGP Router ASN: 65100
* 10.10.10.10 .pi TLD
 

_Edge_  
Connections to external networks or devices
* Connected to eth1 - 172.23.44.1

Interface/ IP associations
* Eth0 - Connect to Laptop 
* Eth1 - Connect to ISP 
* Dummy0 - Connect to Authoritative
* Dummy1 - Connect to mail
* Wlan0 - Wireless access point

Containers or Other sub-components 
* Dovecot
* Rainloop
* Postfix 

Service/ IP associations 
* Mail - 172.23.45.130

__Github repository with links to configuration for each device__ 

All files on master branch  

_Pull request includes annotated summary of our configuration_
https://github.com/bonngyn/info341/pull/1
