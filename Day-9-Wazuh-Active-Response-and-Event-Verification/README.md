# Day 9: Wazuh Active Response and Event Verification

## Objective
Test Windows authentication monitoring in Wazuh and investigate whether Active Response execution could be verified.

## Lab Activities
- Reviewed Windows successful logon events (Rule ID 60106).
- Reviewed Windows user logoff events (Rule ID 60137).
- Performed five incorrect login attempts on the Windows Server 2022 endpoint, WinServ22.
- Confirmed that Wazuh generated a failed-login alert (Rule ID 60122).
- Checked the Wazuh manager log and Active Response log for execution evidence.

## Results
Wazuh successfully detected the failed login activity. However, the logs checked during this session did not provide evidence confirming that Active Response executed.

## Lessons Learned
Detecting a security event and executing an automated response are two separate processes. Security monitoring requires verification of both detection and response rather than assuming that an alert automatically triggers an action.

## Conclusion
The event detection portion of the lab was successful. Active Response execution remains unverified and will require further investigation.
