🔐 Splunk Insider Threat Simulation 2
Simulated Attacker Activity | Battlefield-Aligned Detection Engineering

This project simulates insider threat behavior from a compromised Windows user account, generating real Security log telemetry to assess detection coverage using Splunk. Each attacker step reflects a common enterprise tactic — from failed logons and obfuscated PowerShell to scheduled task persistence, lateral movement, and privilege inspection.

🎯 Project Objective
To replicate post-compromise attacker behavior from a local user account, evaluate host telemetry and SIEM alert coverage, and identify logging gaps and misconfigurations impacting detection.

🧪 Simulated Attacker Behaviors
Failed Logons: Three failed attempts followed by successful login (brute-force simulation).

Obfuscated PowerShell Execution: Launched notepad.exe using variable indirection with iex.

Scheduled Task Persistence: Created Updater2 to execute mspaint.exe at user logon.

Lateral Movement Attempt: Issued mstsc and net use commands toward internal IP (RDP + SMB).

Nmap Reconnaissance: Scanned external and localhost targets for open ports and services.

Privilege Enumeration: Ran whoami /priv and icacls to inspect local privileges and access rights.

🛡️ Detection Results (Splunk)
✅ Detected: Logon failures (4625), task creation (4698), lateral movement (partial), attribution to user Steve.

⚠️ Partially Detected: PowerShell use (4688 missing command-line), no content from -EncodedCommand.

❌ Not Detected: Nmap execution, privilege probe commands, multiple 4688 process creation gaps.

🧩 Lessons Learned
4688 process telemetry was missing due to misconfigured audit policy and Splunk privilege level.

Splunk was running under LocalSystem without access to Security logs, limiting visibility.

Mid-session policy changes failed to capture key attacker actions — audit configs must be active before simulations begin.

✔️ Scheduled tasks and logon events were captured as expected.

🔍 Battlefield Framework Mapping
This simulation tests the following host layers from the Cybersecurity Battlefield model:

Layer 1: Process Execution

Layer 2: Startup & Persistence

Layer 4: Credentials & Secrets

Layer 5: Event Monitoring

Layer 6: Network Communication

Key telemetry blind spots were observed at Layer 1 and Layer 5 due to misconfigured Splunk ingestion. The project reinforces how audit policy gaps directly impact incident response effectiveness.

📁 Tools Used
Splunk (local instance), Windows Event Viewer, PowerShell, Nmap, Wireshark

Detection queries written in Splunk SPL with event-level validation and gap documentation


