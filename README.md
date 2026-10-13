# 🛡️ Cyber Security Internship – Week 10
## SOC: Wazuh SIEM Setup & Alert Monitoring

### 📌 Project Overview

This project focuses on setting up **Wazuh SIEM** for security monitoring, collecting endpoint logs, detecting security events, and investigating alerts in a Security Operations Center (SOC) environment.

The project demonstrates how Wazuh can monitor a Windows endpoint, detect authentication failures, track file modifications, and display security alerts through a centralized dashboard. It also explores how Snort IDS alerts can be collected and analyzed alongside endpoint events.

### 🎯 Objectives

- Understand Wazuh SIEM and its main components.
- Install and configure Wazuh in a Linux virtual machine.
- Deploy a Wazuh Agent on a Windows endpoint.
- Generate and monitor safe security events.
- Investigate at least 10 security alerts.
- Study the Wazuh and Snort integration workflow.
- Perform a SOC incident investigation.
- Prepare a summary of security alerts and agent status.

### 🧰 Tools and Technologies

| Tool | Purpose |
|---|---|
| Wazuh 4.8.1 | Security monitoring and SIEM |
| Ubuntu 22.04.5 LTS | Wazuh server operating system |
| Windows 10 Pro | Monitored endpoint |
| Snort IDS | Network intrusion detection |
| Oracle VirtualBox | Virtual lab environment |
| Nmap | Authorized network scanning |

### 🖥️ Lab Environment

The lab was configured using VirtualBox with a host-only network.

| Machine | Role | IP Address |
|---|---|---|
| Wazuh Server | Manager, Indexer and Dashboard | `192.168.56.10` |
| Windows Agent | Monitored endpoint | `192.168.56.11` |
| Lab Host | Test traffic source | `192.168.56.1` |

**Network:** `192.168.56.0/24`

### ⚙️ Wazuh Installation

Wazuh was installed on Ubuntu using the official installation assistant.

```bash
curl -sO https://packages.wazuh.com/4.8/wazuh-install.sh
chmod +x wazuh-install.sh
sudo ./wazuh-install.sh -a
```

The `-a` option installs the all-in-one deployment components.

### ✅ Verify Wazuh Services

Run the following commands on the Wazuh server:

```bash
sudo systemctl status wazuh-manager
sudo systemctl status wazuh-dashboard
sudo systemctl status wazuh-indexer
```

Open the dashboard in a browser:

```text
https://192.168.56.10
```

Log in using the credentials generated during installation.

### 🖥️ Wazuh Agent Configuration

Wazuh Agent 4.8.1 was installed on the Windows 10 test endpoint and configured to communicate with the Wazuh Manager at `192.168.56.10`.

The agent appeared as **Active** in the Wazuh Dashboard.

The monitoring workflow is:

```text
Windows Endpoint
       ↓
   Wazuh Agent
       ↓
   Wazuh Manager
       ↓
   Wazuh Dashboard
       ↓
   SOC Analyst
```

### 🔍 Security Events Monitored

The following events were generated and investigated in the authorized lab environment:

- **Failed login attempts:** Windows authentication failures.
- **Successful logins:** Normal authentication activity.
- **File integrity monitoring:** Creation and modification of a test file.
- **Network activity:** Authorized Nmap scanning activity.
- **IDS alerts:** Snort-labelled network security events available in Wazuh.

### 🚨 Alert Investigation

Ten alerts were recorded and reviewed using their timestamps, agent details, source information, rule IDs, severity levels, and descriptions.

The investigation included:

- Windows authentication failure — Rule ID `60106`
- Windows logon success — Rule ID `60103`
- File modification — Rule ID `554`
- Nmap scripting-engine detection — Rule ID `2100498`

The high-interest event was the **ET SCAN Nmap Scripting Engine User-Agent Detected** alert. It was assessed as expected reconnaissance from the authorized lab host. The alert alone did not indicate a successful attack.

### 🛡️ Wazuh and Snort

The report documents a workflow for collecting Snort alerts and viewing them alongside endpoint events.

```text
Snort IDS
    ↓
Snort Alert Log
    ↓
Wazuh Agent Log Collection
    ↓
Wazuh Manager
    ↓
Wazuh Dashboard
    ↓
SOC Analyst Investigation
```

**Integration limitation:** The Snort alerts displayed in the report came from the endpoint-side Snort log. Integration with the separate Week 9 Kali Linux Snort sensor is documented as an architecture, but was not verified as a working integration.

### 📊 SOC Dashboard Summary

The final dashboard summary recorded the following figures for the 24-hour reporting period:

| Metric | Reported Value |
|---|---:|
| Total alerts | 156 |
| Critical alerts | 12 |
| High alerts | 34 |
| Medium alerts | 67 |
| Low alerts | 43 |
| Total events | 1,842 |
| Rules fired | 78 |
| Authentication events | 284 |
| File integrity events | 174 |
| Active monitored agent | 1 |

These figures are taken from the report's final dashboard summary.

### 🕵️ Incident Investigation

The selected incident involved Nmap scripting-engine detection against the Windows test endpoint.

**Investigation findings:**

- **Detection:** Rule ID `2100498`
- **Source:** `192.168.56.1`
- **Destination:** `192.168.56.11:80/TCP`
- **Severity:** Level 4 — Medium
- **Assessment:** Consistent with authorized lab scanning
- **Observed impact:** No exploitation or related malicious endpoint activity was identified in the reviewed evidence.

**Recommendations:**

1. Verify whether scanning activity is authorized.
2. Preserve relevant alerts and network evidence.
3. Review exposed services and restrict unnecessary access.
4. Monitor for repeated reconnaissance and follow-on activity.
5. Keep Wazuh rules and Snort detection signatures updated.

### 📸 Project Evidence

The report includes screenshots documenting:

- Wazuh installation completion.
- Wazuh Manager, Dashboard and Indexer service status.
- Wazuh Dashboard access.
- Ubuntu system information.
- Windows Agent connection status.
- Authentication and file integrity events.
- Network and IDS alerts.
- Alert investigation details.
- Final SOC dashboard summary.

### ⚠️ Scope and Limitations

All testing was described as taking place in an authorized virtual lab. The activities focused on detection and investigation rather than prevention or exploitation.

The separate Kali Linux Snort sensor integration remains a documented next step. The alert counts represent the report's recorded test environment and should not be interpreted as measurements of a production network.

### 🏁 Conclusion

This project demonstrates the basic setup and use of Wazuh SIEM for endpoint monitoring, event collection, alert investigation, and SOC reporting. It shows how authentication events, file modifications, and network alerts can be examined through a centralized dashboard.

The project also establishes a documented approach for integrating Snort IDS logs with Wazuh, providing a foundation for further work on centralized security monitoring and incident response.

---

**Internship:** DG Interns Hub  
**Module:** Cyber Security Internship – Week 10  
**Topic:** SOC Wazuh SIEM Setup & Alert Monitoring  
**Platform:** Wazuh 4.8.1  
**Environment:** VirtualBox-based security lab
