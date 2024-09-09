
**Things to think of during analysis:** 

	The source IP should always be from the internal network, if it isn't this is a sign of packet crafting. 

	The source IP for outbound traffic should always be from the internal network, if not this a sign of malicious traffic from inside the network. 


-----------------------------------------


**Packet Crafting Attacks:**

	Decoy Scanning: 

		To circumvent firewall rules, an attacker could change the source IP to be from inside the subnet.


	Random Source Attack (DDOS): 

		An attacker will send lots of traffic from various IPs 


	LAND Attacks (DOS): 

		An attacker will send a packet where the source and destination IP are the same confusing the host. Older hosts are usually the only ones vulnerable to this attack. 