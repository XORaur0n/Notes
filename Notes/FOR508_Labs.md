# Labs

# Malware Persistence (Autoruns): WB1 10-25

## Pre-Analysis/Filtering:

- Filter autoruns by the signer column & only want to see untrusted entries.
- Also filter to show any publishers that aren’t major software vendors.
- Filter to show enabled.

## Analysis:

- Review the Category column to see types of auto-start locations.
- Review the Services category (Entry location, description, image path, and launch string). Pay attention to where the exe is located, is it in a normal location?
- Review the Drivers category (see if they match baseline & if they are signed by trusted entities), if not run the hash.
- Review the Tasks category to see scheduled tasks that are not from trusted signers, if there are some - what kind of file are they? Are they located in suspicious directories? Does it exist in the baseline? Is there anything interesting in the Launch String?
- Also look at KnownDLLs & Logon categories just in case.

---

# Creating Triage Images with KAPE

For brevity, this lab is WB1 26-42

---

# Scaling IR & Threat Hunting: WB1 43-86

### Navigate to the directory for Velociraptor:

Update the server config with a new key:

```bash
velociraptor.exe --config server.config.yaml config rotate_key > newkey_server.config.yaml
```

To start the frontend server in verbose mode with the new config file: 

```bash
velociraptor.exe --config newkey_server.config.yaml frontend -v
```

### Navigate to the Velociraptor webpage & login:

Actual hunting/analysis is 49-81 with the following hunts: 

- Windows.Detection.Forwarded (Forwarded DLLs 49-62)
- Windows.Persistence.PermanentWMIEvents (WMI Event Consumers 62-67)
- Windows.Attack.UnexpectedImagePath (Masquerading as legit processes 68-70)
- Windows.System.Pslist (Running processes filtereing out signed processes 70-80)

### Mostly VQL querying/modifying syntax + hunting with Kansa (81-86)

---

# Evidence of Execution (Prefetch, ShimCache, Amcache):

### Prefetch:

- To parse the prefetch file & see timestamps & run count:

```bash
pecmd -f <file.pf>
```

Remember to subtract 10 seconds from the created time for approximate first execution. There is no need to do this for last run (last execution). 

Also make sure to look at the files stored within the prefetch file to see what exectuables it interacted with. Are any files/directories suspicious? 

You can also filter output for keywords: 

```bash
pecmd -f <file.pf> | findstr /I "<keyword>"
```

- To parse the entire Prefetch folder & print to CSV: (prefetch.csv & prefetch_Timeline.csv)

```bash
pecmd -d <Prefetch path> -q --csv <write directory> --csvf <outfile.csv>

```

Pivot off what you found before and see if you can find anything else in those suspicious directories (Files Loaded).

Again filter output for keywords and see if you can’t find anything weird. Evidence of staging, lateral movement, file access, etc. 

Filter the timeline by runtime & filter, anchor to a specfic result & clear filter & review items near the anchor. Anything strange? 

### ShimCache:

- To parse the Shimcache to CSV (appcompatcache.csv):

```bash
appcompatcacheparser -f <ShimCache path> --csv <write directory> --csvf <outfile.csv>

```

Last modified time is not the last time of execution, just the last mod of the exe. Can track renames, if exes are moved, modified, or renamed they are shimmed again & a new entry will be created for the same file (timestamps will be the same with different file names). 

Pivot from what was found in Prefetch, maybe some new/suspicious directories or files? Pivot off of what you find. 

See if you can find network activity, maybe lateral movement? 

### Amcache.hve

- To parse the database to CSV (amcache_UnassociatedFileEntries.csv & amcache_DriveBinaries.csv)

```bash
amcacheparser -i -f <Amcache.hve path> --csv <write directory> --csvf <outfile.csv>
```

Pivot off what was found in the other two databases, run the hashes for anything that was found. 

Look at unsigned drivers, anything we’ve seen before? 

### AppCompatProcessor.py

