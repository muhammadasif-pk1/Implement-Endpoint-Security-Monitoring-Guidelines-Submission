# EDR & Malware Detection System — Project Report

## 1. Project Title

**Endpoint Detection and Response (EDR) & Malware Detection System**

**Organization:** Internee.pk
**Project Type:** Cybersecurity Internship Task
**Technologies:** Wazuh, Sysmon, Python, MalwareBazaar Threat Intelligence

---

## 2. Introduction

This project focuses on developing a security monitoring and malware detection solution for protecting organizational devices and servers from malware, suspicious activities, and unauthorized access.

The solution uses **Wazuh** as the central security monitoring and EDR platform. **Sysmon** is used on Windows systems to collect detailed endpoint activity logs, while **MalwareBazaar** provides publicly available malware threat intelligence and Indicators of Compromise (IOCs).

The collected security events are monitored and analyzed to identify suspicious behavior. File Integrity Monitoring (FIM), user activity monitoring, custom detection rules, and automated alerts are used to improve the detection capability of the environment.

---

## 3. Project Objectives

The main objectives of this project are:

* Deploy an EDR/security monitoring solution using Wazuh.
* Monitor endpoint and server activities.
* Detect unauthorized file creation, modification, and deletion.
* Monitor suspicious user activities.
* Integrate Sysmon logs for detailed Windows endpoint visibility.
* Use malware-related threat intelligence from MalwareBazaar.
* Detect known malicious file hashes.
* Create automated alerts for suspicious activities.
* Improve visibility into endpoint security events.
* Provide documentation and testing evidence for the implemented solution.

---

## 4. Proposed Security Architecture

The overall architecture consists of Windows endpoints, Sysmon, Wazuh agents, the Wazuh manager, and threat intelligence sources.

```text
                 +----------------------+
                 |   MalwareBazaar      |
                 | Threat Intelligence  |
                 +----------+-----------+
                            |
                            v
                 +----------------------+
                 | Malware Hash / IOCs  |
                 +----------+-----------+
                            |
                            v
+----------------+   +----------------------+
| Windows        |   | Wazuh Manager        |
| Endpoint       +-->| Detection & Analysis |
+-------+--------+   +----------+-----------+
        |                       |
        v                       v
+----------------+     +--------------------+
| Sysmon         |     | Wazuh Dashboard    |
| Event Logging  |     | Alerts & Monitoring|
+----------------+     +--------------------+
        |
        v
+----------------+
| Wazuh Agent    |
+----------------+
```

---

## 5. Technologies Used

### 5.1 Wazuh

Wazuh is used as the main security monitoring and EDR platform.

It provides:

* Endpoint monitoring
* File Integrity Monitoring
* Security event analysis
* Log collection
* Detection rules
* Alert generation
* Security dashboards
* Agent-based endpoint monitoring

### 5.2 Sysmon

Sysmon is used on Windows endpoints to provide detailed system activity logs.

Relevant events can include:

* Process creation
* Network connections
* File creation
* Registry activity
* Process termination
* Driver and DLL related activity

These events provide additional visibility for detecting suspicious endpoint behavior.

### 5.3 MalwareBazaar

MalwareBazaar is used as a public threat intelligence source.

The project uses malware-related metadata and Indicators of Compromise such as:

* File hashes
* Malware family information
* IOC information

Only safe threat-intelligence metadata should be stored in this repository. Malware samples must not be downloaded, executed, or committed to GitHub.

### 5.4 Python

Python scripts are used for automation tasks such as:

* Retrieving threat intelligence metadata
* Processing malware hashes
* Updating IOC data
* Supporting alert automation

---

## 6. File Integrity Monitoring

File Integrity Monitoring (FIM) is one of the important components of this project.

The system monitors selected directories and files for unauthorized changes.

The following activities can be monitored:

* File creation
* File modification
* File deletion
* Permission changes
* Ownership changes
* Other relevant file attributes

For example, if an unauthorized user modifies an important configuration file, Wazuh can generate a security event and alert.

### FIM Workflow

