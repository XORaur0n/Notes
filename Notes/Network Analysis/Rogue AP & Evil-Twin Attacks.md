
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

	Most evil twin attacks 