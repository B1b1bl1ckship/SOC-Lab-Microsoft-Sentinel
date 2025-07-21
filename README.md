# SOC-Lab-Microsoft-Sentinel

# 🛡️ Building a Cloud-Based SOC Lab Using Microsoft Sentinel

This project documents my first step into building a personal, cloud-based Security Operations Center (SOC) lab using Microsoft Azure and Microsoft Sentinel. It focuses on setting up infrastructure, ingesting Windows security logs, querying data using KQL, and visualizing login attempts with enriched GeoIP data — all within a real cloud environment.

📖 **Full Article on Medium →** [My Journey to Building a Cloud-Based SOC Lab](https://medium.com/@pattel.milan/my-journey-to-building-a-cloud-based-soc-lab-at-home-using-microsoft-sentinel-40d257d593f7)

---## 🖼️ Visual Overview

Here’s a high-level architecture of the SOC lab setup built on Azure using Microsoft Sentinel:

![SOC Lab Architecture](./sentinel-architecture.png)


## 🧩 Key Features

- ✅ Provisioned complete SOC infrastructure on **Azure**: Resource Group, Virtual Network, and Virtual Machine  
- ✅ Configured **Microsoft Sentinel** to ingest **Windows Security Event logs** via AMA (Agent Management)  
- ✅ Simulated **failed login attempts** manually by entering wrong credentials via RDP  
- ✅ Queried Event ID **4625** (failed login) using **KQL** in Log Analytics  
- ✅ Uploaded a **GeoIP Watchlist** (CSV with 19k IP range entries) to enrich logs with location data  
- ✅ Built a **map visualization** in Microsoft Sentinel Workbooks to display attacker IPs and locations  
- ✅ Practiced **log correlation, event filtering**, and map-based threat visualization

---

## ⚙️ Lab Components

| Component            | Details                                 |
|----------------------|------------------------------------------|
| Cloud Provider       | Microsoft Azure                         |
| SIEM Platform        | Microsoft Sentinel                      |
| Log Source           | Windows 11 VM (RDP + Security Logs)     |
| Event Focus          | Event ID 4625 – Failed Logon Attempts   |
| Query Language       | Kusto Query Language (KQL)              |
| Visualization        | Azure Sentinel Workbooks (Map Type)     |
| Data Enrichment      | Watchlist with GeoIP Mapping            |

---

## 🔍 Example KQL Query

```kql
let GeoIPDB_FULL = _GetWatchlist("geoip");
let WindowsEvents = SecurityEvent;
WindowsEvents
| where EventID == 4625
| order by TimeGenerated desc
| evaluate ipv4_lookup(GeoIPDB_FULL, IpAddress, network)
| project TimeGenerated, Computer, AttackerIp = IpAddress, cityname, countryname, latitude, longitude, 
  friendly_location = strcat(cityname, " (", countryname, ")")

---
# 🚀 Next Steps

This lab forms the **foundation** of my SOC journey. Future phases will include:

- 🔐 **Integrate Active Directory logs** into Sentinel  
- ⚠️ **Simulate attack scenarios** like Password Spray and Brute Force  
- 📊 **Build KQL-based analytics rules** mapped to MITRE ATT&CK (e.g., T1110.003)  
- 🔔 Configure **Scheduled Analytics Rules** to generate alerts for suspicious login behavior  
- 🧠 Practice **incident triage** and alert investigation  
- 🤖 Explore **Logic App playbooks** for automated response  
- 📍 Create custom **visual dashboards and maps** for security monitoring

---

## 📬 Contact Me

**Milan Patel**  
🔗 [LinkedIn](https://www.linkedin.com/in/milan0)  
✉️ pattel.milan@gmail.com  

If you found this project helpful or want to collaborate on more security projects, feel free to connect!

---
