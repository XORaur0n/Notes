
Just because it doesn't look like anything weird is going on when analyzing traffic, a deeper look is often needed. 

Specifically, looking at strange headers like: 

	 Weird Hosts (Host: ) 
	 Unusual HTTP Verbs 
	 Changed User Agents 


-----------------------------------------


Detecting Attacks: 

	Strange Host Headers: Use the following wireshark filter: 
	
	http.request and (!(http.host == "[IP Address]"))

		Look for URIs like loopback addresses (127.0.0.1)