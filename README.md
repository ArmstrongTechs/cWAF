# Imperva WAF – Incident IOCs

## 📌 Overview
ไฟล์ **`imperva_incidents.csv`** นี้เป็น IOC feed ที่ได้มาจาก **Imperva WAF (Cloud Web Application Firewall)**  
ซึ่งบันทึก **Indicators of Compromise (IOCs)** ที่ตรวจจับได้จากการโจมตีหรือพฤติกรรมผิดปกติที่เข้ามายังระบบ

ข้อมูลในไฟล์ประกอบด้วย:
- **Value** → ค่า IOC (IP Address ที่ตรวจจับได้)  
- **Comment** → หมายเหตุ/ประเภทการโจมตี  

---

## 📂 File Structure
ตัวอย่างข้อมูลจาก `imperva_incidents.csv`:

| Value          | Comment                 |
|----------------|-------------------------|
| 193.142.147.5  | Illegal Resource Access |
| 77.90.185.47   | Bad Bots                |
| 152.42.255.68  | SQL Injection           |

---

## 🛡️ IOC Categories
IOC ในไฟล์นี้แบ่งออกตามหมวดหมู่ เช่น:
- **Illegal Resource Access** → การเข้าถึงไฟล์/ทรัพยากรที่ไม่ได้รับอนุญาต  
- **Bad Bots** → Bot ผิดกฎหมายที่สแกนหรือเก็บข้อมูล  
- **SQL Injection** → การพยายามโจมตีฐานข้อมูลผ่านช่องโหว่ SQLi  
- **อื่น ๆ** ตามที่ WAF ตรวจจับได้  

---

## 🔧 Usage
คุณสามารถนำ IOC เหล่านี้ไปใช้งานได้ดังนี้:
- **Threat Intelligence Sharing** → แชร์ IOC เข้าสู่ระบบ Threat Intel (เช่น **MISP**, OpenCTI)  
- **SIEM Integration** → ส่ง IOC เข้า **Splunk, ELK** เพื่อ correlation และ alerting  
- **Blocklist Update** → เพิ่ม IP ลง **Firewall / IDS / IPS blocklist**  
- **Threat Hunting** → ใช้ IOC ค้นหาในระบบ Log หรือ Endpoint ว่ามีการติดต่อกับ IOC เหล่านี้หรือไม่  

---

## 🗂️ MISP Attribute Mapping
เมื่อ import IOC เข้าสู่ **MISP** สามารถ mapping ได้ตามนี้:

| CSV Column | MISP Attribute Type | Example                |
|------------|----------------------|------------------------|
| Value      | `ip-src` / `ip-dst` | `193.142.147.5`        |
| Comment    | `comment`           | `Illegal Resource Access` |

---

## 📜 Example (Python Import to MISP)

```python
import pandas as pd

df = pd.read_csv("imperva_incidents.csv")

for _, row in df.iterrows():
    attribute = {
        "type": "ip-src",          # or ip-dst
        "value": row["Value"],
        "comment": row["Comment"]
    }
    print(attribute)  # ส่งเข้า MISP API ได้
