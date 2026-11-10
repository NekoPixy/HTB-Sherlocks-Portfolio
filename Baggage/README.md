# 🔍 HTB Sherlock Write-up: Baggage

Analysis conducted and documented by: **Jonathan Di Genova** (Junior SOC Analyst & Cybersecurity Specialist)

---

## 📌 Scenario Overview
The **Baggage** Sherlock lab presents a Digital Forensics and Incident Response (DFIR) investigation centered around Windows Shellbag artifacts. The primary objective is to reconstruct an attacker's post-exploitation activities following initial compromise—specifically tracking file exploration, search utility deployment, network share traversal, data staging, and compression prior to exfiltration.

- **Platform:** Hack The Box
- **Lab Name:** Baggage
- **Category:** DFIR (Digital Forensics & Incident Response)
- **Difficulty:** Very Easy

---

## 🛠️ Tools & Applied Skills

### Forensics & Analysis Tools
- **Kali Linux CLI Suite:** `strings`, `grep`, `python3`
- **Registry & Artifact Parsers:** `ShellBags Explorer (SBE)`, `regipy` (Python Library)
- **Text & Log Processing:** Regex parsing for UTF-16LE registry strings

### Core Technical Competencies Tested
- Windows Forensics & Registry Hive Analysis (`NTUSER.DAT`, `UsrClass.dat`)
- Shellbag Parsing & Directory Traversal Reconstruction
- Identifying Attacker Staging & Obfuscation Tactics
- Timeline Reconstruction & Timestamp Analysis (UTC)[cite: 

---

## 🎯 Investigation Methodology & Analytical Journey

### 1. Initial Reconnaissance & Utility Tracking
The investigation began by inspecting extracted artifacts to trace initial execution and tool transfer. By analyzing recent file activity and user profile artifacts, I identified the original compressed file downloaded by the compromised user, alongside a third-party search utility introduced by the adversary to perform targeted internal recon across the victim's filesystem.

### 2. Timeline & File Access Reconstruction
To determine the extent of unauthorized access, I focused on Windows Shellbag artifacts (`UsrClass.dat`). Shellbags preserve user navigation preferences and folder interaction history even when underlying directories are removed. 

Through string extraction and hive parsing:
- I identified key timestamps marking when sensitive directories (such as VPN credentials and stored password stores) were opened by the attacker.
- I mapped out lateral movement attempts involving an internal network share (UNC path) accessed to pillage corporate documentation.

### 3. Overcoming Hurdles: Locating the Staging Directory
The most challenging phase of the investigation was identifying the exact staging directory used to accumulate loot before exfiltration. 
- **Initial Pitfall:** Standard system temporary paths (e.g., `C:\Users\<user>\AppData\Local\Temp`) were initially evaluated but ruled out as false leads.
- **Analytical Breakthrough:** Realizing that physical temporary directories were missing from the forensic acquisition image, I shifted focus entirely to deep parsing of `UsrClass.dat` Shellbag entries. Using structured regex scripts and targeted string extraction, I uncovered that the attacker deliberately created an obfuscated, single-letter folder inside a standard user library directory to stage data and compress it into a hidden zip file.

### 4. Exfiltration Archive Analysis
Finally, by matching timestamp artifacts associated with the compressed archive created inside the staging location, I established the exact timestamp when the exfiltration package was accessed prior to removal.

---

## 📋 Task Summary

All 10 investigation tasks were systematically solved during this lab:

1. **Task 1:** Name of the archive file downloaded by the compromised account — **Task Completed**
2. **Task 2:** Utility brought in by the attacker to search for sensitive data — **Task Completed**
3. **Task 3:** Timestamp when the sensitive VPN folder was accessed — **Task Completed**
4. **Task 4:** Directory name containing the victim's stored passwords — **Task Completed**
5. **Task 5:** Full UNC path of the network share accessed by the attacker — **Task Completed**
6. **Task 6:** Target year identified in the stolen engineering plans — **Task Completed**
7. **Task 7:** Archive file name present on the internal network share — **Task Completed**
8. **Task 8:** Timestamp when the network share archive was accessed — **Task Completed**
9. **Task 9:** Full filesystem path of the attacker's hidden staging folder — **Task Completed**
10. **Task 10:** Access timestamp of the final exfiltration archive — **Task Completed**

---

## 💡 Key Takeaways & SOC Reflections

- **Shellbags are Essential for DFIR:** Even when physical folders are wiped or temporary files cleaned up, Windows Shellbags (`UsrClass.dat`) leave a persistent footprint of folder access and navigation history.
- **Adversaries Obfuscate Staging Locations:** Attackers rarely rely on default temporary directories when attempting to hide data; leveraging user libraries like `Pictures` or `Documents` with benign-looking or single-character folder names is a common defense evasion technique.
- **Methodology Over Guesswork:** Relying strictly on systematic registry parsing rather than assumptions saved critical investigation time when identifying non-standard staging paths.
