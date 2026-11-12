# 🛡️ Hack The Box - Sherlock: Malicious_AutoIT

Analysis conducted and documented by: **Jonathan Di Genova** (Junior SOC Analyst & Cybersecurity Specialist)

---

## 📋 Investigation Scenario
Our organization's Security Operations Center (SOC) detected suspicious behavior linked to a compiled AutoIt script. The objective of this investigation was to perform static analysis on the suspicious Windows executable, inspect its PE headers, entropy, and internal structure, and successfully extract the embedded AutoIt source code to uncover command-and-control indicators and execution chains.

## 🛠️ Tools & Applied Skills
- **Detect It Easy (DIE):** Static file analysis, Portable Executable (PE) header inspection, section analysis, entropy calculation, and OEP identification.
- **AutoIt-Ripper (CLI):** Decompilation and extraction of compiled AutoIt script resources (`.au3`).
- **Windows Command Prompt / Python:** Execution environment navigation and script invocation.
- **Core Competencies:** Static malware triage, indicator of compromise (IoC) extraction, PE structure analysis, and obfuscated script interpretation.

## 🔍 My Approach & Methodology
1. **Initial Triage & Static Profiling:** Started by computing basic file metrics and loading the sample into Detect It Easy to evaluate file characteristics, entropy levels, and packer indicators.
2. **PE Structure & Section Analysis:** Examined section layouts, virtual addresses, and timestamps to understand compilation properties and memory alignment.
3. **Decompilation & Source Extraction:** Faced initial path and module resolution hurdles when invoking the Python-based extraction tool, which were successfully resolved by navigating to the correct directory and using proper module flags.
4. **Code Review & IoC Triage:** Analyzed the extracted AutoIt script (`script.au3`) to uncover network connections, string obfuscation, and memory injection techniques.

## 📝 Task Summary
- **Task 1:** File Hash Identification — **Task Found**
- **Task 2:** Entropy & Packing Analysis — **Task Found**
- **Task 3:** `.text` Section Virtual Address Identification — **Task Found**
- **Task 4:** Compilation Time-Date Stamp Extraction — **Task Found**
- **Task 5:** Original Entry Point (OEP) Analysis — **Task Found**
- **Task 6:** Embedded C2 Domain Identification — **Task Found**
- **Task 7:** Hex-Encoded File Path Decoding — **Task Found**
- **Task 8:** API / DLL Execution Chain Analysis — **Task Found**

## 💡 Key Takeaways
This investigation reinforced the importance of moving smoothly between high-level structural analysis (PE headers and entropy) and deep-dive script code review. Tackling compiled AutoIt loaders highlighted how adversaries use legitimate automation tools and memory-loading primitives (`DllCall` with `CallWindowProcW`) to evade disk-based detection, sharpening my analytical rigor as a Junior SOC Analyst.