Make sure you sudo & navigate to the correct directory: 

```bash
AppCompatProcessor.py ./database.db load <zip archive>

```

To run a regex across the evidence: 

```bash
AppCompatProcessor.py ./database.db search
```

Review the results (output.txt), specifically the labels on each line.

To narrow the results, use one regex type (known bad): 

```bash
grep Known output.txt
```

To search for UNC paths to remote paths (\\):

```bash
grep VFS Output.txt | grep -v \?
```

Anything interesting? 

To search for exes in strange locations: 

```bash
grep Missplaced Output.txt
```

Any executables not where they are supposed to be? 

To perform stack/LFO analysis:

```bash
AppCompatProcessor.py ./database.db stack "FilePath" "FileName LIKE '%<executable>'"

```

Anything not where it is supposed to be vs the correct location, LFO is usually right. 

To look for correlations between a known filename with tcorr: 

```bash
AppCompatProcessor.py ./database.db tcorr "<exectuable>"

```

To search for short executable names with stack: 

```bash
AppCompatProcessor.py ./database.db stack "FileName" "length(filename)<8"

```

Known for false positives, but you could find something interesting. 

To identify the affected machine & full path for any hits with fsearch: 

```bash
AppCompatProcessor.py ./database.db fsearch FileName -f "<executable"

```

---

# Tracking Cred Use (Event Log Explorer) WB1 123-153

Make sure date (yyyy-mm-dd) & time (hh:nn:ss) are correct, along with using UTC time. 

File → Open Log File → New API

