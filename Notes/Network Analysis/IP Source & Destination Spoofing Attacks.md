
Things to think of during analysis: 

	The source IP should always be from the internal network, if it isn't this is a sign of packet crafting. 

	The source IP for outbound traffic should always be from the internal network, if not this a sign of malicious traffic from inside the network. 


-----------------------------------------


Packet Crafting Attacks

	Decoy Scanning: 

		To circumvent firewall rules, an attacker could change the source IP to be from inside the subnet. In order to learn more information about hosts in other subnets. 