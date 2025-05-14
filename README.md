# Suspected-Data-Exfiltration-From-PIP-Employee
An employee placed on a P.I.P. has displayed behavior to suggest he may exfiltrate data and quit the company.

## 09:00 AM – A Sudden Shift in Focus:<BR>
It’s a standard midweek morning at a technology firm specializing in proprietary R&D. The IT security team is wrapping up routine endpoint health checks when a message from HR changes the day’s trajectory. An employee, John Doe, working in a sensitive department, has just been placed on a _Performance Improvement Plan_ (PIP). Minutes later, John had an emotional outburst in the office—slamming his desk and leaving abruptly.

Management, concerned by John’s access level and erratic behavior, raises a red flag: Could he be planning to steal proprietary information before quitting?

## 09:30 AM – The Hunt Begins<BR>
The endpoint response team pivots immediately. Using Microsoft Defender for Endpoint (MDE), they initiate a targeted investigation into John's corporate device:
First step: pull the device timeline and process activity for the last 72 hours. The team is on alert for signs of reconnaissance, unusual file access, or potential data staging.

## 10:05 AM – Irregular Patterns Detected<BR>
Reviewing logs, the team notes a spike in access to sensitive files within a directory labeled "Project_Hermes", a confidential R&D initiative.
Time of access: 10:27 PM the previous evening—well outside normal working hours.
Over a dozen .docx, .pdf, and .pptx files were opened in quick succession.

## 10:22 AM – External Device Activity<BR>
Device control logs confirm the connection of a USB mass storage device during the same session. Files accessed were then copied to a temporary folder, a classic staging behavior often preceding exfiltration.

## 10:40 AM – Potential Cloud Upload Detected<BR>
Network telemetry shows a burst of outbound traffic to Dropbox.com at 10:33 PM, consistent with file uploads. No business justification exists for John to use personal cloud storage for his role.

## 11:00 AM – Containment Protocol Activated<BR>
The team triggers automated device isolation via MDE. John’s workstation is now locked off from the network. Simultaneously, an alert is escalated to the Insider Risk Response Team and legal counsel. Forensic disk imaging is authorized for deeper offline analysis.

## 11:20 AM – Corroborating the Threat<BR>
Additional review of endpoint behavior reveals the presence of 7-Zip processes and batch script executions logged in the Temp directory—indicative of file compression scripts, possibly for bundling sensitive data before exfiltration.

## 12:00 PM – The Interview Awaits<BR>
With enough indicators of intent and capability, HR and security prepare to confront John. Meanwhile, the SOC finalizes a preliminary incident summary and begins long-term log retention procedures to support a potential legal investigation.

Now that the stage is set, let's work through this scenario and see what we as the SOC Analyst discovers.

### - Preparation
> [!NOTE]
> <strong>Set up the hunt by defining what you're looking for:</strong><BR><BR>
The server team has noticed a significant network performance degradation on some of their older devices attached to the network in the 10.0.0.0/16 network. After ruling out external DDoS attacks, the security team suspects something might be going on internally.<BR><BR>
Activity: All traffic originating from within the local network is by default allowed by all hosts. There is also unrestricted use of PowerShell and other applications in the environment. It’s possible someone is either downloading large files or doing some kind of port scanning against hosts in the local network.
### - Data Collection
> [!TIP]
> <strong>Gather relevant data from logs, network traffic, and endpoints.</strong><BR><BR>
Consider inspecting the logs for excessive successful/failed connections from any devices.  If discovered, pivot and inspect those devices for any suspicious file or process events.<BR><BR>
Activity: Ensure data is available from all key sources for analysis.
Ensure the relevant tables contain recent logs:<br><br>
DeviceNetworkEvents<BR>
DeviceFileEvents<BR>
DeviceProcessEvents

### - Data Analysis
> [!TIP]
> <strong>Analyze data to test your hypothesis.</strong><BR><BR>
Activity: Look for anomalies, patterns, or indicators of compromise (IOCs) using various tools and techniques.
Anything going on that you can notice in terms of excessive network connections to/from any hosts? Take note of the the query/logs/time
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
