# Day 13 — Wazuh Active Response Validation

## Objective
Review the Wazuh failed-login detection process and evaluate the evidence available for Active Response execution.

## Environment
- Wazuh manager: wazuh-server
- Windows endpoint: WinServ22
- Agent ID: 003
- Agent IP: 192.168.114.12

## Results
1. Windows Event ID 4625 recorded a failed logon.
2. Wazuh rule 60122 detected the event with the description "Logon Failure - Unknown user or bad password."
3. The configured response executable was netsh.exe.
4. The Wazuh execution component, wazuh-execd, was running.
5. The Windows agent's Active Response configuration was enabled.
6. The Windows Active Response log contained response-processing entries.
7. Successful firewall blocking was not confirmed.

## IP Address Analysis
The Windows event reported 127.0.0.1 as its source IP address. The Windows endpoint's network IP was 192.168.114.12. These addresses serve different purposes and should not be treated as interchangeable.

## Lessons Learned
Security alert detection, response execution, and successful remediation are separate stages. Reviewing the response logs is necessary, and additional evidence is required to confirm that a firewall action succeeded.

## Conclusion
The lab demonstrated Wazuh failed-login detection and provided evidence of Active Response processing. The investigation did not establish that a firewall block was successfully applied. The response outcome remains unverified.

