# Detection Validation – Mimikatz Credential Dumping

This document validates the end-to-end detection pipeline by executing **Mimikatz** on a Windows 10 endpoint and confirming that the activity is successfully detected, enriched, and escalated through the SOC automation workflow.

---

## Objective

The purpose of this validation was to confirm that:

- Windows generates Sysmon process creation events.
- Wazuh detects Mimikatz using a custom detection rule.
- Shuffle automatically receives the alert.
- VirusTotal enriches the alert.
- TheHive automatically creates an alert.
- An email notification is sent to the SOC analyst.

---

## Test Environment

| Component | Version |
|-----------|----------|
| Windows Endpoint | Windows 10 |
| Ubuntu Server | 24.04.4 LTS |
| Wazuh Manager | 4.14.6 |
| Wazuh Agent | 4.12 |
| TheHive | 5.7.3 |
| Shuffle | Cloud |
| VirusTotal | API Integration |
| Sysmon | Installed |

---

# Attack Simulation

## Tool

Mimikatz

Repository:

https://github.com/gentilkiwi/mimikatz

---

## Detection Rule

The following custom Wazuh rule was created to detect execution of **mimikatz.exe**.

```xml
<rule id="100002" level="15">
    <if_group>sysmon_event1</if_group>

    <field name="win.eventdata.originalFileName" type="pcre2">
        (?i)mimikatz\.exe
    </field>

    <description>Mimikatz Usage Detected</description>

    <mitre>
        <id>T1003</id>
    </mitre>
</rule>
```

---

# Detection Workflow

```
Windows Endpoint
        │
        ▼
Sysmon
        │
        ▼
Wazuh Agent
        │
        ▼
Wazuh Manager
        │
        ▼
Custom Rule (100002)
        │
        ▼
Shuffle SOAR
      │      │
      │      ├────────► VirusTotal
      │      │
      │      ├────────► Email
      │
      ▼
TheHive Alert
```

---

# Step 1 – Execute Mimikatz

The Mimikatz executable was launched on the Windows endpoint.

The execution generated a **Sysmon Event ID 1 (Process Create)** event.

<p align="center">
<img src="screenshots/mimikatz-process-event.png" width="900">
</p>

<p align="center">
<b>Figure 1.</b> Sysmon Process Create event generated after executing mimikatz.exe.
</p>

---

# Step 2 – Wazuh Detection

Wazuh evaluated the Sysmon event against the custom detection rule and generated a high-severity alert.

MITRE ATT&CK Mapping:

- Technique: **T1003**
- Tactic: **Credential Access**

<p align="center">
<img src="screenshots/mimikatz-detection-results.png" width="900">
</p>

<p align="center">
<b>Figure 2.</b> Wazuh detecting Mimikatz execution using the custom Rule ID 100002.
</p>

---

# Step 3 – Shuffle Automation

The Wazuh webhook forwarded the alert to Shuffle.

Shuffle then executed the configured automation workflow.

Workflow actions:

- Receive webhook
- Extract IOC
- Query VirusTotal
- Create TheHive alert
- Send email notification

<p align="center">
<img src="screenshots/shuffle-workflow-overview.png" width="900">
</p>

<p align="center">
<b>Figure 3.</b> Shuffle SOAR workflow automatically processing the Wazuh alert.
</p>

---

# Step 4 – VirusTotal Enrichment

Shuffle extracted the SHA256 hash from the alert and queried VirusTotal.

The enrichment adds threat intelligence to the alert before analyst investigation.

---

# Step 5 – Alert Created in TheHive

After enrichment, Shuffle automatically created an alert inside TheHive.

<p align="center">
<img src="screenshots/thehive-alert-created.png" width="900">
</p>

<p align="center">
<b>Figure 4.</b> Automatically generated alert inside TheHive.
</p>

---

# Step 6 – Email Notification

Finally, Shuffle sent an email notification to the SOC analyst.

<p align="center">
<img src="screenshots/email-alert-received.png" width="900">
</p>

<p align="center">
<b>Figure 5.</b> Email notification confirming successful detection and automation.
</p>

---

# Detection Summary

| Stage | Status |
|---------|--------|
| Sysmon Process Logged | ✅ |
| Wazuh Detection | ✅ |
| Custom Rule Triggered | ✅ |
| VirusTotal Enrichment | ✅ |
| Shuffle Workflow Executed | ✅ |
| TheHive Alert Created | ✅ |
| Email Notification Sent | ✅ |

---

# MITRE ATT&CK

| Tactic | Technique | ID |
|----------|-----------|----|
| Credential Access | OS Credential Dumping | T1003 |

---

# Outcome

This validation confirms that the SOC automation pipeline functions as expected.

The workflow successfully:

- Collected endpoint telemetry using Sysmon.
- Detected malicious activity with Wazuh.
- Triggered a custom detection rule.
- Automated enrichment using VirusTotal.
- Created an alert in TheHive.
- Notified the SOC analyst via email.

This demonstrates an end-to-end automated detection and incident response workflow suitable for a Security Operations Center (SOC).

---

## Next Step

Return to the project README:

➡ **README.md**
