🔐 Splunk Insider Threat Simulation 2
🎯 Project Objective
Simulate realistic insider threat behavior from a compromised Windows user account.

Generate real Security log telemetry by executing attacker-like actions.

Use Splunk to assess detection visibility, identify misconfigurations, and analyze logging gaps.

Map all detection findings to the Cybersecurity Battlefield host layers.

🧪 Simulated Attacker Behaviors
Step 1 – Failed + Successful Logons

Three failed logon attempts followed by a successful logon using the “Steve” account.

Simulates brute-force or password guessing.

Maps to: Layer 4 – Credentials & Secrets

Step 2 – Obfuscated PowerShell Execution

Used -EncodedCommand and variable indirection to launch notepad.exe via iex.

Simulates stealthy execution using living-off-the-land techniques.

Maps to: Layer 1 – Process Execution

Step 3 – Scheduled Task Persistence

Created a scheduled task named Updater2 to run mspaint.exe at user logon.

Demonstrates attacker persistence using legitimate OS mechanisms.

Maps to: Layer 2 – Startup & Persistence

Step 4 – Simulated Lateral Movement Attempt

Ran mstsc /v:<internal IP> and net use \\<internal IP>\C$ to simulate RDP and SMB.

Represents attacker attempts to pivot laterally.

Maps to: Layer 6 – Network Communication and Layer 1 – Process Execution

Step 5 – Nmap Scan of External Target

Scanned IP 192.168.56.102 for common ports and services.

Represents post-compromise reconnaissance.

Maps to: Layer 6 – Network Communication and Layer 1 – Process Execution

Step 6 – Nmap Scan of Localhost

Scanned 127.0.0.1 to identify exposed local services.

Simulates internal recon to discover privilege escalation paths.

Maps to: Layer 6 – Network Communication and Layer 1 – Process Execution

Step 7 – Simulated Privilege Escalation

Ran whoami /priv and icacls C:\Windows to inspect inherited privileges and file system access.

Mimics post-exploitation privilege probing.

Maps to: Layer 4 – Credentials & Secrets and Layer 5 – Event Monitoring

🛡️ Detection Results in Splunk
✅ Detected

All failed and successful logon events (4625, 4624)

Scheduled task creation (4698)

RDP and SMB login failures via 4625

Account attribution to “Steve” was clear in all valid events

⚠️ Partially Detected

PowerShell use (4688 process seen, but no command-line content)

Lateral movement attempts had logon traces but no process telemetry

❌ Not Detected

PowerShell -EncodedCommand content not visible

Nmap execution (no process creation or network telemetry)

Privilege inspection tools (whoami, icacls) were completely invisible

Event ID 4688 was mostly missing or non-functional

⚠️ Visibility Gaps and Issues
Event ID 4688 (Process Creation) was not reliably ingested

Splunk’s splunkd was running under LocalSystem with insufficient access to Security logs

Audit policy changes were applied mid-session, missing early attacker activity

PowerShell command content using -EncodedCommand was invisible

No network or scan-related events were present from Nmap or SMB probes

🔍 Battlefield Framework Mapping
Layer 1 – Process Execution

Incomplete visibility (4688 failure blocked view of PowerShell, Nmap, mstsc, net.exe)

Layer 2 – Startup & Persistence

Fully captured (scheduled task creation logged with full metadata)

Layer 4 – Credentials & Secrets

Logon success/failure captured

Privilege inspection commands not logged

Layer 5 – Event Monitoring

Misconfigured at start of simulation (mid-session policy activation failed)

Layer 6 – Network Communication

RDP and SMB activity partially captured (4625)

Nmap scanning and socket-based behaviors completely missed

🧠 Lessons Learned
4688 process logging must be tested prior to attack simulation

Validate ingestion with a known process like notepad.exe

Splunk should run under a privileged account

Use a dedicated service account in the Event Log Readers group

Audit policy must be active before any attacker behavior

Mid-session policy changes will not retroactively capture events

Consider integrating Sysmon

To fill gaps in process telemetry, command-line logging, and network visibility

🛠️ Tools Used
Splunk (local instance)

Windows Event Viewer

PowerShell (elevated and user mode)

Nmap (internal and external scan variations)

Wireshark (to confirm lateral movement packet behavior)

🧭 Enterprise Analogy
Models a local user account compromise after phishing or credential reuse.

Simulates real-world behaviors used by APTs and insider threats:

Brute-force login

PowerShell-based execution

Scheduled task persistence

Internal lateral movement attempts

Post-compromise recon and privilege inspection


