
Given there is so much DNS traffic on the network, sometimes it can be hard to filter to see the weird stuff. 


-----------------------------------------


**DNS Forward Lookup:** 

Used when the client knows the FQDN, but not the IP address. 

| Step                                   | Description                                                                                                                                                                                      |
|----------------------------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| 1. Query Initiation                    | When the user wants to visit something like search.google.com it initiates a DNS forward query.                                                                                                  |
| 2. Local Cache Check                   | The client then checks its local DNS cache to see if it has already resolved the domain name to an IP address. If not it continues with the following.                                           |
| 3. Recursive Query                     | The client then sends its recursive query to its configured DNS server (local or remote).                                                                                                        |
| 4. Root Servers                        | The DNS resolver, if necessary, starts by querying the root name servers to find the authoritative name servers for the top-level domain (TLD). There are 13 root servers distributed worldwide. |
| 5. TLD Servers                         | The root server then responds with the authoritative name servers for the TLD (aka .com or .org)                                                                                                 |
| 6. Authoritative Servers               | The DNS resolver then queries the TLD's authoritative name servers for the second-level domain (aka google.com).                                                                                 |
| 7. Domain Name's Authoritative Servers | Finally, the DNS resolver queries the domains authoritative name servers to obtain the IP address associated with the requested domain name (aka search.google.com).                             |
| 8. Response                            | The DNS resolver then receives the IP address (A or AAAA record) and sends it back to the client that initiated the query.                                                                       |

-----------------------------------------


**DNS Reverse Lookup:**

Used when the client knows the IP address but wants to find the FQDN.


| Step                    | Description                                                                                                                                                                                                           |
|-------------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| 1. Query Initiation     | The client sends a DNS reverse query to its configured DNS resolver (server) with the IP address it wants to find the domain name.                                                                                    |
| 2. Reverse Lookup Zones | The DNS resolver checks if it is authoritative for the reverse lookup zone that corresponds to the IP range as determined by the received IP address. Aka 192.0.2.1, the reverse zone would be 1.2.0.192.in-addr.arpa |
| 3. PTR Record Query     | The DNS resolver then looks for a PTR (pointer) record on the reverse lookup zone that corresponds to the provided IP address.                                                                                        |
| 4. Response             | If a matching PTR is found, the DNS server (resolver) then returns the FQDN of the IP for the client.                                                                                                                 |


-----------------------------------------


**DNS Record Types:** 


| Record Type              | Description                                                                                             |
|--------------------------|---------------------------------------------------------------------------------------------------------|
| A (Address)              | This record maps a domain name to an IPv4 address                                                       |
| AAAA (Ipv6 Address)      | This record maps a domain name to an IPv6 address                                                       |
| CNAME (Canonical Name)   | This record creates an alias for the domain name. Aka hello.com = world.com                             |
| MX (Mail Exchange)       | This record specifies the mail server responsible for receiving email messages on behalf of the domain. |
| NS (Name Server)         | This specifies an authoritative name servers for a domain.                                              |
| PTR (Pointer)            | This is used in reverse queries to map an IP to a domain name                                           |
| TXT (Text)               | This is used to specify text associated with the domain                                                 |
| SOA (Start of Authority) | This contains administrative information about the zone                                                 |

-----------------------------------------


**Detecting Attacks:** 

	DNS Enumeration: 
	
		Filter by DNS, check records, zone transfers, look for conclusions that end with ANY. 

	DNS Tunneling: 