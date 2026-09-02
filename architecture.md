# System Architecture

## Internee.pk EDR & Malware Detection System

This document describes the architecture of the Endpoint Detection and Response (EDR) system developed for the Internee.pk internship task.

The system combines **Wazuh**, **Sysmon**, and **MalwareBazaar threat intelligence** to monitor endpoint activity, detect unauthorized file changes, identify suspicious events, and generate security alerts.

---

## 1. Architecture Overview

The system consists of four major components:

1. **Windows Endpoint**
2. **Sysmon**
3. **Wazuh**
4. **MalwareBazaar Threat Intelligence**

The overall architecture is:

```text
                         ┌─────────────────────────┐
                         │      MalwareBazaar      │
                         │   Threat Intelligence   │
                         └────────────┬────────────┘
                                      │
                                      │ Public IOCs
                                      │ Malware Hashes
                                      ▼
                         ┌─────────────────────────┐
                         │     IOC / Hash Data     │
                         │       Database          │
                         └────────────┬────────────┘
                                      │
                                      │
                                      ▼
┌──────────────────────┐     ┌──────────────────────┐
│  Windows Endpoint    │     │    Wazuh Manager     │
│                      │     │                      │
│       Sysmon         │────▶│  Log Analysis        │
│                      │     │  Detection Rules     │
│    Wazuh Agent       │────▶│  IOC Detection       │
│                      │     │  Alert Generation    │
│  File System         │────▶│  FIM Monitoring      │
└──────────────────────┘     └───────────┬──────────┘
                                         │
                                         ▼
                              ┌──────────────────────┐
                              │   Wazuh Dashboard    │
                              │                      │
                              │ Security Events      │
                              │ Alerts               │
                              │ Endpoint Monitoring  │
                              └──────────────────────┘
```

---

# 2. Architecture Components

## 2.1 Windows Endpoint

The Windows endpoint represents the system being monitored.

The endpoint contains:

* Windows operating system
* Sysmon
* Wazuh Agent
* Files and directories
* User activity
* System activity

The endpoint generates security telemetry that is collected and analyzed by Wazuh.

---

## 2.2 Sysmon

Sysmon is used to provide detailed Windows system activity information.

Sysmon can generate events related to:

* Process creation
* Process termination
* Network activity
* File activity
* System changes
* Other Windows security-relevant events

These events are collected by the Wazuh Agent and forwarded to the Wazuh Manager.

---

## 2.3 Wazuh Agent

The Wazuh Agent runs on the monitored Windows endpoint.

Its main responsibilities include:

* Collecting Sysmon events
* Collecting Windows event logs
* Performing File Integrity Monitoring
* Sending security events to the Wazuh Manager
* Collecting endpoint information

The agent acts as the communication layer between the endpoint and the Wazuh Manager.

---

## 2.4 Wazuh Manager

The Wazuh Manager is the central component of the monitoring architecture.

It receives events from Wazuh Agents and analyzes them using detection rules.

Main responsibilities include:

* Event analysis
* Security monitoring
* File Integrity Monitoring analysis
* Sysmon event analysis
* IOC matching
* Rule-based detection
* Alert generation

---

## 2.5 Wazuh Dashboard

The Wazuh Dashboard provides a centralized interface for security monitoring.

It can be used to view:

* Security alerts
* Endpoint status
* File Integrity Monitoring events
* Sysmon events
* Security logs
* Detection results
* Event statistics

The dashboard helps security analysts investigate suspicious activity.

---

# 3. File Integrity Monitoring Architecture

File Integrity Monitoring (FIM) is used to detect unauthorized changes to important files and directories.

The workflow is:

```text
Windows File System
        │
        ▼
Wazuh Agent
        │
        ▼
File Integrity Monitoring
        │
        ├── File Created
        ├── File Modified
        ├── File Deleted
        └── File Attribute Changed
        │
        ▼
Wazuh Manager
        │
        ▼
Detection Rules
        │
        ▼
Security Alert
        │
        ▼
Wazuh Dashboard
```

---

# 4. Sysmon Monitoring Architecture

Sysmon provides detailed Windows telemetry.

The workflow is:

```text
Windows Activity
       │
       ▼
     Sysmon
       │
       ▼
Sysmon Event Logs
       │
       ▼
 Wazuh Agent
       │
       ▼
Wazuh Manager
       │
       ▼
Detection Rules
       │
       ▼
Security Alerts
```

---

# 5. Malware Threat Intelligence Architecture

MalwareBazaar is used as a public source of threat intelligence.

The project uses public malware metadata and Indicators of Compromise (IOCs), particularly hashes, for defensive detection.

The workflow is:

```text
MalwareBazaar
      │
      ▼
Public Threat Intelligence
      │
      ▼
Malware Hashes / IOCs
      │
      ▼
IOC Database
      │
      ▼
Wazuh Detection Rules
      │
      ▼
Endpoint Event
      │
      ▼
IOC Match
      │
      ▼
Security Alert
```

---

# 6. Alert Generation Architecture

When suspicious activity is detected, Wazuh generates a security alert.

Example workflow:

```text
Endpoint Activity
       │
       ▼
Wazuh Agent
       │
       ▼
Wazuh Manager
       │
       ▼
Detection Rule
       │
       ├── Normal Event
       │       │
       │       ▼
       │    No Alert
       │
       └── Suspicious Event
               │
               ▼
         Security Alert
               │
               ▼
        Wazuh Dashboard
```

---

# 7. End-to-End Data Flow

The complete system data flow is:

