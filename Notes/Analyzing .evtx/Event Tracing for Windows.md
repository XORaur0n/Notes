
A built in mechanism for tracing events for user & kernel events. It can capture sys calls, process creation & termination, network activity, file & reg modifications, etc. 

**Architecture:** 

		Controllers: Facilitates management of ETW functions (initating & terminating traces, enabling & disabling providers, etc.)

		Providers: Generate events & write them to ETW sessions, there are 4 types.
			
			MOF providers: Based on the Managed Object Format (MOF).
			
			WPP Providers: Stands for Windows Software Trace Preprocessor, uses macros & annotations within source code to generate events.
			
			Manifest-Based Providers: Use .xml manifest files to determine what events will be logged, allowing for on the fly customization. 
			
			TraceLogging Providers: Use the TraceLogging API to simplify event generation. 

			

		Consumers: Subscribe to events of interest and receive them for further analysis, by default are exported a .etl file. They can also utilize the Windows API to process events. 

		Channels: Act as logical containers to filter events based on characteristics & importance. These are what consumers subscribe to for events of interest. 

		.ETL Files: Allow for writing events to disk for storage, offline analysis, archiving, and forensics. 


**Logman:** Pre-installed controller for creating, initating, halting, and investigating traces. Using the -ets flag allows for direct interaction with trace sessions. See below 
	
		logman.exe query -ets 
			
		logman.exe query "EventLog-System" -ets
			
			**From System logs**
				Name/Provider GUID
				Level
				Keywords Any

			logman.exe query providers 
				will provide a list of all providers on the system plus their GUIDs
				
		Logman can also be piped into findstr to identify strings we are looking for. *The following will find the string 'Winlogon' within the providers.*
				logman.exe query providers | findstr "[desired service]"

		Logman can be used with specific providers in mind, the following will output keywords we can filter, available event levels, and which processes are currently utilizing the provider: 
			logman.exe query providers Microsoft-Windows-Winlogon


**SilkETW:** A C# Wrapper for ETW used to abstract away some of the complexities of ETW. The following will collect information about providers and will output a .json file.
		
	SilkETW.exe -t user -pn [desired provider] -ot -file -p [path to file]\etw.json

	Get-WinEvent: Powershell cmdlet for querying Windows Event Logs. 
	
		*The following will list all logs and their properties.* 
		
			Get-WinEvent -ListLog * | Select-Object LogName, RecordCount, IsClassicLog, IsEnabled, LogMode, LogType | Format-Table -AutoSize

		*The following will list ID, level display name, message, and creation time.* 

			Get-WinEvent -LogName '[log type]' -MaxEvents [how many events you want] | Select-Object TimeCreated, ID, ProviderName, LevelDisplayName, Message | Format-Table -AutoSize

		*The following will pull events from WinRM/Operational log, can also used -Oldest flag after -LogName*

			Get-WinEvent -LogName 'Microsoft-Windows-WinRM/Operational' -MaxEvents 30 | Select-Object TimeCreated, ID, ProviderName, LevelDisplayName, Message | Format-Table -AutoSize

		*The following will pull events from an existing .evtx file*

			Get-WinEvent -Path '[file path]' -MaxEvents [how many events you want] | Select-Object TimeCreated, ID, ProviderName, LevelDisplayName, Message | Format-Table -AutoSize

	For more Powershell examples, see https://adamtheautomator.com/get-winevent/

		
		
You can also filter logs with FilterHashtable & XML, an example below. 

```
			*<Get-WinEvent -FilterHashtable @{LogName='Microsoft-Windows-Sysmon/Operational'; ID=3} |*
			*`ForEach-Object {*
			*$xml = [xml]$_.ToXml()*
			*$eventData = $xml.Event.EventData.Data*
				*New-Object PSObject -Property @{*
			    *SourceIP = $eventData | Where-Object {$_.Name -eq "SourceIp"} | Select-Object -ExpandProperty '#text'*
			    *DestinationIP = $eventData | Where-Object {$_.Name -eq "DestinationIp"} | Select-Object -ExpandProperty '#text'*
		    *ProcessGuid = $eventData | Where-Object {$_.Name -eq "ProcessGuid"} | Select-Object -ExpandProperty '#text'*
		    *ProcessId = $eventData | Where-Object {$_.Name -eq "ProcessId"} | Select-Object -ExpandProperty '#text'*
	*}*
		*}  | Where-Object {$_.DestinationIP -eq "52.113.194.132"}>*
```



	
