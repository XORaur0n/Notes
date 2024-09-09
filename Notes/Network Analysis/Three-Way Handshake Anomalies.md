

| Flags                                  | Description                                                                                                                   |
|----------------------------------------|-------------------------------------------------------------------------------------------------------------------------------|
| URG (Urgent)                           | This flag is to denote urgency with the current data in stream.                                                               |
| ACK (Acknowledgement)                  | This flag acknowledges receipt of data.                                                                                       |
| PSH (Push)                             | This flag instructs the TCP stack to immediately deliver the received data to the application layer, and bypass buffering.    |
| RST (Reset)                            | This flag is used for termination of the TCP connection (we will dive into hijacking and RST attacks soon).                   |
| SYN (Synchronize)                      | This flag is used to establish an initial connection with TCP.                                                                |
| FIN (Finish)                           | This flag is used to denote the finish of a TCP connection. It is used when no more data needs to be sent.                    |
| ECN (Explicit Congestion Notification) | This flag is used to denote congestion within our network, it is to let the hosts know to avoid unnecessary re-transmissions. |



-----------------------------------------



**Detecting abnormal TCP handshake traffic:** 


	Excessive SYN flags: 

		Usually a result of network scanning. Look for signs of SYN scans & SYN Stealth Scans


	No flags: Look for NULL scans. If the port is open, the host will not respond. If the port is closed, the host will respond with a RST. 


	Excessive ACK flags: Look for ACK scans. If the port is open the host will not respond/or respond with a RST. If the port is closed the host will respond with a RST. 


	Excessive FINs: Look for FIN scans. If the port is open the host will not respond. If the port is closed the host will respond with a rest. 