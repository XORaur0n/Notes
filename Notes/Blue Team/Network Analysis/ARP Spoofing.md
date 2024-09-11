

| Step | Description                                                                                                                                                                                                                                                 |
|------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| 1    | Consider a network with three machines: the victim's computer, the router, and the attacker's machine.                                                                                                                                                      |
| 2    | The attacker initiates their ARP cache poisoning scheme by dispatching counterfeit ARP messages to both the victim's computer and the router.                                                                                                               |
| 3    | The message to the victim's computer asserts that the gateway's (router's) IP address corresponds to the physical address of the attacker's machine.                                                                                                        |
| 4    | Conversely, the message to the router claims that the IP address of the victim's machine maps to the physical address of the attacker's machine.                                                                                                            |
| 5    | On successfully executing these requests, the attacker may manage to corrupt the ARP cache on both the victim's machine and the router, causing all data to be misdirected to the attacker's machine.                                                       |
| 6    | If the attacker configures traffic forwarding, they can escalate the situation from a denial-of-service to a man-in-the-middle attack.                                                                                                                      |
| 7    |  	By examining other layers of our network model, we might discover additional attacks. The attacker could conduct DNS spoofing to redirect web requests to a bogus site or perform SSL stripping to attempt the interception of sensitive data in transit. |

In order to detect ARP Spoofing utilize the following wireshark filters: 
		
		arp.opcode == 1 (Requests)
		arp.opcode == 1 (Replies)