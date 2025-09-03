# SOC-Detection-Lab – SIEM Implementation

## Objective
The goal of this project was to build and configure a working Security Information and Event Management (SIEM) solution using **Splunk Enterprise** and **Splunk Universal Forwarder** in a home lab environment.  
The project focused on ingesting Windows Event Logs, validating log flow, and creating alerts/dashboards for real-time detection.

## Skills Learned
- Configured log ingestion pipelines for multiple sources
- Built correlation rules and custom alerts
- Analyzed logs to detect anomalous behaviors
- How to configure and validate a Universal Forwarder in a SIEM lab
- How to troubleshoot connection issues (firewall, ports, services)
- Gained experience writing SPL queries for log analysis and alerting
- Learned the importance of sanitizing sensitive details before publishing results
- Developed critical thinking for real-world security incidents

## Tools Used
| **Splunk Enterprise** | Central SIEM platform for log analysis |
| **Splunk Universal Forwarder** | Agent for collecting and forwarding logs |
| **Windows Event Logs** | Data source (Security, System, Application) |
| **PowerShell / Admin CMD** | Used for configuration and troubleshooting |

## Steps / Screenshots
**Ref 1:** SIEM Architecture Overview
This screenshot shows the architecture setup, including the Universal Forwarder sending logs over TCP port 9997 to Splunk Enterprise.

**Sanitization Note**: IP addresses and hostnames have been replaced with generic placeholders.

**Screenshot: Architecture Overview**
![Architecture Overview](https://github.com/antwoinecollins/SOC-Detection-Lab-SIEM-Implementation/blob/main/Splunk_System_Log_Volume_by_Source.jpg?raw=true)*

**Ref 2:** Log Ingestion Confirmation
**SPL Query:**
```spl
index=win_log | stats count by sourcetype
````

This proves logs are being received from the forwarder into Splunk.
**Sanitization Note**: Hostnames/IPs were anonymized.

**Screenshot: Log Ingestion Confirmation**  
![Log Ingestion Confirmation](https://github.com/antwoinecollins/SOC-Detection-Lab-SIEM-Implementation/blob/main/Log_Ingestion_Confirmation.jpg?raw=true)*


**Ref 3:** Windows Event Log Forwarding

This screenshot shows successful ingestion of **Security**, **System**, and **Application** logs into the `win_log` index.

**Screenshot: Windows Event Logs**
![Windows Event Logs](https://github.com/antwoinecollins/SOC-Detection-Lab-SIEM-Implementation/blob/main/Errors_Warnings_Breakdown_by_Source.jpg?raw=true)*

**Ref 4:** Alert Configuration & Testing

An alert was created for **failed login attempts** (EventCode 4625). This simulates detection of brute-force or unauthorized access attempts.

**SPL Query:**

```spl
index=win_log sourcetype=WinEventLog:Security EventCode=4625
| stats count by Account_Name, host
| where count > 3
```

**Screenshot: `Failed Login Alert**
![Failed Login Alert](https://github.com/antwoinecollins/SOC-Detection-Lab-SIEM-Implementation/blob/main/Splunk_Errors_In_Last_Hour.jpg?raw=true)*

## ⚙️ Configuration Summary

### Universal Forwarder (outputs.conf)

```conf
[tcpout]
defaultGroup = default-autolb-group

[tcpout:default-autolb-group]
server = <RedactedIP>:9997

[tcpout-server://<RedactedIP>:9997]
```

### Splunk Enterprise (inputs.conf)

```conf
[splunktcp://9997]
connection_host = ip
```

👉 **Sanitization Note**: IP addresses have been redacted.

## 📦 Future Improvements

* Add Sysmon for deeper endpoint telemetry
* Expand to Linux log ingestion
* Build dashboards aligned with MITRE ATT\&CK detections
* Automate alerts for brute-force and privilege escalation scenarios

---

## 🤝 Connect

If you’re a recruiter, engineer, or mentor interested in SIEM, detection engineering, or blue team practices, feel free to connect:

* 🔗 [LinkedIn Profile](#)
* 💻 [GitHub Portfolio](#)

---

*Note: All sensitive details (IP addresses, hostnames, usernames) have been sanitized for confidentiality.*
