
**Log types (.evtx):** 

	Security
	Application
	Setup
	System
	Forwarded Events


-----------------------------------------

**Log fields:** 

	Source: The software that logged the event.
    
    Event ID: A unique identifier for the event.
    
    
    Task Category: This often contains a value or name that can help us understand the purpose or use of the event.
    
    Level: The severity of the event (Information, Warning, Error, Critical, and Verbose).
    
    Keywords: Keywords are flags that allow us to categorize events in ways beyond the other classification options. These are generally broad categories, such as "Audit Success" or "Audit Failure" in the Security log.
    
    User: The user account that was logged on when the event occurred.
    
    OpCode: This field can identify the specific operation that the event reports.
    
    Logged: The date and time when the event was logged.
    
    Computer: The name of the computer where the event occurred.
    
    XML Data: All the above information is also included in an XML format along with additional event data.


-----------------------------------------

**Log manipulation with XML:**

	MS documentation: https://techcommunity.microsoft.com/t5/ask-the-directory-services-team/advanced-xml-filtering-in-the-windows-event-viewer/ba-p/399761


-----------------------------------------


**Sysmon:** 

	A service/driver part of the SysInternals suite used for extending Windows logging capability, finding IOCs. 
	
Sysmon uses a .xml file for its configuration. 
		
		When .xml file has been configured for logging, push the config with:
			
			sysmon.exe -c sysmonconfig-export.xml
	
Once downloaded install sysmon with:
		
		sysmon.exe -i -accepteula -h md5,sha256,imphash -l -n


