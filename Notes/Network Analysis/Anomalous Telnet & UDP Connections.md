
Telnet is a much less secure alternative to SSH, many older Windows NT OS and the like still use Telnet (Port 23) to provide remote C2 to Microsoft Terminal Services. 


-----------------------------------------


**Detecting Attacks:** 

	Traditional Telnet Traffic: 
	
	Look at traffic on port 23, analyze data field. 

	
	Unrecognized TCP Telnet Traffic: 

		As attackers can change ports at will, make sure to look out for encoded text within the packets, if found - follow the stream. 


	Telnet through IPv6: 

		Use this Wireshark filter to 



