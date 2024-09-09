
An attacker attempts to abuse, interrogate, and exploit web servers. 

Sometimes attackers will stagger the response or send them from multiple hosts & addresses. 

-----------------------------------------

**Detecting Attacks:** 


	Directory Fuzzing: 

		Look for a host repeatedly & in quick succession trying to access files on the web server that do not exist (404)


	Other Types of Fuzzing: 

	Use this wireshark to look at one host. 
	
		filterhttp.request and ((ip.src_host == <suspected IP>) or (ip.dst_host == <suspected IP>))

		Follow the HTTP stream of the suspect requests. 