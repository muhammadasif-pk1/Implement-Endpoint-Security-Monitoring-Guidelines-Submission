# Implement-Endpoint-Security-Monitoring-Guidelines-Submission
Implement endpoint security to protect devices. Deploy EDR tools like Wazuh to monitor files and user activity. Use logs and threat data to trigger alerts for suspicious behavior.
# Internee.pk EDR & Malware Detection System

## Project Overview

This project implements an Endpoint Detection and Response (EDR) monitoring solution using Wazuh, Sysmon, and MalwareBazaar threat intelligence.

The objective is to improve endpoint security by monitoring file integrity, tracking suspicious user and system activity, detecting known malware indicators, and generating automated security alerts.

## Objectives

* Deploy Wazuh as an EDR/SIEM monitoring platform.
* Integrate Windows Sysmon logs with Wazuh.
* Monitor important files and directories for unauthorized changes.
* Detect suspicious endpoint activity.
* Use malware hashes as Indicators of Compromise (IOCs).
* Integrate MalwareBazaar data for threat intelligence.
* Generate automated alerts for suspicious activity.
* Document and test the complete detection workflow.

## Architecture

Windows Endpoint
|
| Sysmon Logs
v
Wazuh Agent
|
v
Wazuh Manager
|
+---- File Integrity Monitoring
|
+---- Detection Rules
|
+---- Malware Hash IOC Database
|
v
Wazuh Dashboard
|
v
Security Alerts

MalwareBazaar
|
v
Threat Intelligence Feed
|
v
Malware Hash Database
|
v
Wazuh Detection Rules

## Main Components

### Wazuh

Wazuh is used to collect endpoint logs, monitor file integrity, analyze security events, and generate alerts.

### Sysmon

Sysmon provides detailed Windows endpoint telemetry such as process creation and other system activity. These events are forwarded to Wazuh for analysis.

### File Integrity Monitoring

Wazuh FIM monitors selected files and directories and detects:

* File creation
* File modification
* File deletion
* Permission changes
* Other file attribute changes

### MalwareBazaar

MalwareBazaar is used as a threat-intelligence source. Malware hashes can be collected and used as Indicators of Compromise (IOCs).

## Detection Workflow

1. Sysmon generates endpoint activity logs.
2. Wazuh Agent collects the logs.
3. Wazuh Manager analyzes the events.
4. FIM detects unauthorized file changes.
5. File hashes are compared against known malware indicators.
6. Detection rules generate alerts for suspicious activity.
7. Security personnel investigate the generated alerts.

## Testing

The system should be tested using safe test files and simulated events rather than executing real malware.

Example tests:

* Create a test file in a monitored directory.
* Modify the test file.
* Delete the test file.
* Generate a controlled Sysmon event.
* Check whether Wazuh generates the expected alert.
* Test a known malware hash using a harmless simulated IOC entry.

## Expected Result

The completed system should provide centralized endpoint monitoring and generate alerts when suspicious file or system activity is detected.

## Technologies

* Wazuh
* Sysmon
* Windows
* Python
* MalwareBazaar
* XML
* JSON
* Linux/Ubuntu

## Disclaimer

This project is intended for authorized cybersecurity training, defensive monitoring, and educational purposes. Real malware should not be executed on production systems.