```text
File Activity
     |
     v
Wazuh Agent
     |
     v
FIM Detection
     |
     v
Wazuh Manager
     |
     v
Detection Rule
     |
     v
Security Alert
```

---

## 7. Sysmon Monitoring

Sysmon provides detailed endpoint telemetry that can be collected by Wazuh.

The integration improves visibility into activities such as process execution and network behavior.

Example monitoring flow:

```text
Windows Endpoint
       |
       v
     Sysmon
       |
       v
Sysmon Event Logs
       |
       v
  Wazuh Agent
       |
       v
 Wazuh Manager
       |
       v
Detection & Alert
```

Sysmon logs can help identify suspicious process execution, unusual network activity, and other endpoint behaviors.

---

## 8. Malware Detection

Malware detection is implemented using a combination of endpoint monitoring and threat intelligence.

A simplified detection process is:

```text
Endpoint Activity
       |
       v
File / Process Event
       |
       v
IOC / Hash Check
       |
       +------ Known IOC ------> Alert
       |
       +------ No Match -------> Continue Monitoring
```

Known malicious hashes can be maintained in a local IOC list and matched against relevant security events.

The purpose of this mechanism is to identify files or activities associated with known malicious indicators.

---

## 9. Threat Intelligence Integration

Public threat intelligence from MalwareBazaar can be used to enrich the detection system.

The workflow is:

```text
MalwareBazaar
     |
     v
Threat Intelligence Data
     |
     v
Extract Relevant Hashes
     |
     v
IOC / Hash List
     |
     v
Wazuh Detection Rules
     |
     v
Security Alert
```

The project should store only relevant metadata and indicators in the GitHub repository.

Sensitive organizational information, credentials, private logs, or internal infrastructure details must not be uploaded.

---

## 10. Alert Automation

Automated alerts are used to notify administrators when suspicious activity is detected.

Potential alert conditions include:

* Known malicious hash detected
* Unauthorized file modification
* Suspicious file creation
* Important file deletion
* Suspicious process activity
* Abnormal user activity
* Other rule-based security events

Example:

```text
Suspicious Activity
        |
        v
Detection Rule
        |
        v
Wazuh Alert
        |
        v
Alert Handler
        |
        v
Administrator Notification
```

---

## 11. Detection Scenarios

The following scenarios can be used for testing the system.

### Scenario 1 — File Creation

A test file is created inside a monitored directory.

**Expected Result:**

Wazuh detects the file creation and generates an event/alert according to the configured monitoring rules.

---

### Scenario 2 — File Modification

An existing monitored file is modified.

**Expected Result:**

The modification is detected by File Integrity Monitoring.

---

### Scenario 3 — File Deletion

A monitored test file is deleted.

**Expected Result:**

Wazuh records the deletion event and generates an alert if the configured rule matches.

---

### Scenario 4 — Suspicious Hash

A safe test event containing a known public IOC/hash is used for detection validation.

**Expected Result:**

The configured detection logic identifies the matching IOC and generates an alert.

> Testing should use safe simulated events and authorized lab data. Do not execute malware samples.

---

### Scenario 5 — Sysmon Event

A normal administrative activity generates a Sysmon event.

**Expected Result:**

The event is collected by the Wazuh agent and becomes visible for analysis.

---

## 12. Testing Methodology

Testing is performed in an authorized and controlled environment.

The testing process includes:

1. Verify Wazuh agent connectivity.
2. Verify Sysmon event collection.
3. Configure monitored directories.
4. Create safe test files.
5. Modify test files.
6. Delete test files.
7. Generate safe test endpoint activity.
8. Verify events in Wazuh.
9. Verify detection rules.
10. Verify generated alerts.
11. Record screenshots and observations.

No real malware is required for functional testing.

---

## 13. Expected Results

After successful implementation, the system should provide:

* Centralized endpoint monitoring.
* Visibility into Windows Sysmon events.
* File integrity monitoring.
* Detection of suspicious file activity.
* IOC/hash-based malware detection.
* Automated security alerts.
* Better visibility into endpoint behavior.
* A documented security monitoring workflow.

---

