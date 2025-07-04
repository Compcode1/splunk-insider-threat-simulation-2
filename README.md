# Splunk Insider Threat Simulation 2

This project simulates insider threat activity on a Windows host to generate realistic telemetry for detection and triage using Splunk. The simulation is structured around attacker behavior typically seen in internal compromise scenarios and supports structured investigation through SIEM workflows.

## 📁 Project Structure

- **Notebook**: `insider-threat-simulation-2.ipynb`
- **Format**: Jupyter Notebook with bullet-pointed detection summaries and Splunk queries
- **Tools Used**: Splunk (local install), Windows Event Viewer, PowerShell, Wireshark, Nmap

## 🎯 Objective

To simulate a staged attack by a local user on a Windows machine and use Splunk to detect each phase using real telemetry. The project helps analysts develop investigative fluency by:
- Generating controlled attacker behavior
- Collecting relevant Windows Security events
- Performing detection and triage in Splunk
- Documenting detection logic and limitations

## 🧪 Simulated Attacker Actions

1. **Multiple failed logon attempts**
2. **Obfuscated PowerShell execution of Notepad**
3. **Scheduled task persistence via `Updater2`**
4. **RDP and SMB lateral movement attempt**
5. **Nmap scan of local host**
6. **Privilege enumeration via `whoami /priv` and `icacls`**

## 🔍 Detection Approach

- Each action is correlated with specific Event IDs (e.g., 4625, 4688, 4698)
- Network behavior analyzed using Wireshark for lateral movement and scanning patterns
- Splunk used to query and validate each detection step
- Detection failures are documented for transparency and configuration learning

## ⚠️ Known Limitations

- **Process creation (4688)** was not ingested due to Splunk permissions/configuration issues
- Project highlights the importance of correct Windows audit settings and Splunk indexing

## ✅ Outcomes

- Clear identification of logon failures and lateral movement attempts
- Scheduled task creation and privilege probing successfully detected
- Event parsing, detection queries, and log gaps explicitly documented

## 🧠 Skills Demonstrated

- Host-based detection methodology
- Splunk SIEM query construction
- Network forensic analysis
- Adversary simulation and structured investigation
- Windows auditing configuration troubleshooting

---

### 📌 Author: [Compcode1](https://github.com/Compcode1)

This simulation is part of a broader portfolio of SOC analyst training and incident response documentation. Questions, issues, or collaboration ideas are welcome via GitHub.
