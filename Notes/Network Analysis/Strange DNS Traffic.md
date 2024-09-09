
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


