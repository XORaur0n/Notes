
Attacker will fabricate an 802.11 deuth frame and masquerade it as one originating from a legitimate AP

In doing so, the attacker might be able to disconnect one of the clients from the network.

Often times, the client will attempt to reconnect while the attacker is sniffing packets. 

This attack operates by the attacker spoofing or altering the MAC of the frame's sender. The client device cannot really discern the difference without additional controls like IEEE 802.11w (Management Frame Protection). Each deauthentication request is associated with a reason code explaining why the client is being disconnected.

**Detecting Deauthentication Attacks:** 

	In order to see traffic from the AP's BSSID use: 
		
		wlan.bssid == xx:xx:xx:xx:xx:xx

	In order to see the deauth frames from the BSSID/attacker pretending to send them from the AP's BSSID use: 
		
		(wlan.bssid == xx:xx:xx:xx:xx:xx) and (wlan.fc.type == 00) and (wlan.fc.type_subtype == 12)

	To see if requests are from an attacker use: 

		(wlan.bssid == F8:14:FE:4D:E6:F1) and (wlan.fc.type == 00) and (wlan.fc.type_subtype == 12) and (wlan.fixed.reason_code == 7)

	*In order to defeat an attacker using revolving reason codes just use all of the codes startign with 1* 