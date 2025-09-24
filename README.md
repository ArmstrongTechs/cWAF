# Imperva WAF – Incident IOCs

## 📌 Overview
The file **`imperva_incidents.csv`** contains **Indicators of Compromise (IOCs)** exported from **Imperva Cloud WAF**.  
It provides a collection of suspicious IP addresses along with detection context, useful for threat intelligence, hunting, and security operations.

---

## ⚠️ Disclaimer
The IOCs in this dataset are collected from **Imperva WAF detections**.  
They indicate potentially malicious activity but **false positives may exist**.  
Use these indicators **for enrichment, correlation, or investigation** – not for blind blocking.  
Always validate with **additional intelligence sources, SIEM correlation, and log evidence** before taking action.

---

## 📂 File Structure
Example rows from `imperva_incidents.csv`:

| Value          | Comment                 |
|----------------|-------------------------|
| 193.142.147.5  | Illegal Resource Access |
| 77.90.185.47   | Bad Bots                |
| 152.42.255.68  | SQL Injection           |

---

## 🛡️ IOC Categories
Typical IOC categories included in this dataset:
- **Illegal Resource Access** → Unauthorized access attempts  
- **Bad Bots** → Automated scanners, crawlers, or malicious bots  
- **SQL Injection** → Attempts to exploit SQL injection vulnerabilities  
- **Others** → Based on WAF detection  

---

## 🔧 Usage
These IOCs can be consumed by multiple security workflows:
- **Threat Intelligence Sharing** → Import into platforms like **MISP**, OpenCTI  
- **SIEM Integration** → Enrich logs in **Splunk, ELK, QRadar** for correlation and alerting  
- **Blocklist Updates** → Push IP addresses into **firewall / IDS / IPS rules**  
- **Threat Hunting** → Search in system logs or EDR/XDR for potential contact with these IOCs  

---

## 🗂️ MISP Attribute Mapping
For MISP integration, CSV fields can be mapped as:

| CSV Column | MISP Attribute Type | Example                |
|------------|----------------------|------------------------|
| Value      | `ip-src` / `ip-dst` | `193.142.147.5`        |
| Comment    | `comment`            | `Illegal Resource Access` |

---

## 📜 Example: Python Loader for MISP

```python
import pandas as pd

# Load IOC feed
df = pd.read_csv("imperva_incidents.csv")

# Iterate and prepare MISP-compatible attributes
for _, row in df.iterrows():
    attribute = {
        "type": "ip-src",     # or ip-dst depending on context
        "value": row["Value"],
        "comment": row["Comment"]
    }
    print(attribute)  # This can be pushed to MISP via PyMISP API