```text
                    ┌──────────────────┐
                    │   Windows Host   │
                    └────────┬─────────┘
                             │
              ┌──────────────┴──────────────┐
              │                             │
              ▼                             ▼
        ┌───────────┐                ┌──────────────┐
        │  Sysmon   │                │ File System  │
        └─────┬─────┘                └──────┬───────┘
              │                             │
              │                             │
              └──────────────┬──────────────┘
                             ▼
                    ┌─────────────────┐
                    │  Wazuh Agent   │
                    └────────┬────────┘
                             │
                             ▼
                    ┌─────────────────┐
                    │ Wazuh Manager   │
                    └────────┬────────┘
                             │
                ┌────────────┼────────────┐
                │            │            │
                ▼            ▼            ▼
             Sysmon         FIM       IOC Matching
             Rules          Rules          Rules
                │            │            │
                └────────────┼────────────┘
                             ▼
                    ┌─────────────────┐
                    │ Security Alerts │
                    └────────┬────────┘
                             │
                             ▼
                    ┌─────────────────┐
                    │ Wazuh Dashboard │
                    └─────────────────┘
```

---

# 8. Threat Intelligence Data Flow

MalwareBazaar data is processed separately and used to improve detection capabilities.

```text
┌──────────────────────┐
│     MalwareBazaar    │
└──────────┬───────────┘
           │
           ▼
┌──────────────────────┐
│ Public Threat Data   │
└──────────┬───────────┘
           │
           ▼
┌──────────────────────┐
│ Python Processing    │
│ Script               │
└──────────┬───────────┘
           │
           ▼
┌──────────────────────┐
│ Malware Hash / IOC   │
│ Database             │
└──────────┬───────────┘
           │
           ▼
┌──────────────────────┐
│ Wazuh Detection      │
│ Rules                │
└──────────┬───────────┘
           │
           ▼
┌──────────────────────┐
│ Security Alert       │
└──────────────────────┘
```

---

# 9. Detection Scenarios

The architecture supports multiple defensive detection scenarios.

## Scenario 1 — Unauthorized File Creation

```text
User/System
    │
    ▼
Creates File
    │
    ▼
Wazuh FIM
    │
    ▼
Detection Rule
    │
    ▼
Alert
```

---

## Scenario 2 — File Modification

```text
File Modified
      │
      ▼
Wazuh FIM
      │
      ▼
Change Detected
      │
      ▼
Detection Rule
      │
      ▼
Security Alert
```

---

## Scenario 3 — Suspicious Sysmon Activity

```text
Windows Process / Activity
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
    Detection Rule
          │
          ▼
       Alert
```

---

## Scenario 4 — Malware IOC Detection

```text
Endpoint Event
      │
      ▼
File Hash / IOC
      │
      ▼
IOC Database
      │
      ▼
Hash Match
      │
      ▼
Wazuh Rule
      │
      ▼
Security Alert
```

---

# 10. Security Monitoring Layers

The architecture provides multiple monitoring layers:

| Layer               | Technology      | Purpose                       |
| ------------------- | --------------- | ----------------------------- |
| Endpoint            | Windows         | System being monitored        |
| Telemetry           | Sysmon          | Detailed Windows activity     |
| Agent               | Wazuh Agent     | Event collection              |
| FIM                 | Wazuh FIM       | File change detection         |
| Analysis            | Wazuh Manager   | Event analysis                |
| Detection           | Wazuh Rules     | Suspicious activity detection |
| Threat Intelligence | MalwareBazaar   | Public IOC information        |
| Visualization       | Wazuh Dashboard | Security monitoring           |

---

# 11. Security Benefits

This architecture provides the following security benefits:

* Centralized endpoint monitoring
* Real-time file monitoring
* File integrity detection
* Detailed Windows telemetry
* Malware IOC detection
* Threat-intelligence integration
* Automated security alerts
* Centralized security visibility
* Faster investigation of suspicious activity

---

# 12. Security Considerations

The system should be deployed only in an authorized environment.

Important security practices include:

* Protect Wazuh credentials.
* Do not commit passwords or API keys.
* Do not upload confidential company information.
* Do not upload private endpoint logs.
* Do not execute real malware on production systems.
* Use isolated systems for security testing.
* Use safe test files for FIM demonstrations.
* Use public IOC/hash metadata for threat-intelligence testing.

---

# 13. Project Architecture Summary

The complete architecture connects endpoint monitoring, security analytics, file integrity monitoring, and threat intelligence.

```text
Windows Endpoint
       │
       ├──────────► Sysmon
       │
       ├──────────► File Integrity Monitoring
       │
       └──────────► Wazuh Agent
                         │
                         ▼
                   Wazuh Manager
                         │
             ┌───────────┼───────────┐
             │           │           │
             ▼           ▼           ▼
          Sysmon        FIM       IOC Rules
          Analysis    Analysis    Matching
             │           │           │
             └───────────┼───────────┘
                         │
                         ▼
                  Security Alerts
                         │
                         ▼
                  Wazuh Dashboard

MalwareBazaar
      │
      ▼
Threat Intelligence
      │
      ▼
Malware Hashes / IOCs
      │
      ▼
Wazuh IOC Detection
```

---

## Conclusion

The proposed architecture provides a defensive EDR monitoring solution using Wazuh as the central security platform, Sysmon for Windows telemetry, File Integrity Monitoring for detecting unauthorized changes, and MalwareBazaar for public threat intelligence.

The architecture is designed to provide centralized monitoring, detection, alerting, and investigation capabilities for authorized endpoints.
