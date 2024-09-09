

HTTPS, unlike HTTP is much more secure - meaning it incorporates encryption in the form of TLS & SSL. 

-----------------------------------------


**HTTPS Session:** 

	Handshake:

		The server & client undergo a handshake when establishing connection. During the handshake the types of encryption to use is determined & they exchange their certificates. 


	Encryption: 
		
		When the handshake finishes, the client & server use the agreed upon encryption algorithm to encrypt future data


	Further Data Exchange: 
		
		Once the encrypted session is established, the client & server continue to exchange data (web pages, images, etc.)


	Decryption: When the client talks to the server/vice versa, data must be decrypted with 

-----------------------------------------

**SSL & TLS Session:** 

	Client Hello: 
	
		The message contains TLS/SSL versions supported, encryption type that will be used, and nonce (number to be used once) to be used in later steps. 


	Server Hello: 
	
		The message includes TLS/SSL version chosen, encryption type, and nonce.

	
	Certificate Exchange: The server sends a digital certificate to the client to prove its identity, this includes the server's public key and sends it to the server. 


	Key Exchange: 

		The client generates the premaster secret, then encrypts is using the server's public key to determine the session keys. 


	Session Key Derivation: 
	
		The client & server use the nonces, along with the premaster secret to compute the session keys. The keys are used for encryption & decryption of data during the secure connection. 


	Finished Messages: 
	
		To verify the handshake is successfully complete, both parties have the correct session keys, trhe client & server exchange finished messages. The message contains the hash of all previous handshake messages and is encrypted using the session keys. 


	Secure Data Exchange: Given the handshake is complete, the client & server can now exchange data over the encrypted channel. 

-----------------------------------------

**SSL Renegotiation Attacks:** 

	An attacker will attempt to negotiate the session down to the weakest possible encryption standard. 

-----------------------------------------


**Detecting Attacks:** 

	To look for SSL renegotiation attacks use this Wireshark filter: 

	ssl.record.content_type == 22

	Search for multiple Hello messages from one client within a short period. 

	Look for messages that are out of order, (i.e. a Hello message after the handshake is done.) 
