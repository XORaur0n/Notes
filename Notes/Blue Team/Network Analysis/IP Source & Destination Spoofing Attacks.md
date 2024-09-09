
**Things to think of during analysis:** 

	The source IP should always be from the internal network, if it isn't this is a sign of packet crafting. 

	The source IP for outbound traffic should always be from the internal network, if not this a sign of malicious traffic from inside the network. 


-----------------------------------------


**Packet Crafting Attacks:**

	Decoy Scanning: 

		To circumvent firewall rules, an attacker could change the source IP to be from inside the subnet.


	Random Source Attack DDOS: 

		An attacker will send lots of traffic from various IPs to the same port to a victim. 


	LAND Attacks (DOS): 

		An attacker will send a packet where the source and destination IP are the same confusing the host. Older hosts are usually the only ones vulnerable to this attack. 

	SMURF Attacks: 

		An attacker will send large amounts of ICMP packets to various hosts. The source IP is set to the victim's and every host that recieves the packet replies causing resource exhaustion for that victim. 


	Initialization Vector Generation: 

		While usually only legavy wireless networks are vulnerable, an attacker will capture, decrypt, craft, and re-inject a packet in order to build a decryption table for a stastical attack. 


-----------------------------------------


**Detecting Attacks:** 


	Decoy Scanning: 

		Look for:

			Initial Fragmentation from a fake address

			TCP traffic from a legitimate source address

			Look for responses from multiple closed ports, as the RST flags will point towards the attacker address. 


	
	Random Source Attack DDOS: 

		Look for: 

			Single port utilization from different hosts

			Incremental base port with a lack of randomization

			Identical length fields



	SMURF Attacks: 

		Look for: 

			An excessive amount of ICMP replies from one host to the affected host. Also pay attention to if the request has any data/fragmentation



	LAND Attacks: 

		Look for: 

			All base ports being occupied