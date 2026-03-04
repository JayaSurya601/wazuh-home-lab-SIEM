**🛡️ Wazuh SIEM Home Lab – WAF + Web Attack Detection**
![SIEM](https://img.shields.io/badge/SIEM-Wazuh-blue)
![WAF](https://img.shields.io/badge/WAF-ModSecurity-green)
![Attack Simulation](https://img.shields.io/badge/Attack-XSS-red)
![Platform](https://img.shields.io/badge/Platform-Ubuntu-orange)

A Security Operations Center (SOC) home lab demonstrating real-world web attack detection using Wazuh SIEM integrated with a Web Application Firewall (ModSecurity + OWASP CRS).
This project simulates a Cross-Site Scripting (XSS) attack against a vulnerable web application (DVWA) and shows how the attack is:

Detected → Blocked by WAF → Logged → Sent to SIEM → Investigated in SOC Dashboard

**⭐ Project Highlights**

• Simulated Cross-Site Scripting (XSS) attack against DVWA
• Protected web server using ModSecurity WAF + OWASP CRS
• Collected logs using Wazuh Agent
• Generated SIEM alerts in Wazuh Dashboard
• Investigated attack through SOC-style workflow

**🏗️ Lab Architecture**

The environment consists of three main systems:

  **System**	            **Purpose**
  
Web Server	        Hosts DVWA and WAF protection
Wazuh SIEM Server	  Centralized log analysis and alerting
Attacker Machine	  Simulates web attacks

**🧩 Components**

**🖥️ Web Server (Monitored Host)**

• Ubuntu Server
• Apache Web Server
• DVWA (Damn Vulnerable Web Application)
• ModSecurity Web Application Firewall
• OWASP Core Rule Set (CRS) v4.21.0
• Wazuh Agent
The Wazuh agent monitors system and web server logs and forwards them to the Wazuh Manager.

**🛡️ Wazuh SIEM Server**

• Wazuh Manager – Security event analysis
• Wazuh Indexer (OpenSearch) – Data indexing and storage
• Wazuh API – Management interface
• Wazuh Dashboard – Visualization and investigation interface

**🧪 Attacker Machine**

• Kali Linux
• Used to simulate Cross-Site Scripting (XSS) attacks against the DVWA application.

**🌐 Network Communication**

| Source             | Destination        | Port  | Purpose                    |
| ------------------ | ------------------ | ----- | -------------------------- |
| Attacker           | Web Server         | 80    | HTTP web requests          |
| Wazuh Agent        | Wazuh Manager      | 1514  | Encrypted log transmission |
| Agent Registration | Wazuh Manager      | 1515  | Secure agent enrollment    |
| Wazuh Manager      | OpenSearch Indexer | 9200  | Alert indexing             |
| User Browser       | Wazuh Dashboard    | 443   | Dashboard access           |
| Dashboard          | Wazuh API          | 55000 | Management communication   |

**🛡️ Web Application Firewall Integration**

The web server is protected using ModSecurity with the OWASP Core Rule Set (CRS).

**Configuration**

• ModSecurity enabled in Apache
• OWASP CRS deployed
• Blocking mode enabled
• Security events logged to: /var/log/apache2/error.log

**🚨 Attack Simulation**

A Cross-Site Scripting (XSS) payload was injected into the DVWA application.

**Payload Used**
```
<script>alert(123)</script>
```
This payload attempts to execute JavaScript in the victim's browser.

**📊 Example SIEM Alert (Wazuh)**

After the attack was performed, Wazuh generated a security alert from the Apache access log.

| Field         | Value                              |
| ------------- | ---------------------------------- |
| Rule ID       | 31105                              |
| Description   | XSS (Cross Site Scripting) attempt |
| Source IP     | 192.168.1.2                        |
| Target Server | 192.168.1.6                        |
| HTTP Method   | GET                                |
| HTTP Status   | 403                                |
| Log Source    | /var/log/apache2/access.log        |
| Decoder       | web-accesslog                      |

**Captured Request**
```
GET /DVWA/vulnerabilities/xss_r/?name=<script>alert(123)</script>
```
**Encoded version:**
```
/DVWA/vulnerabilities/xss_r/?name=%3Cscript%3Ealert%28123%29%3C%2Fscript%3E
```
**🛡️ WAF Detection**

The OWASP CRS detected the malicious payload using multiple detection rules.

| Rule ID | Description                   |
| ------- | ----------------------------- |
| 941100  | XSS detected via libinjection |
| 941110  | Script tag injection          |
| 941160  | HTML injection pattern        |
| 941390  | Javascript method detected    |

🚫 WAF Blocking Rule

| Rule ID | Description                    |
| ------- | ------------------------------ |
| 949110  | Inbound anomaly score exceeded |

Anomaly Score: 20

Threshold: 5

Result: HTTP 403 – Request Blocked

**🔎 WAF Detection Pipeline**

```
Attacker
   │
   │ <script>alert(123)</script>
   ▼
DVWA Web Server
   │
   ▼
ModSecurity WAF
   │
   ▼
OWASP CRS Detection Rules
   │
   ├─ 941100 XSS detected via libinjection
   ├─ 941110 Script tag injection
   ├─ 941160 HTML injection pattern
   ├─ 941390 Javascript method detected
   ▼
Anomaly Score = 20
Threshold = 5
   ▼
949110 → Request Blocked
   ▼
Apache WAF Log
   ▼
Wazuh Agent
   ▼
Wazuh Manager → OpenSearch Indexer → Wazuh Dashboard
```

**🔎 SOC Investigation Summary**

**Attack Type:** Cross-Site Scripting (XSS)

**Investigation Steps**

1.Suspicious HTTP request detected in Apache access logs
2.Request contained an XSS payload
3.ModSecurity OWASP CRS triggered detection rules
4.Anomaly score exceeded the threshold
5.Request blocked with HTTP 403
6.Wazuh agent forwarded logs to the Wazuh Manager
7.Wazuh SIEM generated an alert in the dashboard

**Conclusion**

The layered defense architecture successfully detected, blocked, and logged the attack, enabling SOC analysts to investigate the event through the SIEM dashboard.

**🎯 MITRE ATT&CK Mapping**
| Attack Technique           | MITRE ID | Description                                    |
| -------------------------- | -------- | ---------------------------------------------- |
| Cross-Site Scripting (XSS) | T1059    | Command and script execution through web input |
| Web Application Attack     | T1190    | Exploitation of public-facing application      |

**Detection Source**

• Apache access logs
• ModSecurity WAF alerts
• Wazuh SIEM correlation rules

**🏆 Skills Demonstrated**

• SIEM Deployment using Wazuh
• Web Application Firewall configuration (ModSecurity + OWASP CRS)
• Log collection and centralized monitoring
• Security alert analysis
• SOC investigation workflow
• Network traffic analysis
• Service and port verification
• Layered defense architecture

**🎥 Video Demonstration**

The video shows:
• XSS attack from Kali Linux
• WAF blocking the request
• Logs generated in Apache
• Alert generated in Wazuh dashboard

**🚀 Why This Project Matters**

This lab demonstrates practical Blue Team security skills including:
• Real attack simulation
• Web application attack detection
• WAF + SIEM integration
• End-to-end security monitoring pipeline
• SOC-style alert investigation

**🔮 Future Enhancements**

• Active response configuration
• Threat intelligence integration
• Email / Slack alerting
• Monitoring multiple agents
• Custom decoders and rules

**📂 Repository Structure**

wazuh-home-lab-SIEM

├── README.md
│
├── architecture
│   └── Lab_Architecture_Diagram.png
│
├── screenshots
│   ├── XSS_(Cross_Site_Scripting)_attempt.png
│   ├── ModSecurity_Rejected_Query.png

**🎯 Conclusion**

This project demonstrates a complete defensive monitoring pipeline used in modern SOC environments.

**The lab successfully shows:**

• Web application attack detection
• Web Application Firewall protection
• Secure log collection
• SIEM-based alert generation
• SOC investigation workflow
