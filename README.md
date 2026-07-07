# SOC Automation Project using Wazuh, Shuffle SOAR & TheHive

![SOC Automation Architecture](screenshots/soc-automation-architecture.png)

<p align="center">
<i><b>Figure 1.</b> End-to-end SOC Automation Architecture integrating Wazuh SIEM, Shuffle SOAR, VirusTotal threat intelligence, and TheHive for automated incident response and case management.</i>
</p>

---

## Project Overview

This project demonstrates the design and implementation of a Security Operations Center (SOC) automation platform using open-source security tools.

The objective is to automate the incident response lifecycle by integrating:

- **Wazuh** for Security Information and Event Management (SIEM)
- **Shuffle SOAR** for security orchestration and workflow automation
- **VirusTotal** for automated IOC enrichment
- **TheHive** for incident and case management
- **Sysmon** for enhanced Windows endpoint visibility
- **Windows 10** as the monitored endpoint
- **Ubuntu 24.04 LTS** cloud servers hosted on Vultr

The workflow automatically detects suspicious activity, enriches indicators of compromise (IOCs), creates cases in TheHive, and sends email notifications to analysts.

---

# Architecture

The diagram below illustrates the complete SOC automation workflow.

- Windows endpoint generates Sysmon events.
- Wazuh detects malicious activity using custom detection rules.
- Shuffle receives alerts through the Wazuh webhook.
- Shuffle enriches Indicators of Compromise (IOCs) using VirusTotal.
- TheHive automatically creates an incident for analyst investigation.
- Email notifications are sent to SOC analysts.
- Automated response actions can be executed when required.

---

# Technologies Used

| Technology | Purpose |
|------------|---------|
| Wazuh | SIEM & Endpoint Monitoring |
| Shuffle SOAR | Security Automation |
| TheHive | Incident Response & Case Management |
| VirusTotal API | IOC Enrichment |
| Sysmon | Windows Endpoint Logging |
| Windows 10 | Endpoint Agent |
| Ubuntu 24.04 LTS | Cloud Servers |
| Vultr | Cloud Infrastructure |

---

# Project Workflow

```
Windows Endpoint
        │
        ▼
Generate Sysmon Events
        │
        ▼
Wazuh SIEM
        │
        ▼
Shuffle SOAR
   │          │
   │          ▼
   │    VirusTotal
   │
   ▼
TheHive
   │
   ▼
Email Notification
   │
   ▼
SOC Analyst
```

---

# Features

- Custom Wazuh Detection Rules
- Sysmon Event Collection
- IOC Enrichment using VirusTotal
- Automated Case Creation in TheHive
- Automated Email Notifications
- Security Orchestration using Shuffle
- End-to-End Alert Automation

---

# Repository Structure

```
SOC-Automation-Project/
│
├── README.md
├── docs/
├── screenshots/
├── rules/
└── workflow/
```

---

# Documentation

Detailed installation and configuration guides are available in the **docs** directory.

- Wazuh Installation
- TheHive Installation
- Shuffle SOAR Configuration
- Sysmon Configuration
- Custom Detection Rules
- Automation Workflow
- Detection Testing

---

# Future Improvements

- Slack Notifications
- Microsoft Teams Integration
- Active Response Scripts
- Malware Sandbox Integration
- Threat Intelligence Feeds
- Automated Threat Hunting

---

# Acknowledgements

This project was built using official documentation from:

- Wazuh Documentation
- Shuffle Documentation
- TheHive Documentation
- VirusTotal API Documentation

Special thanks to the open-source security community for providing these excellent tools.


