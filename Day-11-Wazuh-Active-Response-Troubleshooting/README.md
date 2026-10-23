# Day 11: Wazuh Active Response Troubleshooting

## Objective

The objective of this lab was to investigate why the Wazuh Active Response did not appear to execute after a Windows failed-login event was detected.

## Lab Environment
- Wazuh Manager: 192.168.114.19
- Windows Server: WinServ22
- Agent ID: 003
- Windows Server Version: Microsoft Windows Server 2022 Datacenter
- Active Response: netsh
- Trigger Rule: 60122

## Lab Activities

I first verified that the Wazuh manager was running and that the wazuh-execd process was active. The Windows Server 2022 agent, WinServ22, was also connected to the Wazuh manager.

I reviewed the Active Response configuration and confirmed that the netsh command was defined and that the Active Response configuration was enabled for rule 60122.

The configuration used was
xml
<active-response>
  <command>netsh</command>
  <location>local</location>
  <rules_id>60122</rules_id>
  <timeout>60</timeout>
</active-response>

I then reviewed a Wazuh alert generated from a Windows failed-login event.

## Alert Analysis
The event was Windows Security Event ID 4625 and generated Wazuh rule 60122:
Logon Failure - Unknown user or bad password

The alert contained the following information:
- Agent: WinServ22
- Agent ID: 003
- Windows Event ID: 4625
- Wazuh Rule ID: 60122
- Rule Level: 5
- Logon Type: 2
- Target User: John.admin
- Source IP reported by Windows: 127.0.0.1
- Agent IP: 192.168.114.12

The event showed that the failed login originated from the local system using the loopback address 127.0.0.1.

## Results
Wazuh successfully detected the failed-login event and generated rule 60122.

The Wazuh manager and wazuh-execd were running normally. However, I could not confirm that the Active Response executed.

The Active Response log remained empty, and no Wazuh firewall rule was found on WinServ22 after the test.

## Lessons Learned
This lab helped me understand that detecting an event and executing an automated response are two separate processes.

I also learned that the IP address shown in an alert needs to be interpreted carefully. The WinServ22 agent has the IP address 192.168.114.12, while the Windows failed-login event reported 127.0.0.1 as the source address. These addresses represent different information from the event.

I also learned that an Active Response configuration can be present and the execution service can be running while the actual response still requires additional verification.

## Conclusion

The Wazuh detection portion of the lab was successful. I was able to trace a Windows failed-login event from Event ID 4625 to Wazuh rule 60122 and examine the information provided by the alert.

The Active Response execution was not confirmed during this lab. I will continue investigating this in a future lab rather than assuming that the response executed successfully.
