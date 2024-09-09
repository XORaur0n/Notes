

HTTPS, unlike HTTP is a stateful protocol - meaning it incorporates encryption in the form of TLS & SSL. 

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
	
		The message includes TLS/SSL version chosen, encryption type, and nonce

	
	Certificate Exchange: The server sends a digital certificate to the client to prove it 


