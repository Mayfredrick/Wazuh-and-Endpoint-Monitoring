# Day 2: Wazuh Agent Enrollment and Connectivity

## Project Overview

The objective of this lab was to troubleshoot Wazuh agent connectivity and determine whether the Windows endpoints were communicating with the Wazuh server. I verified the Wazuh server address, checked the Windows agent configuration, tested network connectivity, reviewed agent status, and examined the Wazuh agent log.

This lab built on Day 1, where the Windows 11 Wazuh service was running but the agent was not visible in the dashboard.

## Lab Environment

- Wazuh Server IP: 192.168.114.19
- Deployment: All-in-one Wazuh VM
- Windows Endpoint: Windows 11
- Additional Endpoint: Windows Server 2022
- Agent Communication Port: 1514/TCP
- Agent Enrollment Port: 1515/TCP

Wazuh uses TCP port 1514 by default for agent communication and TCP port 1515 for enrollment through the agent configuration method.

## 1. Verified Wazuh Server Address

The Wazuh server was using:

192.168.114.19

I checked the Windows agent configuration and confirmed that the Wazuh server address listed in ossec.conf was also:

192.168.114.19

This showed that the Windows agent was configured to communicate with the expected Wazuh server address.

## 2. Tested Network Connectivity

I tested connectivity from the Windows endpoint to the Wazuh server.

| Test |                  Result |
| 192.168.114.19:1514/TCP | True |
| 192.168.114.19:1515/TCP | True |

The successful tests showed that the Windows endpoint was able to establish TCP connectivity to the Wazuh server on the tested ports at the time of testing.

Wazuh requires outbound connectivity from the agent to the manager for agent communication.

## 3. Checked Agent Status

I used the Wazuh agent_control -l command on the Wazuh server to review the registered agents.

### Windows 11

The Windows 11 agent was shown as: Active

According to Wazuh documentation, an Active agent has successfully connected to the Wazuh manager.

### Windows Server 2022

The Windows Server 2022 agent was shown as: Never connected

Wazuh uses this status when an enrolled agent has not successfully connected to the manager.

This demonstrated that the two Windows endpoints had different connection states.

## 4. Reviewed the Windows Agent Log

The Windows agent log contained the following connection attempts:

text
2026/10/15 02:08:12 wazuh-agent: INFO: Trying to connect to server ([192.168.114.19]:1514/tcp).

2026/10/15 02:08:33 wazuh-agent: ERROR: (1216): Unable to connect to '[192.168.114.19]:1514/tcp': 'A connection attempt failed because the connected party did not properly respond after a period of time, or established connection failed because connected host has failed to respond.'.

2026/10/15 02:08:43 wazuh-agent: INFO: Trying to connect to server ([192.168.114.19]:1514/tcp).

2026/10/15 02:08:51 wazuh-agent: ERROR: (1216): Unable to connect to '[192.168.114.19]:1514/tcp': 'No connection could be made because the target machine actively refused it.'.

2026/10/15 02:09:01 wazuh-agent: INFO: Trying to connect to server ([192.168.114.19]:1514/tcp).

2026/10/15 02:09:03 wazuh-agent: ERROR: (1216): Unable to connect to '[192.168.114.19]:1514/tcp': 'No connection could be made because the target machine actively refused it.'.


These messages showed that the agent attempted to connect to the Wazuh server over TCP port 1514 but encountered connection failures at the times recorded.

The log evidence is important because it shows that the agent's actual connection attempts were not consistently successful, even though the later TCP connectivity test returned True.

## 5. Indexer Observation

I also observed that the indexer connector successfully reported inventory services for the Wazuh server.

This provided evidence that the Wazuh indexer-related inventory information was functioning during the lab.

## 6. Memory Observation

During the lab, I noticed that the Wazuh environment could become disconnected when the virtual machine was under low-memory conditions.

To reduce memory usage, I closed virtual machines that were not being used.

Low memory was an observation from the lab environment, but I cannot confirm from these results alone that memory pressure was the direct cause of the Wazuh connection errors.

## Findings

The main findings from this lab were:

1. The Wazuh server IP was 192.168.114.19.
2. The Windows agent was configured with the correct Wazuh server IP.
3. TCP connectivity tests to ports 1514 and 1515 returned True.
4. The Windows 11 agent was listed as Active.
5. The Windows Server 2022 agent was listed as Never connected.
6. The Windows agent log showed failed connection attempts to TCP port 1514.
7. The indexer connector successfully reported inventory services for the Wazuh server.
8. Memory usage was an important consideration in the lab environment.

## Lessons Learned

This lab taught me that checking whether the Wazuh service is running is only one part of troubleshooting an endpoint.

I learned that an agent can have its Windows service running while still experiencing communication problems. Checking the server address, testing network connectivity, reviewing the agent status on the Wazuh manager, and examining the agent logs provides a better understanding of the actual connection state.

I also learned that Wazuh provides different agent states, including Active and Never connected, which help administrators determine whether an endpoint has successfully communicated with the manager.

## Conclusion

The lab successfully demonstrated how to troubleshoot Wazuh agent connectivity without immediately reinstalling or changing the configuration.

The Windows 11 agent was ultimately shown as Active, while the Windows Server 2022 agent remained Never connected. The Windows agent logs also showed connection failures to the Wazuh server on port 1514 at specific times.

The investigation reinforced the importance of using multiple sources of evidence, including agent status, configuration, network connectivity tests, and logs, before making changes to a security monitoring environment.
