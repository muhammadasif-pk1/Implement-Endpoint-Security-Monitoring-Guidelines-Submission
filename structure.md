# Project Structure

## Internee.pk EDR & Malware Detection Project

This document describes the structure of the repository and explains the purpose of each file and folder used in the project.

---

## Repository Structure

```text
internee-edr-malware-detection/
│
├── README.md
├── structure.md
├── requirements.txt
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

## 1. Root Files

### README.md

The main documentation file of the project.

It contains:

* Project overview
* Objectives
* Technologies used
* Project architecture
* EDR implementation
* Wazuh implementation
* Sysmon integration
* MalwareBazaar integration
* Testing
* Expected results
* Security considerations

---

### structure.md

This file explains the complete GitHub repository structure and describes the purpose of every important file and directory.

---

### requirements.txt

Contains the Python dependencies required by the project automation scripts.

Example dependencies may include:

```text
requests
```

Additional packages can be added when required by the implementation.

---

### .gitignore

Used to prevent sensitive and unnecessary files from being uploaded to GitHub.

Examples:

```text
.env
*.log
*.tmp
__pycache__/
*.pyc
credentials.json
```

---

# 2. Wazuh Directory

```text
wazuh/
├── ossec.conf
├── custom_rules.xml
└── malware_hashes.txt
```

## ossec.conf

Contains the Wazuh configuration used for endpoint monitoring.

Main functions include:

* File Integrity Monitoring
* Real-time file monitoring
* User/process attribution
* Sysmon event collection
* Windows security event collection
* Windows system event collection

---

## custom_rules.xml

Contains custom Wazuh detection rules.

These rules can be used to identify:

* Suspicious file activity
* File modifications
* File creation
* File deletion
* Suspicious Sysmon events
* Malware IOC matches

The rules generate alerts when defined security conditions are detected.

---

## malware_hashes.txt

Contains public malware Indicators of Compromise (IOCs), such as file hashes, used for defensive detection.

The data can be obtained from public threat-intelligence sources such as MalwareBazaar.

Only safe, publicly available IOC information should be stored in this repository.

---

# 3. Sysmon Directory

```text
sysmon/
├── sysmon-config.xml
└── README.md
```

## sysmon-config.xml

Contains the Sysmon configuration used for Windows endpoint monitoring.

Sysmon can provide detailed information about system activity, including:

* Process creation
* Process termination
* Network-related activity
* File activity
* System changes

The collected Sysmon events are forwarded to Wazuh for analysis.

---

## sysmon/README.md

Contains information about:

* Sysmon installation
* Sysmon configuration
* Event collection
* Wazuh integration
* Testing Sysmon events

---

# 4. Scripts Directory

```text
scripts/
├── malwarebazaar_feed.py
└── alert_handler.py
```

## malwarebazaar_feed.py

Python automation script for processing public MalwareBazaar threat-intelligence information.

The script can be used to:

1. Obtain public IOC information.
2. Extract malware hashes.
3. Process the hash data.
4. Store the resulting IOCs for defensive monitoring.

The script should use malware metadata and IOC information rather than downloading or executing malware samples.

---

## alert_handler.py

Contains automation logic for handling security alerts.

Possible functions include:

* Processing Wazuh alerts
* Filtering important alerts
* Formatting alert information
* Sending notifications
* Recording security events

---

# 5. Datasets Directory

```text
datasets/
└── README.md
```

## datasets/README.md

Documents the public cybersecurity datasets and threat-intelligence sources used in the project.

The primary threat-intelligence source for this project is:

**MalwareBazaar**

The dataset documentation should describe:

* Dataset/source name
* Purpose
* Type of data
* IOC/hash information
* How the data is used
* Security considerations

---

# 6. Documentation Directory

```text
docs/
├── installation.md
├── configuration.md
├── testing.md
└── results.md
```

## installation.md

Documents the installation process for:

* Wazuh
* Wazuh Agent
* Sysmon
* Required Python components

---

## configuration.md

Documents the configuration of:

* Wazuh
* File Integrity Monitoring
* Sysmon
* Custom detection rules
* Malware IOC detection

---

## testing.md

Documents the tests performed on the EDR system.

Example tests:

* File creation
* File modification
* File deletion
* Sysmon event generation
* IOC/hash detection
* Wazuh alert generation

---

## results.md

Contains the final project results.

It can include:

* Detected events
* Generated alerts
* FIM results
* Sysmon results
* IOC detection results
* Screenshots
* Observations
* Conclusion

---

# 7. Screenshots Directory

```text
screenshots/
├── wazuh-dashboard.png
├── sysmon-event.png
├── fim-alert.png
└── malware-alert.png
```

This directory contains screenshots that provide evidence of the completed implementation.

### wazuh-dashboard.png

Screenshot of the Wazuh dashboard showing endpoint monitoring and security events.

### sysmon-event.png

Screenshot showing Sysmon events collected from the Windows endpoint.

### fim-alert.png

Screenshot showing a File Integrity Monitoring alert.

### malware-alert.png

Screenshot showing a malware IOC/hash detection alert.

---

# 8. Project Workflow

The overall workflow of the project is:

```text
                    MalwareBazaar
                          │
                          ▼
                 Public Threat Data
                          │
                          ▼
                   Malware Hashes
                          │
                          ▼
                  IOC Hash Database
                          │
                          ▼
                    Wazuh Detection
                          │
                          │
