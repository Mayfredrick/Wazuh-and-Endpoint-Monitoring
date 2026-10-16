# Day 4: Wazuh Windows Security Event Monitoring

## Project Overview

The objective of this lab was to use Wazuh to monitor Windows Server 2022 security events and compare the information collected by Wazuh with the original Windows Event Viewer data.

I generated a controlled failed-login event and investigated how Wazuh detected, displayed, and analyzed the event.

## Lab Environment

- Wazuh Server: All-in-one deployment
- Wazuh Server IP: 192.168.114.19
- Windows Endpoint: Windows Server 2022
- Wazuh Agent: WinServ22
- Operating System: Microsoft Windows Server 2022 Datacenter

## 1. Agent Status

Before beginning the investigation, I confirmed that the WinServ22 agent was active in Wazuh.

Wazuh displayed the operating system as:

Microsoft Windows Server 2022 Datacenter

This confirmed that the Windows Server 2022 endpoint was connected to the Wazuh environment and available for monitoring.


## 2. Generating a Controlled Security Event

I intentionally created three failed login attempts on the Windows Server 2022 system.

The purpose was to generate a controlled security event that could be observed in both Windows Event Viewer and Wazuh.

The Windows Security log recorded the failed authentication as:

Event ID: 4625

Microsoft documents Event ID 4625 as an event generated when an account fails to log on.

## 3. Windows Event Viewer Findings

In Windows Event Viewer, the failed authentication involved the:

Administrator

account.

The event showed:

Logon Type: 2

Microsoft identifies Logon Type 2 as an interactive logon, meaning a user attempted to log on directly to the computer.

The Windows Event Viewer displayed the source network address as:

127.0.0.1

This is the loopback/localhost address.

## 4. Wazuh Findings

The same failed-login activity appeared in Wazuh.

The Wazuh event displayed:

- Event ID: 4625
- Source IP: 192.168.114.12
- Logon Type: 2
- Wazuh Rule ID: 60122

Wazuh's documentation uses rule 60122 as one of the Windows rules relevant to detecting repeated failed logons.

The Wazuh event therefore provided security monitoring information about the failed authentication.


## 5. Comparing Windows Event Viewer and Wazuh

I compared the information displayed by Windows Event Viewer with the information displayed by Wazuh.

Some information matched between the two sources, while some information was presented differently.

| Information | Windows Event Viewer| Wazuh |

| Event ID    | 4625              | 4625 |
| Account     | Administrator     | Related to the failed-login event |
| Logon Type | 2                  | 2 |
| Source Address | 127.0.0.1      | 192.168.114.12 |
| Wazuh Rule | N/A                  | 60122 |

The difference in source IP addresses was an important observation. I did not treat the difference as proof of malicious activity because the available evidence did not establish why Wazuh displayed 192.168.114.12 while Windows Event Viewer displayed 127.0.0.1.

## 6. Security Analysis

The event represented a failed authentication attempt.

I generated three failed login attempts intentionally as part of the lab. Because the activity was controlled and limited to three attempts, I did not identify anything suspicious during this investigation.

However, a failed-login event should not automatically be considered benign or malicious based only on the Event ID. Additional context such as the number of attempts, account involved, source system, timing, and failure details should be considered during a real SOC investigation.

Microsoft notes that repeated failed-logon events can warrant additional investigation depending on the circumstances.

## 7. Wazuh Rule Investigation

The Wazuh rule associated with the event was:

Rule ID: 60122

This demonstrated how Wazuh can apply detection rules to Windows security events and make potentially important authentication activity easier for a SOC analyst to identify.

The alert itself was not treated as proof of a confirmed security incident. Further investigation would be required before determining that the activity represented malicious behavior.

## 8. Resource Management Issue

During the lab, I experienced a RAM limitation on the Wazuh environment.

The reduced available memory made it more difficult to analyze some of the information.

To reduce resource usage, I shut down the Windows Server 2022 virtual machine after generating the required security events.

This allowed me to conserve memory while continuing the investigation.

## Lessons Learned

This lab helped me understand how Wazuh can collect and display Windows security events.

I learned how to:

- Generate a controlled Windows security event.
- Identify Event ID 4625 as a failed logon.
- Compare Windows Event Viewer information with Wazuh data.
- Identify the affected account.
- Review the logon type.
- Identify a Wazuh detection rule.
- Investigate differences between the original Windows event and the information presented by Wazuh.
- Avoid treating an alert as proof of a confirmed security incident without additional investigation.

I also learned that resource availability can affect the ability to work with security monitoring tools in a virtualized home lab.

## Technical Issues

The main technical issue encountered was limited RAM available to the Wazuh environment.

To manage the available resources, I shut down the Windows Server 2022 VM after generating the required events.

I did not encounter any other technical problems during the lab.

## Conclusion

The lab successfully demonstrated Wazuh's ability to monitor Windows authentication activity.

I generated three controlled failed login attempts and observed Event ID 4625 in Windows Event Viewer and Wazuh. The event involved the Administrator account and used Logon Type 2. Wazuh identified the event with rule ID 60122.

The investigation also showed that information displayed by Wazuh may not always appear exactly the same as the original Windows Event Viewer information. This reinforced the importance of comparing multiple sources and understanding the context of an alert before deciding whether an event is malicious.

Overall, this lab improved my ability to investigate Windows authentication events using Wazuh from a SOC analyst perspective.
