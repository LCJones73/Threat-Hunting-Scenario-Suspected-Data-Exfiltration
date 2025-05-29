# Suspected Data Exfiltration

## The HR department for CypherT3ch had to confront Labuser about the malware and Brute Force attack.

Unfortunately the employee was placed on a P.I.P. and since has displayed behavior to suggest he could potentially retaliate, exfiltrate data and quit the company.

## 09:00 AM – A Sudden Shift in Focus:<BR>
It’s a standard midweek morning at this technology firm specializing in proprietary R&D. The IT security team is wrapping up routine endpoint health checks when a message from HR changes the day’s trajectory. An employee, John Doe, working in a sensitive department, has just been placed on a _Performance Improvement Plan_ (PIP). Minutes later, John had an emotional outburst in the office—slamming his desk and leaving abruptly.

Management, concerned by John’s access level and erratic behavior, raises a red flag: Could he be planning to steal proprietary information before quitting?

## 09:30 AM – The Hunt Begins<BR>
The endpoint response team pivots immediately. Using Microsoft Defender for Endpoint (MDE), they initiate a targeted investigation into John's corporate device:
First step: pull the device timeline and process activity for the last 72 hours. The team is on alert for signs of reconnaissance, unusual file access, or potential data staging.

Now that the stage is set, let's work through this scenario and see what we as the SOC Analyst discovers.

### - Preparation
> [!NOTE]
> <strong>Set up the hunt by defining what you're looking for:</strong><BR><BR>
An employee named John Doe, working in a sensitive department, recently got put on a performance improvement plan (PIP). After John threw a fit, management has raised concerns that John may be planning to steal proprietary information and then quit the company. Your task is to investigate John's activities on his corporate device (vm-mde-cj) using Microsoft Defender for Endpoint (MDE) and ensure nothing suspicious is taking place.<BR><BR>
Activity: Develop a hypothesis based on threat intelligence and security gaps (e.g., “Could there be lateral movement in the network?”).
John is an administrator on his device and is not limited on which applications he uses. He may try to archive/compress sensitive information and send it to a private drive or something.

### - Data Collection
> [!TIP]
> <strong>Gather relevant data from logs, network traffic, and endpoints.</strong><BR><BR>
Consider inspecting process activity as well as the file system for anything that matches the compression or exfiltration of data.<BR><BR>
Activity: Ensure data is available from all key sources for analysis.
Ensure the relevant tables contain recent logs:<br><br>
DeviceFileEvents<BR>
DeviceProcessEvents<BR>
> DeviceNetworkEvents

### - Data Analysis
> [!TIP]
> <strong>Analyze data to test your hypothesis.</strong><BR><BR>
Activity: Look for anomalies, patterns, or indicators of compromise (IOCs) using various tools and techniques.
  - Is there any evidence of anything that resembles company files being archived?
  - If so, can you identify exactly what is happening?
  - If you find something, take note of the time stamp and search the other tables for +/-1 minutes around the same time to see if you can notice anything
  - Take note of your findings with the corresponding queries below

### - Investigation
> [!WARNING]
> <strong>Investigate any suspicious findings.</strong><BR><BR>
Activity: Dig deeper into detected threats, determine their scope, and escalate if necessary. See if anything you find matches TTPs within the MITRE ATT&CK Framework.
Search the DeviceFileEvents and DeviceProcessEvents tables around the same time based on your findings in the DeviceNetworkEvents tables to see if you can find more evidence for the cause of network slowdowns.
### - Response
> [!WARNING]
> <strong>Mitigate any confirmed threats.</strong><BR><BR>
Activity: Work with security teams to contain, remove, and recover from the threat.
Can anything be done?
### - Documentation
> [!NOTE]
> <strong>Record your findings and learn from them.</strong><BR><BR>
Activity: Document what you found and use it to improve future hunts and defenses.
Document what you did
### - Improvement
> [!IMPORTANT]
> <strong>Improve your security posture or refine your methods for the next hunt.</strong><BR><BR>
Activity: Adjust strategies and tools based on what worked or didn’t.
Anything we could have done to prevent the thing we hunted for? Any way we could have improved our hunting process?

## Notes / Findings:

### Timeline Summary and Findings:
I started with checking the DeviceFileEvents to see if John Doe was running anything that might indicate data exfiltration:

![image](https://github.com/user-attachments/assets/5143fa01-ef91-4412-b4b5-ead4ed2824f6)

I noticed that he had run a PowerShell script meant to test to see if PowerShell scripts can be run on his maching successfully

![image](https://github.com/user-attachments/assets/ba1d7b7a-af63-4f77-975e-1130f06230f3)

I also noticed in the same search that John Doe also ran had an Archive.zip file for: 2025-05-14T00:46:24.5921685Z

![image](https://github.com/user-attachments/assets/c17fd720-fbb9-45e5-ab68-2b9b80761823)

I know that when I see an "Archive.zip" (via DeviceProcessEvents, FileCreationEvents, or similar tables) it typically indicates that a ZIP archive was either:

🔹 Created, Accessed, Downloaded, or Dropped

In this case John Doe was using it in relation to Microsoft 3D Viewer (look below):

![image](https://github.com/user-attachments/assets/0c7f4684-088e-48da-881d-a4cf963571ed)

Since that is not so suspicious, I remembered there was that one .ps1 file indicating testing to ensure the PowerShell scripts work on his device. So, I ran the following KQL code to dig deeper into what John Doe was potentially doing:

![image](https://github.com/user-attachments/assets/fa7c9930-61e4-4f7e-a260-f81ea6ceeceb)

I discovered the following:

![image](https://github.com/user-attachments/assets/2c898146-04ff-4312-b442-ccd729eeeb7c)

Digging a little deeper I wanted to see if any files were staged to potentially be exfiltrated:

![image](https://github.com/user-attachments/assets/4198241a-75b8-4f62-8dea-7cad25d5dab6)

Which yielded the following files:

![image](https://github.com/user-attachments/assets/0afeb915-1e60-4445-ade2-f7ada16da3b5)

This PowerShell code is simply downloading, which we might say is "infiltration" not "Exfiltration" of data.

## Why It's Not Exfiltration:
 - Invoke-WebRequest with -OutFile downloads data from a URL to the local machine.
 - No files are being read from the system and sent elsewhere — that would involve:
   - Invoke-RestMethod or Invoke-WebRequest with a POST
   - UploadFile(), System.Net.WebClient
   - Use of cloud storage URLs, FTP, Discord, Mega.nz, etc.

A final scan using the following KQL code to see if John Doe has exfiltrated files:

![image](https://github.com/user-attachments/assets/25cb5c80-5f25-4a82-8352-e810bfd10105)

The results yielded nothing, which means for now John Doe has not acted maliciously to exfiltrate data.

## 12:00 PM – The Incident Summary Report finalized: 
John Doe is cleared of any suspicion for exfiltrating data, the report is finalized and sent to HR for review.<BR><BR>

<p align="center">
  <strong>This wraps up Threat Hunting Scenario: Suspected Data Exfiltration From employee John doe under a P.I.P.</strong>
</p>

<p align="center">
____________________________________________________________________________________________________________________________
</p><BR>
<p align="center">
  <img src="https://github.com/user-attachments/assets/357dc7ff-c6d7-4d02-9200-97c028e0a2a7" alt="Description" width="400"/>
</p>
<p align="center">
____________________________________________________________________________________________________________________________
</p>
