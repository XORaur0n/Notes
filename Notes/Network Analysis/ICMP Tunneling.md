
The attacker will add data for exfiltration within the data field of a common ICMP request in hopes that it will be able to masquerade as legitimate network traffic. 

-----------------------------------------


**Detecting Attacks:** 

	Filter for ICMP requests

	Analyze the data field

	Search for fragmentation & compare against a legitimate ICMP request. 

	Look at the size of the data within the packet, is it normal or way too big? 

	Look at transiting data, see if anything strange (credentials) are being pinged to an external/internal host. 

	Further, if an attacker is tryin
