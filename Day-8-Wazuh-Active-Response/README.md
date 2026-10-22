# Day 8: Wazuh Active Response

## Objective
Explore Wazuh Active Response and review how the system can execute actions in response to detected security events.

## Work Completed
- Verified that the Wazuh manager and Windows Server 2022 agent were active.
- Located the available Active Response executables on the Windows endpoint.
- Reviewed the Active Response command definitions and configuration on the Wazuh manager.
- Enabled the Active Response configuration block and restarted the manager.

## Results
The Wazuh manager restarted successfully, and the Windows Server 2022 agent remained active. The Active Response log file was empty, so successful response execution was not confirmed during this session.

## Lessons Learned
Active Response requires more than having the necessary executables installed. The configuration must be correct, and an actual execution must be verified before the response can be considered tested.



