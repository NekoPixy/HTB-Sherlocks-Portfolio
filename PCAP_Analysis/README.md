# 🛡️ Hack The Box Sherlock: PCAP Analysis

> **Analysis conducted and documented by:** **Jonathan Di Genova** (Junior SOC Analyst & Cybersecurity Specialist)  
> **Platform:** Hack The Box (Sherlocks)  
> **Category:** SOC / Network Forensics  
> **Difficulty:** Very Easy  
> **Status:** Completed (100%)

---

## 📋 Investigation Scenario

Network traffic captured from a host belonging to a user identified as **P13** was flagged for forensic review. The objective of this investigation was to reconstruct the network communication, identify the external/internal endpoints involved in a suspicious file transfer, determine the underlying web server infrastructure, and measure the exact transmission duration of an encrypted payload uploaded during the session.

---

## 🛠️ Tools & Applied Skills

* **Tools Used:** `tshark`, `awk`, Kali Linux Terminal
* **Core Competencies Tested:**
  * Packet Capture (PCAPNG) Triage & Protocol Filtering
  * TCP Stream Reconstruction & Session Tracking
  * HTTP `POST` Multipart Payload Inspection
  * Network Latency & Stream Duration Calculation (`frame.time_relative` & `http.time`)
  * Blue Team Evidence Documentation & Non-Spoiler Reporting

---

## 🔍 My Approach & Methodology

1. **Endpoint & Communication Mapping (Tasks 1 & 2):**
   * I began by filtering the capture file (`Pcap_Analysis.pcapng`) to identify the primary communicating hosts and isolate HTTP traffic.
   * By inspecting TCP conversations and HTTP request headers, I separated general background chatter from the specific client-to-server file upload session.
   * **Task Found**

2. **Payload & Web Infrastructure Identification (Tasks 3, 4 & 5):**
   * Focusing on HTTP `POST` requests carrying file data (`http.file_data`), I isolated the target TCP stream responsible for the upload.
   * Examining the `multipart/form-data` headers revealed the exact filename transmitted over the wire, while server response headers (`Server:`) identified the web server software running on the destination host.
   * Tracing the application logic and server response paths allowed me to pinpoint the target upload directory on the server.
   * **Task Found**

3. **Transmission Duration & Precision Hurdle (Task 6):**
   * **Initial Hurdle:** Determining how long it took the sender to transmit the encrypted file required careful distinction between three different timing metrics inside the target TCP stream:
     * *Application response time (`http.time`)* between the HTTP request and the `200 OK` response.
     * *Payload transfer delta* from the initial HTTP `POST` packet to the server's acknowledgment.
     * *Full TCP session duration* from the initial `SYN` handshake to the final `FIN/ACK` teardown packet.
   * **Resolution:** Using `tshark` with `awk` to calculate the exact delta between the first and last `frame.time_relative` timestamps of the upload stream—and aligning the output to the platform's 4-decimal precision format—confirmed the full stream transfer duration as the expected metric.
   * **Task Found**

---

## ✅ Task Summary

1. **Task 1** – Identify sender and receiver IP addresses in network communication: `Task Found`
2. **Task 2** – Identify the destination web server IP address receiving the upload: `Task Found`
3. **Task 3** – Identify the name of the transmitted file: `Task Found`
4. **Task 4** – Identify the web server software running on the target machine: `Task Found`
5. **Task 5** – Identify the destination directory where the file was uploaded: `Task Found`
6. **Task 6** – Calculate the exact duration required by the sender to transmit the encrypted file: `Task Found`

---

## 💡 Key Takeaways

This Sherlock reinforced how critical **precision and metric definitions** are during SOC network triage. In packet forensics, "transfer time" can be interpreted at the HTTP application layer (`http.time`) or the TCP transport layer (full `SYN`-to-`FIN` stream duration). Learning to rapidly script custom timestamp deltas via `tshark` and `awk`—without relying solely on GUI conversation tables—sharpened my command-line triage speed and taught me to always verify both transport-layer boundaries and formatting constraints.
