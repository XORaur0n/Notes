
![[Pasted image 20240909072150.png]]


**IPv6 Addressing Types:** 

		Unicast: Addresses for a single interface.
		Anycast: Addresses for multiple interfaces, where only one of them receives the packet.
		Multicast: Addresses for multiple interfaces, where all of them receive the same packet.

-----------------------------------------

HTTP Methods: 
		
	HEAD: *Required* is a safe method that requests a response from the server similar to a Get request except that the message body is not included. It is a great way to acquire more information about the server and its operational status.

	GET: *Required* Get is the most common method used. It requests information and content from the server. For example, GET http://10.1.1.1/Webserver/index.html requests the index.html page from the server based on our supplied URI.

	POST: *Optional* Post is a way to submit information to a server based on the fields in the request. For example, submitting a message to a Facebook post or website forum is a POST action. The actual action taken can vary based on the server, and we should pay attention to the response codes sent back to validate the action.

	PUT: *Optional* Put will take the data appended to the message and place it under the requested URI. If an item does not exist there already, it will create one with the supplied data. If an object already exists, the new PUT will be considered the most up-to-date, and the object will be modified to match. The easiest way to visualize the differences between PUT and POST is to think of it like this; PUT will create or update an object at the URI supplied, while POST will create child entities at the provided URI. The action taken can be compared with the difference between creating a new file vs. writing comments about that file on the same page.

	DELETE: *Optional* Delete does as the name implies. It will remove the object at the given URI.

	TRACE: *Optional* Allows for remote server diagnosis. The remote server will echo the same request that was sent in its response if the TRACE method is enabled.

	OPTIONS: *Optional* The Options method can gather information on the supported HTTP methods the server recognizes. This way, we can determine the requirements for interacting with a specific resource or server without actually requesting data or objects from it.

	CONNECT: Optional Connect is reserved for use with Proxies or other security devices like firewalls. Connect allows for tunneling over HTTP. (SSL tunnels)

-----------------------------------------

**HTTPS Handshake:** 

	Client and server exchange hello messages to agree on connection parameters.
    
    Client and server exchange necessary cryptographic parameters to establish a premaster secret.
    
    Client and server will exchange x.509 certificates and cryptographic information allowing for authentication within the session.
    
    Generate a master secret from the premaster secret and exchanged random values.
    
    Client and server issue negotiated security parameters to the record layer portion of the TLS protocol.
    
    Client and server verify that their peer has calculated the same security parameters and that the handshake occurred without tampering by an attacker.


-----------------------------------------

**FTP Commands:** 

	USER: Specifies the user to log in as.
	PASS: Sends the password for the user attempting to log in.
	PORT:	When in active mode, this will change the data port used.
	PASV:	Switches the connection to the server from active mode to passive.
	LIST	Displays a list of the files in the current directory.
	CWD	Will change the current working directory to one specified.
	PWD	Prints out the directory you are currently working in.
	SIZE	Will return the size of a file specified.
	RETR	Retrieves the file from the FTP server.
	QUIT	Ends the session.


-----------------------------------------


**TCPDump:** 

Common Flags: 

| **Switch**   | **Result**                                                                                                             |
|--------------|------------------------------------------------------------------------------------------------------------------------|
| D            | Will display any interfaces available to capture from.<br>                                                             |
| i            | Selects an interface to capture from. ex. -i eth0<br>                                                                  |
| n            | Do not resolve hostnames.<br><br>                                                                                      |
| nn           | Do not resolve hostnames or well-known ports.                                                                          |
| e            | Will grab the ethernet header along with upper-layer data.<br>                                                         |
| X            | Show Contents of packets in hex and ASCII.<br>                                                                         |
| XX           | Same as X, but will also specify ethernet headers. (like using Xe)<br>                                                 |
| v, vv, vvv   | Increase the verbosity of output shown and saved.<br>                                                                  |
| c            | Grab a specific number of packets, then quit the program.<br>                                                          |
| s            | Defines how much of a packet to grab.<br>                                                                              |
| S            | change relative sequence numbers in the capture display to absolute sequence numbers. (13248765839 instead of 101)<br> |
| q            | Print less protocol information.<br>                                                                                   |
| r file.pcap	 | Read from a file.<br>                                                                                                  |
| w file.pcap	 | Write into a file<br>  


