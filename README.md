# SOC Log Analysis using Splunk
SOC log analysis using Splunk (Linux SSH brute-force detection)

This project demonstrates a SOC-style log investigation using Splunk Cloud on Linux SSH authentication logs.

The objective was to identify brute-force login attempts, analyze attacker behavior, and simulate a real-world SOC analysis workflow.

---

## 🛠 Tools & Environment

- **SIEM Tool:** Splunk Cloud Platform  
- **Log Source:** Linux SSH Authentication Logs  
- **Host:** `linux_server01`  
- **Source Type:** `linux_secure`  

---

## 🎯 Project Objectives

- Detect suspicious authentication failures
- Identify brute-force attack patterns
- Extract top attacker IP addresses
- Analyze targeted user accounts
- Visualize attack timeline

---

## 📂 Log Ingestion

The Linux authentication logs were uploaded into Splunk using the **Add Data** option.

During ingestion:
- **SourceType** was set to: `linux_secure`
- **Host** was configured as: `linux_server01`

This ensured accurate parsing and filtering of SSH authentication events.

---

## 🔍 Detection Queries Used

---

### 1️⃣ Authentication Failure Detection

```spl
source="linux_auth.log" host="linux_server01" sourcetype="linux_secure" "authentication failure"
```

**Purpose:**  
Filters all SSH authentication failure events.

---

### 2️⃣ Top Attacking IP Addresses

```spl
source="linux_auth.log" host="linux_server01" sourcetype="linux_secure" "authentication failure"
| stats count by rhost
| sort -count
```

**Finding:**  
The top attacking IP address was:  
**150.183.249.110** — 240 failed login attempts  
Indicates automated brute-force behavior.

---

### 3️⃣ Targeted User Accounts

```spl
source="linux_auth.log" host="linux_server01" sourcetype="linux_secure" "authentication failure"
| stats count by user
| sort -count
```

**Finding:**  
Most targeted account:  
**root** — 1053 failed login attempts  
Indicates privilege escalation attempts.

---

### 4️⃣ Attack Timeline Analysis

```spl
source="linux_auth.log" host="linux_server01" sourcetype="linux_secure" "authentication failure"
| timechart span=1h count
```

**Finding:**  
A significant spike was observed on:  
**July 10 around 16:00**  

This suggests an automated brute-force script attack.

---

## 🚨 Incident Summary

Multiple brute-force SSH login attempts were detected from several external IP addresses.

Attackers primarily targeted the `root` account to gain privileged access.

---

## 🛡️ Recommendations

- Enable account lockout after multiple failed login attempts  
- Implement SSH key-based authentication  
- Block malicious IP addresses at firewall level  
- Disable direct root login via SSH  
- Continuously monitor authentication logs using SIEM  

---

## 📎 Project Documentation

Full incident report in PDF:  
👉 *(Upload your PDF and paste link here)*
