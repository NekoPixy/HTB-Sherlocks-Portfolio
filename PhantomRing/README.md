# 🕵️ PhantomRing - Linux Malware Analysis Sherlock

Analysis conducted and documented by: **Jonathan Di Genova** (Junior SOC Analyst & Cybersecurity Specialist)

---

## 📋 Investigation Scenario
During a routine threat hunting operation on a corporate Linux server, our SOC team intercepted a suspicious binary located in `/var/tmp` with an unusual name, actively attempting to establish outbound connections. Initial triage suggested this artifact is a stealthy post-exploitation agent. The objective of this investigation is to perform static analysis on the binary to identify its capabilities, extract core Indicators of Compromise (IOCs), and map out the threat actor's infrastructure and evasion tactics.

---

## 🛠️ Tools & Applied Skills
* **CLI Triage Utilities:** `unzip`, `sha256sum`, `strings`, `grep`
* **Disassembly & Analysis:** `objdump` (Intel syntax)
* **Core Competencies:** Static malware analysis, IOC extraction, Linux internals analysis (`procfs`, `sysfs`), evasion technique identification (`io_uring`, `eBPF` bypassing).

---

## 🧠 My Approach & Methodology
1. **Secure Extraction & Triage:** Unpacked the protected archive and established a controlled analysis environment in Kali Linux, calculating the cryptographic hash (`SHA256`) of the binary for validation and tracking.
2. **Network Infrastructure Discovery:** Extracted embedded strings and utilized pattern matching (`grep`) alongside disassembler analysis (`objdump` with `htons@plt`) to uncover hardcoded C2 communication endpoints, port configurations, and reconnection intervals.
3. **Behavioral Function Mapping:** Mapped out the agent's internal command suite by analyzing symbol tables (`cmd_` functions), revealing capabilities ranging from system enumeration to stealth operations.
4. **Deep-Dive into Evasion Mechanisms:** Investigated how the malware bypasses standard EDR monitoring via modern kernel interfaces and how it systematically blinds kernel tracing and self-destructs to eliminate forensic artifacts.

---

## ✅ Task Summary
* **Task 1 (Binary Hash Identification):** `Task Completed`
* **Task 2 (C2 IP Address Extraction):** `Task Completed`
* **Task 3 (C2 Port Discovery):** `Task Completed`
* **Task 4 (Reconnection Timeout Analysis):** `Task Completed`
* **Task 5 (Command Suite Enumeration):** `Task Completed`
* **Task 6 (EDR Syscall Evasion Interface):** `Task Completed`
* **Task 7 (Logged-in User Enumeration Source):** `Task Completed`
* **Task 8 (Privilege Escalation Target Directory):** `Task Completed`
* **Task 9 (eBPF Security Tool Fingerprint):** `Task Completed`
* **Task 10 (Kernel Tracing Disablement Target):** `Task Completed`
* **Task 11 (Self-Destruction Path Resolution):** `Task Completed`
* **Task 12 (Self-Deletion Command Trigger):** `Task Completed`

---

## 💡 Key Takeaways
Tackling PhantomRing provided incredible hands-on practice in understanding advanced Linux malware mechanics beyond simple script execution. Learning how modern threats leverage modern kernel features like `io_uring` to evade standard syscall hooks and how they actively neutralize eBPF-based security tools highlights the importance of layered, kernel-aware defense monitoring in enterprise SOC environments.