Make sure to load custom columns (View → Custom Columns → Load all columns

## Security Log

### Successful Logons:

- Get a good handle on how big the set is by sorting by date, what is the range?
- Start easy: 4624s & a user - what type are the logons? Are they normal for the user? What IPs are there? Are there workstation to workstation connections? Anything out of normal range? Which accounts have connected from the IPs you see?
- Does anything stick out from timestamps? Maybe scripted actions?

### RDP Profiling:

- Filter for 4778, look at client names & IPs. Are these normal & in range?
- Review the RDP client logs, if you find anything weird cross reference the SID with the security log so you can see the user.

### Local Account Auditing:

- Filter for 4776, any successful auths?
- What other accounts authenticated there? Do timestamps lineup with other suspicious activity observed?

### Auditing Admin Account Activity:

- Filter for 4672 & a user (is possible), how many logons were associated? Sort by unique domain identifier, who all has logged in to their machine?

---

# Tracking Lateral Movement (EvtxECmd) WB1 154-180

To parse an Event Log: 

```bash
evtxecmd -f <log path> --csv <write directory> --csvf <outfile.csv>

```

### Tracking Mounted Shares:

Filter by 5140, Group by Payload Data1 (Share Name) → User Name → Remote Host

- Are there any maps to the C$ admin share (common LM technique)? Which computers and when?
- Any maps to the IPC$ admin share? Which accounts? Any we’ve seen before?

### ‘RunAs’ Activity:

Filter by 4648, Group by User Name

- Look for users we’ve seen before? What user accounts did they provide explicit creds for?
- Look at TargetServerName info (Payload Data2), any new systems to add to the compromised list?
- Are they inbound (TargetServerName is localhost) or outbound (TargetServerName is a remote system)?
- Drag Payload Data2 closer to time created & note the time ranges of the connections.
- Look at the Executable Info column, anything interested to suggest attacker tradecraft?
- Look at Payload Data4, look at SPNs, see cifs, RPCSS, TERMSRV?

### Finding/Analyzing Suspicious Scheduled Tasks:

Open the task scheduler CSV, filter by 106, group by User Name → Payload Data1

- Take a look at the tasks created, anything new or suspicious looking?

Filter by 200

- Filter Payload Data1 for anything you found interesting in the previous step. Note the names in the Executable Info column.
- Anything you want to look at further? Note the range of the execution of these tasks.

Open the XML folder for any tasks that looked suspicious at \Windows\System32\Tasks in a text editor

- What user account created it? When was it scheduled to start? What was the trigger? What permission level does it have? What is the full path to the file it executes?
- A scheduled task will not have a corresponding XML if it has been deleted.

### Identify Suspicious Services

Filter by 4697, group by User Name

- Note services tied to user accounts, including ServiceName & the associated exe.

Clear grouping, group by Payload Data4 (Service Type) →Payload Data2 (ServiceFileName)

- Are there any exes that have an automatic start type that aren’t part of a standard installation?
- Look at manual start types too, anything suspicious?

---

# WMI, PowerShell, & Defender Log Analysis WB1 181-210

### Malicious PowerShell

Open the PowerShell Operational logs, make sure View → Log Loading Options → Load all events

- Filter for encoded, filter out false positives for later. Probably warning events
- If you find an encoded command, what user ran it? What time/date did it occur? Does it lineup with other activity?
- If it is Base64 encoded, decode it - what was the command?

Filter for warning events, 4103, 4104

- What days do the warning events take place?

Filter for invoke & after our previously noted timestamps

- What user(s) are tied to these? What’s the time range?
- Do any commands look like they could be from the attacker? Any activity targeting remote systems?
- Any valuable information gleaned?

### Analyzing PowerShell Transcripts

Navigate to the transcripts path, open in text editor

- How many are there? What are the date ranges?
- Any interesting data returned to the attacker? What commands were executed, what were the results?

### Analyzing Defender Logs

Open the Defender Operational log ,filter by 1116-1119

- Do we see any malware suites showing up a lot? Note the file names & paths as potential IoCs.
- Any evidence of code injection? (Legit executables with normal paths)
- Were any persistence mechanisms removed? When?

Move to ProgramData\Microsoft\Windows Defender\Support

- Look at the MPDetection log
    - Should be the same as the event log.
- Check the MP log
    - Look for anything we’ve found before. What was the earliest time we know it was running?
    - Any files related to them that we can look at?
    - Any evidence of renames?

### WMI Attacks

Open WMI Activity Logs, filter by 5861

- Any consumers stand out? Does the time range line up with other activity?
- Is it a known exploited event consumer?
- What is the event filter trigger? How long is it after boot?

---

# Identify Rogue Processes (Volatility) WB2 1-15

### Visualize Process Parent-Child Relationships

To visualize the process treee with windows.pstree:

```bash
vol.py -f <memory image> windows.pstree > pstree.txt

```

To focus on the relatrionship between processes & cut some of the additional info out:

```bash
vol.py -f <memory.img> -r pretty windows.pstree | cut -d '|' -f 1-11 > pstree-cut.txt

```

- Review parent-child relationships, does anything look out of the ordinary? Does anything sawn procs it shouldn’t?
- Note names of processes that seem out of the ordinary & their PID/PPIDs

To focus on just one PID: 

```bash
vol.py -f <memory.img> -r pretty windows.pstree --pid <PID> | cut -d '|' -f 1-11

```

### Develop Pivot Points

To scan memory for EPROCESS blocks with windows.psscan: 

```bash
vol.py -f <memory.img> -r pretty windows.psscan > psscan.txt

```

- Grep through the output for anything you found suspicious in the previous step:

```bash
grep -i <search term> <psscan.txt>

```

- What are the parent processes for it? Document PID & name.
- Compare creation times, are processes with different parents related? Does the timing make sense?
- What types of activity could explain this? One command each, return data, exit?
- Can we correlate anything we found in Defender logs before?

### Filter Process Output with Baseline Image

To compare a memory image with a baseline in JSON: 

```bash
python3 /opt/memory-baseliner/baseline.py -proc -i <memory.img>  --loadbaseline --jsonbaseline 
<baseline path> -o proc_baseline.csv

```

Convert pipe to comma-separated: 

```bash
sed -i 's/|/,/g' proc_baseline.csv

```

Filter by.exe in the DLL NAME column

- If possible, move it to a Windows instance with Timeline Explorer installed.
- Do we see anything we saw before? Any interesting command line?
- Any orphans?

Clear filters

- Review loaded DLLS, any from a user profile?

---

# Memory Process Objects (MemProc & Volatility) WB2 16-34

### Investigating Process Objects (Volatility)

To identify the full path & command-line of a suspicious process: 

```bash
vol.py -f <mem.img> windows.cmdline --pid <PID>

```

- Refer back to psscan output for creation time, document anything found.

To identify all process paths & command-line: 

```bash
vol.py -f <mem.img> windows.cmdline > cmdline.txt

```

- Grep for anything that was earlier suspicious, are they running from normal locations?

To identify the SID & account name used to start a process: 

```bash
vol.py -f <mem.img> windows.getsids --pid <PID>

```

- Are the permissions normal for the process? Does it have admin privs?

To identify the SID & account name used to start all processes:

```bash
vol.py -f <mem.img> windows.getsids > getsids.txt

```

- Grep for any previously known compromised users, do we see anything we saw before? Anything new? Is it normal?

To examine a process’ handles, focusing on file & reg key handles: 

```bash
vol.py -f <mem.img> windows.handles --pid <PID>| egrep 'File|Key'

```

- Any interesting handles? Named pipes maybe? Note it down.

### MemProcFS

Navigate to the memory image location

```bash
MemProcFS.exe -forensic 1 -device <mem.img>

```

A new M: drive should show up: 

- To see suspicious procs identified, navigate to M:\sys\proc\proc.txt

Process Flags: 

| 32 | 32 bit on 64 bit Windows |
| --- | --- |
| E | Process is NOT found in EPROCESS list (corruption, drift, unlink) |
| T | Process is terminated |
| U | Process is a user-account (non-system) |
| * | Process is outside of standard paths |
- Which processes are 32 bit?
- Can we glean anything from this for suspicious procs we’ve seen before? Are they terminated? Outside of standard paths? Running under SYSTEM? Who is their parent?

Most output files are also available in CSV, so we can look at them with Timeline Explorer (M:\forensic\csv\process.csv) 

- Filter the name column for suspicious processes we know, any interesting command lines?

(M:\forensic\csv\handles.csv)

- Any interesting handles/named pipes? Any procs related?
- Global filter for MUP (UNC), which proc has the most interesting MUPs?

### Analyze Network Artifacts (MemProc)

M:\forensic\csv\net.csv

Filter State for CLOSED, ESTABLISHED, SYN_SENT, global filter for 8080 (Alt HTTP):

- Do any procs seem abnormal to be communicating over the network?

Global filter for -8080 to show everything but 8080:

- Any evidence of a malware beacon, over 80 or 443?

Global filter for 3389 (RDP):

- Note connections & their directionality. Are any suspicious? Workstation to workstation connections?

Filter for 445 (SMB): 

- What IPs connected? Directionality?

---

# Code Injection (Mem Proc & Volatility) WB2 35-67

### Code Injection Detection with Malfind (Volatility)

Make a directory for the output & then run:

```bash
vol.py -f <mem.img> -o ./<output dir> windows.malfind --dump > malfind.txt

```

- Grep for unusual memory pages

```bash
grep PAGE <malfind output>

```

- Which processes were identified? Which has the most pages? Note the names & PIDs

Open the output text file

```bash
less <malfind output>

```

- Ignore MsMpEng hits, what processes seem to be injected (function prologues/ASCII, EXECUTE_READWRITE privs, no file mapped to disk)?

### Field Triage of Suspicious Memory Sections (Volatility)

To run strings against a specific PID from the dump

```bash
strings -8 <malfind dir>/pid.<PID>*

```

- Do the strings seem to reference things that would be in a Windows binary (DLLs & WinAPI functions)?

### FindEvil Output from Baseline Image (MemProc)

- Look for dangerous RWX entries, note the processes.
- What are the common alert types?

### FindEvil Output

M:\forensic\csv\findevil.csv

- Look at AV_DETECT events, any processes being repeatedly caught by Defender? Anything we’ve seen before with another name denoted?
- Look at THREAD types, any running outside of image mapped memory (NO_IMAGE) or is anything running with SYSTEM that shouldn’t (SYSTEM_IMPERSONATION)
- Filter for the PID of anything suspicious
    - PRIVATE_RWX, note the memory addresses.
    - PRIVATE_RX (still unusual, but less so)
    - PE_PATCHED (prone to false positives)

Run the strings of suspicious memory sections

```bash
bstrings -f M:\name\<process-PID>\vmemd\<memsection.vvmem -m 8

```

- Anything that would lead us to believe there is code here (error messaging, DLLs referenced, function prologues)?

### MemProc & YARA

Run MemProc with YARA signature scanning 

```bash
MemProcFS.exe -forensic 1 -device <mem.img> -license-accept-elastic-license-2-0

```

M:\forensic\csv\findevil.csv

- What processes were tagged due to YARA signature hits?
- To learn more about the YARA signatures that hit, M:\forensic\csv\yara.csv, what are the descriptions? Are they C2?

### FindEvil for Amadey

For brevity 54-67

---

# Memory Extraction & Rootkits (MemProc & Volatility) WB2 68-93

### Extraction & Analysis (MemProc)

To extract files from a process, M:\name\<process.exe-PID>, this folder contains all objects tied to the process. 

- Copy the representative file to your host, M:\name\<process.exe-PID>\modules\<process>
- Copy the minidump to your host, M:\name\<process.exe-PID>\minidump\mindump.dmp
- Copy any suspicious drivers you’ve seen before to your host, M:\name\<driver name>\files\modules\<driver.sys>

To search for the presence of Cobalt Strike beacons

```bash
<1768.py path> -x <minidump.dmp path>
```

- Does it appear a beacon is present? What is the payload type?
- What processes are used for spawning x86 & x64 sacrificial processes? Have we seen them before?
- What is the default named pipe for the beacon? Have we seen it before?

### Memory-Resident File Investigations (MemProc)

Navigate to M:\registry\HKU\<user>\Software\Microsoft\Windows\CurrentVersion\Run

- What run keys are present, where do they open (check the .txt files)?

Navigate to M:\registry\HKU\<user>\Software\Microsoft\Windows\CurrentVersion\Explorer\RecentDocs\.zip

- What has been opened? Have we seen it before?

Navigate to the rebuilt MFT, M:\forensic\ntfs

- Navigate to any of the suspicious directories we’ve seen before.
- Do we see anything new, what’s in user folders? Downloads?

Navigate to the file cache, M:\forensic\files

- Check out files.txt, anyhing interesting - if anything is found you can attempt to recover them at M:\forensic\files\ROOT\
- You can also parse prefetch from MemProc

```bash
pecmd -f M:\forensic\files\ROOT\Windows\prefetch\<prefetch.pf>

```

- Review PowerShell transcripts if there are any.
- Pivot back to services.csv or taks.csv if need be.
- View M:\forensic\csv\timeline_all.csv, aggregated timeline
- Search for anything we found before that was suspicious, what data sources are related? When was it first seen, does the timestamp seem legit?

You can also query the CSVs with PowerShell by typing into the address bar

```bash
Select-String "<search term>" *

```

### Rootkit Analysis (Volatility)

Make a directory for output, run malfind

```bash
vol2.py -f <mem.vmem> malfind --profile=WinXPSP2x86 --dump-dir=./<output dir> > <outfile.txt>

```

- What processes show the greatest chance for code injection? Note PID, name, address.

Change directories to the output directory

Run file on the sections, look for a PE32

```bash
file * 
```

Run strings to look for anything suspicious

```bash
strings -a <.dmp file> | less

```

Run SSDT & filter out legit entries. 

```bash
vol2.py -f <mem.vmem> --profile=WinXPSP2x86 ssdt | egrep -v '(ntoskrnl|win32k)'

```

- How many functions were hooked & by what?

To better identify the hooking driver, run modules/modscan

```bash
vol2.py -f <mem.vmem> --profile=WinXPSP2x86 modules

```

- Note the base address

Dump the driver 

```bash
vol2.py -f mem.vmem --profile=WinXPSP2x86 moddump -b <base address> --dump-dir=./<output dir>

```

---

# Malware Discovery (Sigcheck, YARA, Capa) WB2 94-113

---

# Filesystem Timeline Creation & Analysis WB2 114-131

### Creating the Timeline (115-117)

### Timeline Analysis

Pivot off of what we found earlier, look for suspected bad directories. 

- When were they created (B)?
- What files are present?
- Fix your view & look around, any idea what user created it?
- Any metadata changes (C)?
- Any timestamps missing for stuff we’ve seen before? Prefetch files?
- Document MFT record numbers (Meta)
- Navigate to file locations seen in the timeline to get more info
- Filter File Name for .lnk files,  focus on compromised users (<user> recent), look at opening times

Focus on executables. Filter File Name for .exe, filter macb for (B). Remove prefetch files, office files, and program files (-.pf -office -”Program Files”

- Anything we’ve seen before
- Pivot on last execution times

---

# Super Timeline Creation (Windows) WB2 132-144

---

# Super Timeline Creation (Linux) WB2 145-154

---

# Super Timeline Analysis WB2 155-181

Column Chooser → Drag Short Description to the left of Long Description

### Pivoting Around IoCs

Filter for bad directories

- What do you see that wasn’t in the filesystem timeline (ShimCache, etc.)
- What user opened the directory?
- What folders used to be present?

Filter for a bad directory in Short Description, global filter for .exe

- What other exes were once present?

Pivot on the last modifed time (M), filter Source Description for Bodyfile & Short Description by the directory

- Any interesting reg key mods near?

### Gaining Context Around Execution

Pivot off a bad exe we know 

- How many times was it executed?
- What other apps executed immediately before/after
- What user was responsible?
- Was a remote utility used to execute? PowerShell, etc.
- Can other executables we tied to it?

### Browser Forensics

Filter Source Description for Chrome History

- Were any machines accessed via browser?
- File access artifacts (file///C:)?
- Any other info, crypto addresses maybe?

### Registry Artifacts

Global filter for regedit

- How many times was it executed? What user ran it?
- What was it used to access?

---

# Scaling Timeline Analysis (ELK) WB2 182-191

---

# Mount & Examine VSS Images WB2 192-210

Make sure to sudo & change directories to the mounted drive (ISO)

```bash
cd /media/sansforensics
```

Change directories to the volume name, and then to disk images (ex. SRL-Data\Disk-Images)

Mount a virtual representation of the uncompressed .E01

```bash
ewfmount <image.E01> /mnt/ewf_mount/
cd /mnt/ewf_mount

```

To access the filesystem

```bash
alias mountwin
```

Now make a directory & mount within it, ex. on 196

### Mount Available Volume Shadows

To see how many snapshots exist

```bash
vshadowinfo /mnt/ewf_mount/ewf1

```

Now mount to expose the stores as their own image

```bash
vshadowmount /mnt/ewf_mount/ewf1 /mnt/vss
```

Change directories to the new one we just made & mount the vssX files as logical file systems with a for loop

```bash
for i in vss*; do mountwin $i /mnt/shadow_mount/$i; done

```

### VSS Analysis

Open the vss timeline, look for known bad directories

- Do we see anything new, batch files?
- What vss snapshot holds them?

```bash
find . | grep -i data\.bat
```

When was that shadow copy created? 

```bash
vshadowinfo /mnt/ewf_mount/ewf1

```

### VSS Filesystem Timeline Creation (209-210)

---

# NTFS Filesystem Forensics WB2 211-260

### USN Journal Analysis

Navigate to the drive letter of the triage image and then, C\$Extend

Open a cmd prompt from this location: 

```bash
MFTECmd.exe -f $J -m ..\$MFT --csv <outfile directory> --csvf <outfile.csv>
```

Make the time format to yyyy-MM-dd HH:mm:ss.fffffff (7 fs)

- Get a hold on how many events there are & what time period they cover.
- Filter Parent Path for ./<known bad dir>
    - Any new files/dirs we haven’t seen?
    - Any evidence of renames/deletes? Note the MFT entry numbers.
    - Filter for MFT entry number & sequence number to drill down on where/when they were created.
- How many changes are related to directories? Do we see any new subdirectories? Are there deletions?

### Identifying Timestomping

Filter Name for a known bad process & pay attention to the Update Reasons column

- Are there two entires with the same Update Timestamp? Do you see BasicInfoChange?

### $STANDARD_INFORMATION & $FILE_NAME Anomalies

To create another filesystem timeline with MFT data to CSV

```bash
MFTECmd.exe -f E:\C\$MFT --csv <outfile dir> --csvf <outfile.csv>
```

Filter file name for a known bad process

- What are the $STANDARD_INFORMATION creation & modification times (0x10)?
- What are the $FILE_NAME creations & modification times (0x30)?
- Is the STANDARD_INFORMATION creation before the $FILE_NAME creation? If so, this is evidence of backdating.

### Compare Mod Time with ShimCache & Amcache.hve

Open super timeline, filter Long Description for known bad proc

- What is the timestamp of the entry with Source Name AMCACHE? Is it before the file on the filesystem? If so, that is suspicious.

Compare the current SHA1 hash with the Amcache entry & run the hash

```powershell
Import-Csv <unassociated entries.csv | Where-Object { $_.Name -eq
'<proc.exe>' } | Select-Object Name,SHA1

Get-FileHash -algo SHA1 <proc path>
```

- If they match, it appears that the exe didn’t change whether or not the timestamps are different.

### Compare with Exe Compile Time

Run exiftool on a known bad process

```bash
exiftool <proc path>
```

- Does this timestamp (compilation) line up with other things we’ve seen? Does it line up with other attacker activity?

### Analyzing $I30 Directories

Open Index2Csv → change output to the output dir → Browse INDX, choose I30-Windows → Change separator, comma → Dump everything → Start parsing

Filter From Indx Slack by 1 to make sure we can only see slack entries (allocated files)

Index2Csv timestamp abbreviations:

| CTime | File Creation |
| --- | --- |
| ATime | File Modified |
| MTime | MFT Entry Modified |
| RTime | Last Accessed |
- How many entries are there?
- Do we see any suspicious exes/scripts in C:\Windows or C:\Windows\System32 (filter does not contain directories). Note timestamps of suspicious files.
- Any signs of file wiping (ex. ZZZZZZZZ.ZZZZZ)? Do the timestamps lineup with known activity ?

### Analyze MFT Resident Data 234-239

### Mount C-Drive Image with AIM 239-241

### Analyzing Alternate Data Streams 241-245

### $I30 Parsing with Velociraptor 245-256

### Extract $I30 Files with FTK Imager 256-250

---

# Anti-Forensics Analysis & Data Recovery

### Signs of File Wiping

Open the UsnJrnl timeline: 

Naviagte to the entry number for the deletion. Look for signs of SDelete

- Can you determine the file’s original name? How long did it take to wipe?
- How many times did it rename the file to overwrite the OG name? (Look for other random letters)

Can you find other files wiped by SDelete? 

- Follow the same steps, filter Name with zzz
- Can also sort by entry number & filter Update Reasons with FileDelete

### Recovering Deleted Files with FTK Imager 265-276

AppData\Local\Temp

### Records Carving with Bulk Extractor 277-280

### UsnJrnl Carved Records (UsnJrnl2Csv) 280-283

### Additional Carved Records (Evtx, MFT, I30, LogFile) 284-286