## 14. Security Considerations

The following security practices should be followed:

* Do not commit passwords or API keys to GitHub.
* Do not upload private organizational logs.
* Do not upload real malware samples.
* Do not execute malware on production systems.
* Perform testing only on authorized systems.
* Keep threat intelligence data limited to safe metadata and IOCs.
* Use `.gitignore` to prevent accidental upload of sensitive files.
* Regularly update threat intelligence data.
* Review and tune detection rules to reduce false positives.

---

## 15. Repository Structure

The project repository is organized as follows:

```text
internee-edr-malware-detection/
│
├── README.md
├── structure.md
├── architecture.md
├── requirements.txt
├── report.md
├── .gitignore
│
├── wazuh/
│   ├── ossec.conf
│   ├── custom_rules.xml
│   └── malware_hashes.txt
│
├── sysmon/
│   ├── sysmon-config.xml
│   └── README.md
│
├── scripts/
│   ├── malwarebazaar_feed.py
│   └── alert_handler.py
│
├── datasets/
│   └── README.md
│
├── docs/
│   ├── installation.md
│   ├── configuration.md
│   ├── testing.md
│   └── results.md
│
└── screenshots/
    ├── wazuh-dashboard.png
    ├── sysmon-event.png
    ├── fim-alert.png
    └── malware-alert.png
```

---

## 16. Project Outcomes

The project demonstrates how an EDR-style monitoring architecture can combine endpoint telemetry, file integrity monitoring, threat intelligence, and automated alerting.

The implementation provides a foundation for detecting suspicious activities on organizational endpoints and servers.

The combination of Wazuh and Sysmon improves endpoint visibility, while public threat intelligence can be used to enrich malware detection.

---

## 17. Limitations

This project has some limitations:

* IOC-based detection may not detect previously unknown malware.
* Detection rules require continuous tuning.
* Sysmon configuration affects the quality and volume of collected events.
* Public threat intelligence may contain outdated or incomplete indicators.
* False positives may occur.
* Production deployment requires additional security controls and monitoring.

---

## 18. Future Improvements

Possible future improvements include:

* Integration with additional threat intelligence feeds.
* Advanced behavioral detection.
* YARA-based file analysis.
* VirusTotal enrichment where appropriate.
* Email or messaging-based alert notifications.
* Automated incident response.
* Centralized long-term security analytics.
* Improved dashboards and reporting.
* Detection rule optimization.
* Integration with a Security Information and Event Management (SIEM) workflow.

---

## 19. Conclusion

The EDR & Malware Detection System provides a practical approach for monitoring endpoint activity and detecting suspicious behavior.

Wazuh acts as the central monitoring and detection platform, while Sysmon provides detailed Windows endpoint telemetry. File Integrity Monitoring helps identify unauthorized file changes, and threat intelligence from MalwareBazaar can be used to identify known malicious indicators.

The project demonstrates the basic components required for an endpoint security monitoring solution and provides a foundation that can be further expanded into a more advanced enterprise security architecture.

---

## 20. Evidence / Screenshots

The following screenshots should be added to the `screenshots/` directory after testing:

### Wazuh Dashboard

```text
screenshots/wazuh-dashboard.png
```

Shows the Wazuh dashboard and connected endpoint information.

### Sysmon Event

```text
screenshots/sysmon-event.png
```

Shows a Sysmon-generated event collected from the Windows endpoint.

### File Integrity Alert

```text
screenshots/fim-alert.png
```

Shows an alert generated after a monitored file was safely created, modified, or deleted.

### Malware/IOC Alert

```text
screenshots/malware-alert.png
```

Shows a safe IOC/hash detection test and the corresponding Wazuh alert.

---

## 21. Final Status

**Project:** EDR & Malware Detection System
**Platform:** Wazuh
**Endpoint Telemetry:** Sysmon
**Threat Intelligence:** MalwareBazaar
**Automation:** Python
**Monitoring:** File Integrity + Endpoint Activity
**Alerting:** Wazuh Detection Rules

**Status:** Documentation and implementation prepared for authorized testing and deployment.