-----------------------------------------


**Helpful Filters:** 


| host                  | host will filter visible traffic to show anything involving the designated host. Bi-directional               |
|-----------------------|---------------------------------------------------------------------------------------------------------------|
| src / dest	           | <br>src and dest are modifiers. We can use them to designate a source or destination host or port.<br>        |
| net	                  | <br>net will show us any traffic sourcing from or destined to the network designated. It uses / notation.<br> |
| proto	                | <br><br>will filter for a specific protocol type. (ether, TCP, UDP, and ICMP as examples)<br>                 |
| port                  | port is bi-directional. It will show any traffic with the specified port as the source or destination.<br>    |
| portrange             | <br>portrange allows us to specify a range of ports. (0-1024)<br>                                             |
| less / greater "< >"	 | <br>less and greater can be used to look for a packet or protocol option of a specific size.<br>              |
| and / &&	             | <br>and && can be used to concatenate two different filters together. for example, src host AND port.<br>     |
| or                    | <br>or allows for a match on either of two conditions. It does not have to meet both. It can be tricky.<br>   |
| not                   | <br>not is a modifier saying anything but x. For example, not UDP.<br>                                        |

-----------------------------------------

**TShark**:  

Flags: 

| D             | Will display any interfaces available to capture from and then exit out.                                          |
|---------------|-------------------------------------------------------------------------------------------------------------------|
| L             | Will list the Link-layer mediums you can capture from and then exit out. (ethernet as an example)                 |
| i             | choose an interface to capture from. (-i eth0)                                                                    |
| f             | packet filter in libpcap syntax. Used during capture.                                                             |
| c             | Grab a specific number of packets, then quit the program. Defines a stop condition.                               |
| a             | Defines an autostop condition. Can be after a duration, specific file size, or after a certain number of packets. |
| r (pcap-file) | Read from a file.                                                                                                 |
| W (pcap-file) | Write into a file using the pcapng format.                                                                        |
| P             | Will print the packet summary while writing into a file (-W)                                                      |
| x             | will add Hex and ASCII output into the capture.                                                                   |
| h             | See the help menu                                                                                                 |


-----------------------------------------

**Wireshark:**

Helpful Capture Filters: 

| host x.x.x.x                    | Capture only traffic pertaining to a certain host                                                                    |
|---------------------------------|----------------------------------------------------------------------------------------------------------------------|
| net x.x.x.x/24                  | Capture traffic to or from a specific network (using slash notation to specify the mask)                             |
| src/dst net x.x.x.x/24          | Using src or dst net will only capture traffic sourcing from the specified network or destined to the target network |
| port #                          | will filter out all traffic except the port you specify                                                              |
| not port #                      | will capture everything except the port specified                                                                    |
| port # and #                    | AND will concatenate your specified ports                                                                            |
| portrange x-x                   | portrange will grab traffic from all ports within the range only                                                     |
| ip / ether / tcp                | These filters will only grab traffic from specified protocol headers.                                                |
| broadcast / multicast / unicast | Grabs a specific type of traffic. one to one, one to many, or one to all.                                            |


-----------------------------------------

Helpful Display Filters: 

| ip.addr == x.x.x.x         | Capture only traffic pertaining to a certain host. This is an OR statement.                   |
|----------------------------|-----------------------------------------------------------------------------------------------|
| ip.addr == x.x.x.x/24      | Capture traffic pertaining to a specific network. This is an OR statement.                    |
| ip.src/dst == x.x.x.x      | Capture traffic to or from a specific host                                                    |
| dns / tcp / ftp / arp / ip | filter traffic by a specific protocol. There are many more options.                           |
| tcp.port == x              | filter by a specific tcp port.                                                                |
| tcp.port / udp.port != x   | will capture everything except the port specified                                             |
| and / or / not             | AND will concatenate, OR will find either of two options, NOT will exclude your input option. |





