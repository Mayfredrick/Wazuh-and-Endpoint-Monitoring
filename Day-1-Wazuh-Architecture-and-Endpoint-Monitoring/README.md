#Day 1: Wazuh Architecture and Endpoint Monitoring

## Project Overview

The objective of this lab was to understand the architecture of Wazuh and the responsibilities of its main components. I reviewed how the Wazuh agent collects security data, how the Wazuh server analyzes events, how the indexer stores data, and how the dashboard helps analysts review security information.

This lab used an all-in-one Wazuh deployment on one virtual machine and a separate Windows 11 endpoint with the Wazuh agent installed.

## Lab Environment

- Security Platform: Wazuh
- Deployment Type: All-in-one
- Central Components: Wazuh server, Wazuh indexer, and Wazuh dashboard on one VM
- Endpoint: Windows 11 virtual machine
- Agent Service: WazuhSvc

## Wazuh Architecture and Components

### 1. Wazuh Agent

The Wazuh agent is installed on monitored endpoints, such as Windows computers and servers. It collects configured security data, including logs, and forwards events to the Wazuh server for analysis.

### 2. Wazuh Server

The Wazuh server receives data from agents, processes events, applies detection rules, and identifies potential security issues. It also manages enrolled agents.

### 3. Wazuh Indexer

The Wazuh indexer is a search and analytics engine that stores and indexes security alerts and other data. This allows analysts to search and investigate events.

### 4. Wazuh Dashboard

The Wazuh dashboard is a web interface used to visualize and analyze security information, review alerts, and manage monitored endpoints.

## Verification and Findings

I checked the Windows 11 Wazuh agent service using PowerShell:
-Get-Service WazuhSvc-
Result: The Wazuh agent service was running.

However, the Windows 11 agent was not listed in the Wazuh dashboard during this lab. Therefore, I could not confirm that the endpoint had successfully registered with or was communicating with the Wazuh server.
A running Windows service does not necessarily mean that an agent is enrolled and communicating successfully. Further troubleshooting is required to identify the cause.

## Lessons Learned

- The Wazuh agent collects security data from monitored endpoints.
- The Wazuh server analyzes events and applies detection rules.
- The Wazuh indexer stores and indexes data for searching and analysis.
- The Wazuh dashboard provides a central interface for reviewing security information and managing agents.
- An all-in-one deployment places the central Wazuh components on a single server.
- The Windows agent service can be running even when the endpoint is not visible in the dashboard.

## Technical Issues and Next Steps

The main issue encountered was that the Windows 11 agent did not appear in the Wazuh dashboard, even though its service was running.

The next step is to verify the agent configuration, confirm that it points to the correct Wazuh server IP address, review the agent logs, and check network connectivity and enrollment status. These checks should help me decide if I need to reinstall the agent.

## Conclusion

This lab improved my understanding of Wazuh architecture and the roles of its main components. I verified that the Windows 11 Wazuh agent service was running, but I identified an unresolved issue because the agent was not listed in the dashboard. This finding will be investigated in the next lab as part of learning how to deploy, register, and monitor Windows endpoints using Wazuh.
