# Day 10: Wazuh Active Response Verification

## Objective

The objective of this lab was to troubleshoot and verify the Wazuh Active Response configuration after the response was not confirmed during the previous lab.

## Lab Activities

I reviewed the Active Response configuration on the Wazuh manager and found that the configuration was commented out and contained a spelling error in the command tag. I corrected the configuration so that the `netsh` response was associated with Wazuh rule 60122.

I restarted the Wazuh manager and confirmed that the manager remained active. I then performed a controlled failed-login test on WinServ22.

## Results

Wazuh detected the failed login and generated rule 60122, which represents a Windows logon failure caused by an unknown user or incorrect password.

However, the Active Response log remained empty, and no Wazuh firewall rule appeared on WinServ22. Therefore, I could not confirm that the Active Response actually executed.

## Lessons Learned

This lab helped me understand that detecting an event and responding to an event are separate processes. Wazuh successfully detected the failed login, but the response could not be verified.

I also learned the importance of checking the configuration carefully. A configuration can appear to contain an Active Response command but still be disabled if it is inside a comment.

## Conclusion

The detection portion of the lab was successful. The Active Response execution remains unresolved and should be investigated further before using automatic response actions in a production environment.
