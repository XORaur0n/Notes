

**TCP Connection Termination:** 

The attacker will spoof the source address to be the victim's. 

The attacker will modify the packet to contain the RST flag to terminate the connection

An attacker will specify that the destination port is the same as the one currently in use by another host. 

-----------------------------------------

TCP Connection Hijacking: 

Attacker will conduct sequence number prediction in order to inject malicious packets in the correct order. 

Attacker will spoof the source address to be the same as the victim's. 

The attacker will block ACKs from reaching the host in order to continue the hijacking. This can be done by delaying or outright denying the ACK packets. 

