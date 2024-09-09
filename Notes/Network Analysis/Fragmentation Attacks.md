
**Commonly Abused Packet Fields:** 

Length: 

This field contains the overall length of the IP header.

Total Length: 

This field specifies the entire length of the IP packet, including any relevant data.

Fragmented Offset: 

In many cases when a packet is large enough to be divided, the fragmentation offset will be set to provide instructions to reassemble the packet upon delivery to the destination host.

Source & Destination IPs: 

These fields contain the origination (source) and destination IP addresses for the two communicating hosts.

-----------------------------------------

Fragmentation Abuse: 


Fragmentation exists to allow legitimate networks a means to communicate large data sets by splitting packets and reassembling them upon delivery. 

Attackers abuse fragmentation for the following reasons: 

	IDS/IPS Evasion: 

		If an IDS/IPS does nto reassemble fragmented packets, an attacker could fragment their packets and have them reassembled upon delivery. 


	Firewall Evasion: 
		If the firewall does not reassemble packets during travel, the same thing as the above could occur. 


	Firewall/IPS/IDS Resource Exhaustion: 

		If an attacker fragmented packets of a very small MTU (10, 15,20, etc.), they may not be reassembled due to rsource limnitations. 


	Denial of Service: Old hosts are vulnerable to packets exceeding 65,535 bytes and would be DOS'd if an attacker sent them


-----------------------------------------


**Detecting Attacks:** 

