# FOR508 Timestamps

### When Was a File Created, Modified, or Deleted?

| Artifact | Timestamp | What It Tells You |
| --- | --- | --- |
| `$FILE_NAME` | Created | Likely when the file was created or first written to disk |
| `$STANDARD_INFORMATION` | Modified | When file content or metadata was last changed (can be faked) |
| `$STANDARD_INFORMATION` | Accessed | When the file was last opened (often unreliable) |
| USN Journal / `$LogFile` | N/A | File was deleted — confirmed by journal or log record |
| `$MFT` | N/A | File likely deleted but entry not yet overwritten |

### **Was a Program Executed? When?**

| Artifact | Timestamp | What It Tells You |
| --- | --- | --- |
| `Amcache.hve` | First recorded | Windows detected the program (usually first run) |
| `Prefetch` | Last run time | The app was launched via GUI or scheduled task |
| `UserAssist` | Last launched | User launched via Start Menu or Explorer |
| `Shimcache` | File mod time at load | Program may have been loaded into memory (not definitive) |
| `TaskScheduler.evtx` | "Action Started" | Windows ran the scheduled task or script |

### **When Did a User Log On or Off?**

| Artifact | Timestamp | What It Tells You |
| --- | --- | --- |
| `Security.evtx (4624)` | Timestamp | Successful logon with user and logon type |
| `Security.evtx (4634/4647)` | Timestamp | End of session using the same Logon ID |
| `PowerShell Logs/TaskScheduler` | TimeCreated | PowerShell remoting or scheduled script executed |

### **Was a File Downloaded?**

| Artifact                       | Timestamp     | What It Tells You                     |
| ------------------------------ | ------------- | ------------------------------------- |
| `Zone.Identifier` (ADS)        | None          | File came from Internet; ZoneID = 3   |
| ADS / URL Zone                 | N/A           | May include original URL and referrer |
| Browser Cache (Chrome History) | Accessed time | File or site was accessed via browser |

### **Correlating Execution and File Events**

| Artifact | Timestamp | What It Tells You |
| --- | --- | --- |
| `$FILE_NAME` | Created | File appeared on the disk |
| `Amcache` | First seen | OS recognized the file — usually after first execution |
| `Prefetch` | Last run | File was run (GUI or scheduled) |
| `Shimcache` | File mod time | File loaded into memory at some point |
| `$LogFile` | Real-time actions | File was created, renamed, deleted — low-level operations |
| `TaskScheduler.evtx` | "Action Started" | Task or script execution recorded by system event log |