Windows Endpoint          │
      │                   │
      ▼                   │
    Sysmon                │
      │                   │
      ▼                   │
 Wazuh Agent ─────────────┘
      │
      ▼
 Wazuh Manager
      │
      ├── File Integrity Monitoring
      ├── Sysmon Analysis
      ├── IOC Detection
      └── Security Rules
              │
              ▼
       Wazuh Dashboard
              │
              ▼
        Security Alerts
```

---

# 9. Main Project Components

| Component       | Purpose                                 |
| --------------- | --------------------------------------- |
| Wazuh           | EDR and centralized security monitoring |
| Wazuh Agent     | Collects endpoint security events       |
| Wazuh Manager   | Analyzes events and generates alerts    |
| Sysmon          | Provides detailed Windows telemetry     |
| FIM             | Detects file changes                    |
| MalwareBazaar   | Provides public threat intelligence     |
| IOC Database    | Stores malware indicators               |
| Python Scripts  | Automate threat-intelligence processing |
| Wazuh Dashboard | Displays security events and alerts     |

---

# 10. Security Guidelines

The following security practices should be followed:

* Do not upload passwords or credentials.
* Do not upload API keys or authentication tokens.
* Do not upload confidential company information.
* Do not upload private internal logs.
* Do not execute real malware on production systems.
* Perform testing only in an authorized environment.
* Use safe test files for FIM testing.
* Use public IOC/hash information for threat-intelligence testing.
* Keep sensitive configuration information outside the repository.

---

# 11. Project Goal

The goal of this repository is to demonstrate a defensive cybersecurity monitoring solution capable of:

1. Monitoring endpoints.
2. Detecting file changes.
3. Collecting Sysmon activity.
4. Monitoring suspicious events.
5. Using malware threat intelligence.
6. Detecting known malware indicators.
7. Generating security alerts.
8. Providing centralized security visibility.

---

## Project Status

```text
[✓] Repository structure created
[✓] Wazuh configuration planned
[✓] Sysmon integration planned
[✓] File Integrity Monitoring planned
[✓] Malware IOC integration planned
[✓] Automated alerting planned
[✓] Testing documentation planned
[✓] Results documentation planned
```

---

## Disclaimer

This project is developed for **educational, defensive cybersecurity, and authorized internship purposes**.

All monitoring and security testing must be performed only on systems for which proper authorization has been obtained.
