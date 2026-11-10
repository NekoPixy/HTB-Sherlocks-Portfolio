# 🛡️ Hack The Box - Sherlock Investigation: WordpressWebForensics

Analysis conducted and documented by: **Jonathan Di Genova** (Junior SOC Analyst & Cybersecurity Specialist)

---

## 📋 Investigation Scenario
A corporate web server running WordPress suffered a critical security breach, leading to significant disruption across the digital environment. As a DFIR (Digital Forensics and Incident Response) Analyst, my objective was to query a provided Splunk SIEM instance loaded with Apache2 access and error logs to reconstruct the full kill chain—from initial enumeration and credential brute-forcing to web shell deployment and persistence.

---

## 🧰 Tools & Applied Skills
- **SIEM & Log Analysis:** Splunk (Querying access and error logs, sorting timelines, string matching).
- **Threat Intelligence & Reconnaissance Triage:** Identifying automated scanner footprints, User-Agents, and enumeration signatures.
- **Web Application Forensics:** Tracking brute-force attacks (`wp-login.php`), administrative session hijacking, theme file inspection, and remote command execution via webshells.
- **Competencies Tested:** Log triage, timeline reconstruction, indicator of compromise (IoC) extraction, and Blue Team defensive analysis.

---

## 🔍 My Approach & Methodology (Step-by-Step)
1. **Initial Reconnaissance Analysis:** Filtered web server logs in Splunk to isolate external IP traffic, identifying the origin of the initial enumeration activity and the specific vulnerability scanning utility used.
2. **Authentication Attack Reconstruction:** Tracked the transition from reconnaissance to aggressive action by analyzing POST requests targeting the WordPress authentication gateway, precisely measuring the duration of the brute-force phase leading up to a successful login status code (`302`).
3. **Post-Exploitation & Persistence Tracking:** Investigated post-authentication activity to locate administrative interactions with core template directories, identifying the exact target theme file modified by the attacker.
4. **Web Shell Execution Analysis:** Examined subsequent HTTP POST requests and associated PHP error logs to uncover remote command execution patterns, pinpointing the exact timestamp and User-Agent signature utilized to interact with the backdoor.

---

## ✅ Task Summary
- **Task 1:** Identify the attacker's enumeration IP address — **Task Found**
- **Task 2:** Determine the start timestamp of reconnaissance activity — **Task Found**
- **Task 3:** Identify the enumeration tool and version — **Task Found**
- **Task 4:** Calculate the duration of the login brute-force attack — **Task Found**
- **Task 5:** Identify the targeted WordPress theme for persistence — **Task Found**
- **Task 6:** Determine the specific PHP theme file used as a web shell entry point — **Task Found**
- **Task 7:** Establish the exact timestamp of the first command execution via the web shell — **Task Found**
- **Task 8:** Document the User-Agent signature used to access the web shell — **Task Found**

---

## 💡 Key Takeaways
Tackling *WordpressWebForensics* provided a clear, end-to-end perspective on how automated web scanners bridge into targeted credential attacks and persistent backdoors. Correlating raw Apache access logs with PHP error logs in Splunk reinforced the importance of monitoring file-integrity anomalies within CMS template structures. This lab sharpens core Blue Team instincts: patience, strict chronological validation, and methodical log filtering.
