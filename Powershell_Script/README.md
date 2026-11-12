# 🛡️ Hack The Box Sherlock: PowerShell_Script

> **Analysis conducted and documented by:** **Jonathan Di Genova** *(Junior SOC Analyst & Cybersecurity Specialist)*  
> **Platform:** Hack The Box (Sherlocks) | **Category:** Malware Analysis | **Difficulty:** Very Easy  
> **Status:** ✅ Completed (100%)

---

## 📋 Investigation Scenario

During routine security monitoring, a suspicious PowerShell script (`script.ps1`) containing a heavily obfuscated payload was isolated on a workstation. The objective of this static malware analysis investigation is to safely dissect the execution parameters, decode the underlying payload without running it in a live production environment, map its network communication capabilities, and extract key Indicators of Compromise (IoCs) to strengthen endpoint defenses.

---

## 🛠️ Tools & Applied Skills

* **Tools Used:**
  * **CyberChef:** Used for multi-stage decoding (`From Base64` and `UTF-16LE` text decoding) to deobfuscate the embedded PowerShell payload.
  * **Text Editor / Static Code Inspection:** Used to examine the outer command-line launcher flags and analyze .NET object instantiations.
  * **RDP (`xfreerdp`):** Used to securely access the isolated analysis sandbox.
* **Applied Blue Team Competencies:**
  * Static Malware Analysis & Deobfuscation
  * PowerShell Command-Line Flag & Defense Evasion Analysis
  * .NET Framework Network Class Identification (`WebClient`, `CredentialCache`)
  * HTTP Header Spoofing & Command-and-Control (C2) Staging Analysis

---

## 🔍 My Approach & Methodology (Step-by-Step)

To ensure a structured, repeatable Blue Team workflow, I broke the investigation down into three logical phases:

### Phase 1: Outer Wrapper & Execution Flag Analysis
I began by inspecting `script.ps1` statically to understand how the Windows PowerShell interpreter was being invoked. Rather than immediately decoding the large string block, I analyzed the command-line arguments passed to `powershell.exe`:
* **Hurdle & Resolution:** Initially, I focused only on single-letter parameter abbreviations when identifying how the script hides its execution window. By carefully reviewing the exact command syntax and spacing, I confirmed that both the parameter flag and its specific state argument work together to suppress the GUI console from the end user.
* **Behavioral Insights:** I identified additional evasion flags designed to bypass local user profile loading (`-NoP`), enforce a non-interactive session so user prompts or error dialogues cannot halt or interrupt execution (`-NonI`), and pass an encoded command block (`-Enc`). **Task Found**

### Phase 2: Multi-Layer Payload Deobfuscation
Once the launcher behavior was clear, I extracted the long encoded string following the `-Enc` parameter and loaded it into **CyberChef**.
* **Hurdle & Resolution:** Applying a standard Base64 decode alone produced null bytes between every character. Remembering that Windows PowerShell natively encodes `-EncodedCommand` strings in **UTF-16LE (1200)**, I chained the `Decode text` operation right after `From Base64` in my CyberChef recipe. This immediately revealed clean, readable PowerShell and .NET code. **Task Found**

### Phase 3: Network Capability, Evasion & C2 Extraction
With the cleartext script exposed (despite randomized capitalization used to evade simple signature matching), I analyzed the execution flow line by line:
1. **Web Interaction:** Identified the instantiation of a .NET `WebClient` object used to handle HTTP requests and retrieve remote resources.
2. **Defense Evasion (User-Agent Spoofing):** Observed the script manually overwriting the default PowerShell HTTP header with a legacy browser User-Agent string so malicious traffic would blend in with standard web browsing.
3. **Proxy Authentication:** Noted how the script inherits the compromised host's default system proxy and network credentials to seamlessly traverse corporate perimeter proxies.
4. **C2 Staging & Execution:** Traced the final routine where the script reaches out to an external IP address over a non-standard port to download an `.asp` payload, decodes/deobfuscates it in memory using a custom XOR loop, and executes it dynamically via `Invoke-Expression` (`IEX`). **Task Found**

---

## ✅ Task Summary

1. **Task 0 — Payload Encoding Identification:** `Task Found`
2. **Task 1 — Window Visibility Suppression Parameter:** `Task Found`
3. **Task 2 — Non-Interactive Process Execution Flag:** `Task Found`
4. **Task 3 — .NET Web Interaction Instantiation:** `Task Found`
5. **Task 4 — Spoofed HTTP User-Agent String:** `Task Found`
6. **Task 5 — Automated Proxy Credential Configuration:** `Task Found`
7. **Task 6 — External C2 Payload Download URL:** `Task Found`

---

## 💡 Key Takeaways

* **Always Account for UTF-16LE in PowerShell:** When decoding `-EncodedCommand` payloads in CyberChef or CLI tools, pairing Base64 decoding with UTF-16LE text decoding is essential to avoid null-byte artifacts.
* **Context Matters in Command-Line Triage:** Attackers combine multiple flags (`-NoP`, `-NonI`, `-W Hidden`) to achieve stealth and execution resilience. Recognizing these combinations helps SOC analysts quickly triage suspicious process creation events (Event ID 4688 / Sysmon Event ID 1).
* **Mixed-Case Obfuscation:** Adversaries frequently randomize uppercase and lowercase letters in .NET class names (e.g., `New-ObJeCt SyStEm.NET.WebClIeNt`) to bypass naive case-sensitive string detections, making behavioral and AST-based detection rules critical.
