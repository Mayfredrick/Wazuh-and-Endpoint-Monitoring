# Day 12 — Wazuh Active Response Log Analysis

## Objective
Investigate Wazuh Active Response behavior and determine whether the response process was triggered by a Windows failed-login alert.

## Environment
- Wazuh manager: wazuh-server
- Windows endpoint: WinServ22
- Agent ID: 003
- Windows endpoint IP: 192.168.114.12
- Detection rule: 60122
- Windows Event ID: 4625

## Findings
I reviewed the Windows Wazuh agent's active-responses.log file. The log contained add, check_keys, continue, and delete command entries associated with failed-login alerts.

The entries showed that Wazuh's execution component initiated Active Response processing and that the response entered its timeout/removal phase approximately 60 seconds later.

The alert reported 127.0.0.1 as the source IP address. This is the loopback address and differs from the endpoint's network IP address, 192.168.114.12.

## Lessons Learned
- Detecting a security alert and executing an Active Response are separate stages.
- Agent-side logs can provide evidence that is not visible in the manager's Active Response log.
- An Active Response execution record does not, by itself, prove that a firewall rule successfully blocked traffic.
- The source IP reported by a Windows event must be evaluated before selecting an automated blocking action.

## Conclusion
The investigation confirmed that the Active Response process was triggered and that its timeout/removal phase was logged. Successful firewall blocking remains unverified. Further validation should use a suitable test event with a genuine remote source address before claiming that IP blocking works.
