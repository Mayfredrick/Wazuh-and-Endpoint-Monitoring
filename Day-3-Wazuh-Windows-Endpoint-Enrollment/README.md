# Day 3: Wazuh Windows Server Agent Connectivity

## Project Overview

The objective of this lab was to investigate the connectivity status of a Windows Server 2022 Wazuh agent. I checked the agent registration, Windows service, agent connection state, network connectivity, and Wazuh logs.

This lab followed the previous investigation where a Windows Server 2022 agent was shown as Never connected. During this lab, I discovered that the Never connected entry was an older Windows Server 2022 agent that had already been deleted before the lab began.



## Lab Environment

- **Wazuh Server IP:** 192.168.114.19
- **Deployment:** All-in-one Wazuh server
- **Windows Endpoint:** Windows Server 2022
- **Agent Name:** WinServ22
- **Agent ID:** 003
- **Agent IP shown by Wazuh:** any
- **Agent Communication Port:** 1514/TCP
- **Enrollment Port:** 1515/TCP

## 1. Agent Registration

I checked the Wazuh manager for registered agents and identified the Windows Server 2022 agent:

| Field |      Result     |
| Agent ID    | 003       |
| Agent Name  | WinServ22 |
| Agent IP    | any       |
| Current connection state | Connected |

There were two Windows Server 2022 entries visible during the investigation. The entry showing Never connected was an older agent that had been deleted before the beginning of this lab.

Therefore, the Never connected status did not represent the current WinServ22 agent.

## 2. Windows Wazuh Service

I checked the Wazuh service on Windows Server 2022 using: powershell Get-Service WazuhSvc

The service status was:

**Running**

This confirmed that the Wazuh agent service was running on the Windows Server 2022 endpoint.

## 3. Agent Connection State

I checked the Wazuh agent state file and received: status='connected'

This provided additional evidence that WinServ22 was communicating with the Wazuh server.

The agent's connection state was consistent with the current agent being active.

## 4. Network Connectivity Tests

I tested connectivity from Windows Server 2022 to the Wazuh server.

| Test     | Result |
| TCP 1514 | True |
| TCP 1515 | True |

The successful TCP tests showed that the Windows Server 2022 endpoint could reach the Wazuh server on both tested ports at the time of testing.

## 5. Windows Agent Log Investigation

I reviewed the Wazuh agent log and found the following messages:
2026/10/15 14:45:25 wazuh-agent: WARNING: Server unavailable. Setting lock.
2026/10/15 14:45:26 wazuh-agent: INFO: Closing connection to server ([192.168.114.19]:1514/tcp).
2026/10/15 14:45:26 wazuh-agent: INFO: Trying to connect to server ([192.168.114.19]:1514/tcp).
2026/10/15 14:45:26 wazuh-agent: INFO: (4102): Connected to the server ([192.168.114.19]:1514/tcp).
2026/10/15 14:45:26 wazuh-agent: INFO: Server responded. Releasing lock.


The log showed a temporary interruption in communication with the Wazuh server. The agent then immediately attempted to reconnect and successfully connected to the server.

The final messages showed:

- The server connection was established.
- The server responded.
- The agent released its connection lock.

Based on these messages, the issue was temporary rather than a persistent connection failure.

## 6. Wazuh Manager Log

I reviewed the Wazuh manager ossec.log.

The manager showed that the agent was connected, and the messages I observed were informational. I did not observe errors or warnings in the manager log during this investigation.

This supported the finding that the current WinServ22 agent was successfully communicating with the Wazuh server.

## Findings

The investigation produced the following results:

1. The Windows Server 2022 agent is named WinServ22.
2. The agent ID is 003.
3. The Windows Wazuh service was running.
4. The agent state showed connected.
5. TCP connectivity to port 1514 was successful.
6. TCP connectivity to port 1515 was successful.
7. The Windows agent log showed a brief server-unavailable condition.
8. The agent successfully reconnected immediately afterward.
9. The Wazuh manager log showed the agent as connected without observed errors or warnings.
10. The previously observed Never connected Windows Server 2022 entry was an older agent that had already been deleted.

## Lessons Learned

This lab taught me that an agent status of Never connected needs to be investigated in context. In this case, the entry was associated with an older Windows Server 2022 agent that had already been deleted.

I also learned that the Windows Wazuh service being Running is not the only indicator of successful communication. The agent state, network connectivity tests, and Wazuh logs provide additional evidence.

The agent log also demonstrated that a temporary connection problem does not necessarily mean that the agent is permanently disconnected. In this case, the agent detected that the server was unavailable, attempted to reconnect, and successfully established the connection.

## Technical Issues

I did not encounter any technical issues during this lab.

The only notable observation was the temporary connection interruption shown in the Windows Wazuh agent log. The agent recovered successfully without requiring a configuration change.

## Conclusion

The Windows Server 2022 WinServ22 agent was successfully verified as connected to the Wazuh server.

The investigation confirmed that the Wazuh service was running, the agent state was connected, TCP connectivity to ports 1514 and 1515 was successful, and the manager recognized the agent.

The temporary connection interruption was followed by a successful reconnection, demonstrating the importance of reviewing logs and current agent status before assuming that an endpoint has a persistent connectivity problem.
