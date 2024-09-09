
**Rogue AP:** 

	A rogue AP serves to circumvent perimeter controls. If installed, it can defeat network controls and segementation. 

	Most of the time, these take the form of hotspots & tethered conenctions. It is directly connected to the network. 


**Evil Twin:** 

	AP initialized by an attacker for various purposes. They are standalone and may point to other network devices (web servers) to act as a MITM. 

	Mostly used to harvest wireless & domain credentials, commonly encompass hostile portal attacks. 


**Detecting Attacks:** 

	To determine an anomaly vs an error, use:
		
		(wlan.fc.type == 00) and (wlan.fc.type_subtype == 8)

	Analyzing beacons is the best way to deconflict between genuine & rogue APs. 

	Most evil twin attacks are devoid of Robust Security Network (RSN) information.

	Always make sure to check other fields for discrepancies as some attackers may use the same cipher as the network's AP to avoid detection. 

	To find a victim use: 
		
		(wlan.bssid == [Suspicious MAC Address])

	Make sure to check network devices & scrutinize WiFi networks in proximity to the victim network. If a network has a strong signal & lacks encryption - it is probably a rogue AP established to navigate around network controls. 