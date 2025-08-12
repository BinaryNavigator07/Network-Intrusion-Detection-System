## Understanding the Dataset — CICIDS 2017 and 2018 Feature Breakdown

**What each column represents:**

### **1. Flow Identification & Duration**

* **Flow Duration** — Total time (in microseconds) between the first and last packet of the flow.
* **Protocol, Source Port, Destination Port** — Identify the network connection endpoints.
* **Why it matters:** Certain attacks have very short or very long flows compared to normal traffic.

---

### **2. Packet Counts and Sizes**

* **Total Fwd Packets / Total Bwd Packets** — Number of packets sent forward (source → destination) and backward.
* **Total Length of Fwd/Bwd Packets** — Sum of bytes sent in each direction.
* **Min/Max/Mean/Std of Packet Lengths** — Packet size statistics.
* **Why it matters:** Attacks often have repetitive, uniform packet sizes or unusual size patterns.

---

### **3. Timing (Inter-Arrival Times)**

* **Flow IAT Mean / Max / Min / Std** — Time gaps between packets in the whole flow.
* **Fwd IAT / Bwd IAT** — Same but calculated separately for each direction.
* **Why it matters:** Some attacks (e.g., flooding) send packets at high speed with very low IAT.

---

### **4. TCP Flags & Header Information**

* **SYN, ACK, PSH, URG, FIN, RST, ECE, CWE Flag Counts** — Count of specific TCP control flags.
* **Fwd Header Length / Bwd Header Length** — Size of headers in packets.
* **Why it matters:** Abnormal flag patterns (e.g., SYN flood) signal scanning or intrusion.

---

### **5. Bulk Data Transfer Metrics**

* **Avg Bytes/Bulk, Avg Packets/Bulk, Avg Bulk Rate** — Bulk transfer behavior in forward/backward directions.
* **Why it matters:** File transfers or data exfiltration often show large bulk metrics.

---

### **6. Subflow Statistics**

* **Subflow Fwd Packets/Bytes, Subflow Bwd Packets/Bytes** — Packets and bytes within smaller segments of the flow.
* **Why it matters:** Detects bursts of traffic or chunked data transfers.

---

### **7. Active and Idle Times**

* **Active Mean / Min / Max / Std** — Periods when packets are actively being sent.
* **Idle Mean / Min / Max / Std** — Periods with no traffic.
* **Why it matters:** Attacks like brute force might have short active bursts and long idle waits.

---

### **8. Target Column**

* **Label** — Category of the traffic:

  * **BENIGN** — Normal traffic
  * **Attack types** — e.g., DDoS, PortScan, Botnet, Brute Force, Web Attack, Infiltration, Heartbleed.
* **Why it matters:** This is the ground truth for model training.

---

### 9. Attack Surface and Explanation about Attacks
 
* **DDoS (Distributed Denial of Service)** – Overwhelms a network or service with excessive traffic, causing outages.
* **Port Scan** – Rapidly probes network ports to find vulnerabilities for later exploitation.
* **Brute Force** – Repeatedly attempts passwords or credentials until access is gained.
* **Web Attacks** – Exploits vulnerabilities in websites or web applications, such as SQL injection or cross-site scripting.
* **Infiltration** – Covertly gains unauthorized access to internal systems to steal data or install malware.
* **Botnet** – Uses a network of compromised devices to carry out coordinated malicious actions.
* **Heartbleed** – Exploits a flaw in SSL/TLS to read sensitive data from memory.

### Additional Datasets for Further Research

Beyond CICIDS 2017/2018, several other public datasets are available for network intrusion detection research. These datasets share similar features but are collected by different organizations, capturing diverse traffic patterns and attack scenarios in various environments:

- **NSL-KDD**: An improved version of the classic KDD Cup 1999 dataset, addressing issues like redundant records and class imbalance. It is widely used for benchmarking intrusion detection algorithms.  
  - [NSL-KDD Repository](https://github.com/defcom17/NSL_KDD)

- **AWID (Aegean WiFi Intrusion Dataset)**: Focused on WiFi network attacks, AWID provides labeled traffic data for evaluating intrusion detection in wireless environments.  
  - [AWID Dataset](https://icsdweb.aegean.gr/awid/)

These datasets can be used to test the generalizability of your models and to compare performance across different network environments and attack types.

**Dataset**: CICIDS 2017 and 2018 (Canadian Institute for Cybersecurity Intrusion Detection System Dataset)
**Records**: \~2.8M flows (rows) (only on 2017)
**Columns**: 79 numerical features + 1 target label


