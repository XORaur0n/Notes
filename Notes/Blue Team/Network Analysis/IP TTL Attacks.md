
The attacker will craft a packet with an intentionally low TTL (1,2,3, etc.)

Every host that the packet traverses, the TTL will decrease by one until it reaches 0. 

When it hits zero the packet will be discarded. The attacker is trying to get it discarded before it is detected. 

When the packets expire, the routers along the path generate ICMP Time Exceeded messages and send them back to the source address



-----------------------------------------


**Detecting Attacks:** 


	Look for extremly low TTL packets


