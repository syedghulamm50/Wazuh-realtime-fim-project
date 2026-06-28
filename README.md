# Building a Real-Time File Integrity Monitoring (FIM) Lab with Wazuh

This project captures my hands-on setup of a local Endpoint Detection and Response (EDR) lab environment using Wazuh. The main goal was to configure and test real-time File Integrity Monitoring (FIM) on a Linux client (Kali Linux) to catch unauthorized system changes, file modifications, and unexpected downloads the exact second they happen.

## Why Build This?
Monitoring critical system paths and sensitive corporate directories is one of the quickest ways to spot an early-stage intrusion. By setting up this lab, I wanted to get practical experience with:
1. Deploying and registering EDR agents with a centralized manager.
2. Tuning agent configuration files (`ossec.conf`) to watch specific high-risk paths.
3. Simulating typical attacker behavior (tampering with configuration data and pulling down files via command-line utilities) to make sure the alerts actually fire.

---

## Setting Up the Infrastructure

### Phase 1: Deploying and Registering the Agent
I started by utilizing the deployment wizard within the Wazuh dashboard to generate a clean installation package for my endpoints. The interface lets you select the target OS architecture and point the agent back to the Wazuh Manager's private IP address.

![Generating the Agent Deployment Script](images/agentdeployment.png)
*Figure 1: Walking through the agent setup wizard in the central console.*

Once the installation commands were executed on the endpoint machines, they checked back into the environment and established a stable connection over port 1514 (TCP). I verified that both my Windows and Linux target endpoints were showing up as active on the central dashboard.

![Active Agents Dashboard Verification](images/agents.png)
*Figure 2: Verifying that endpoints are successfully checking in and active.*

---

## Customizing the FIM Configuration

By default, Wazuh tracks a lot of standard directories (like `/etc` or `/bin`), but I wanted to implement targeted monitoring over a simulated corporate folder structure. I added a custom rule to the agent's `/var/ossec/etc/ossec.conf` file to watch a specific `Downloads` path and a highly critical file named `Sensitive_Information`.

Here are the exact configurations I appended to the `<syscheck>` block:

```xml
<syscheck>
   <directories realtime="yes">/home/kali/Downloads</directories>
   <directories realtime="yes" report_changes="yes">/home/kali/Downloads/Sensitive_Information</directories>
</syscheck># Wazuh-realtime-fim-project

Implementing real-time File Integrity Monitoring (FIM) using a Wazuh  agent on Kali Linux to track unauthorized file modifications and directory creations.
