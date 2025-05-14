# Suspected-Data-Exfiltration-From-PIP-Employee
An employee placed on a P.I.P. has displayed behavior to suggest he may exfiltrate data and quit the company.

09:00 AM – A Sudden Shift in Focus
It’s a standard midweek morning at a technology firm specializing in proprietary R&D. The IT security team is wrapping up routine endpoint health checks when a message from HR changes the day’s trajectory. An employee, John Doe, working in a sensitive department, has just been placed on a Performance Improvement Plan (PIP). Minutes later, John had an emotional outburst in the office—slamming his desk and leaving abruptly.

Management, concerned by John’s access level and erratic behavior, raises a red flag: Could he be planning to steal proprietary information before quitting?

09:30 AM – The Hunt Begins
The endpoint response team pivots immediately. Using Microsoft Defender for Endpoint (MDE), they initiate a targeted investigation into John's corporate device:
First step: pull the device timeline and process activity for the last 72 hours. The team is on alert for signs of reconnaissance, unusual file access, or potential data staging.

10:05 AM – Irregular Patterns Detected
Reviewing logs, the team notes a spike in access to sensitive files within a directory labeled "Project_Hermes", a confidential R&D initiative.
Time of access: 10:27 PM the previous evening—well outside normal working hours.
Over a dozen .docx, .pdf, and .pptx files were opened in quick succession.

10:22 AM – External Device Activity
Device control logs confirm the connection of a USB mass storage device during the same session. Files accessed were then copied to a temporary folder, a classic staging behavior often preceding exfiltration.

10:40 AM – Potential Cloud Upload Detected
Network telemetry shows a burst of outbound traffic to Dropbox.com at 10:33 PM, consistent with file uploads. No business justification exists for John to use personal cloud storage for his role.

11:00 AM – Containment Protocol Activated
The team triggers automated device isolation via MDE. John’s workstation is now locked off from the network. Simultaneously, an alert is escalated to the Insider Risk Response Team and legal counsel. Forensic disk imaging is authorized for deeper offline analysis.

11:20 AM – Corroborating the Threat
Additional review of endpoint behavior reveals the presence of 7-Zip processes and batch script executions logged in the Temp directory—indicative of file compression scripts, possibly for bundling sensitive data before exfiltration.

12:00 PM – The Interview Awaits
With enough indicators of intent and capability, HR and security prepare to confront John. Meanwhile, the SOC finalizes a preliminary incident summary and begins long-term log retention procedures to support a potential legal investigation.
