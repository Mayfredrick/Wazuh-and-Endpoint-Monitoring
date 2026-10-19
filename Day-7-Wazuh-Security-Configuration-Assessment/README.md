# Day 7: Wazuh Security Configuration Assessment

## Project Overview

The objective of this lab was to use Wazuh Security Configuration Assessment (SCA) to review the security configuration of a Windows Server 2022 endpoint.

I reviewed the CIS Microsoft Windows Server 2022 Benchmark, examined the SCA results, investigated a failed security check, verified the related Windows security setting, and decided whether to apply the recommended remediation.

## Lab Environment

- Wazuh Server: All-in-one deployment
- Windows Endpoint: WinServ22
- Agent ID: 003
- Operating System: Microsoft Windows Server 2022 Datacenter
- SCA Policy: CIS Microsoft Windows Server 2022 Benchmark V2.0.0

## 1. Agent Status

WinServ22 was active on the Wazuh command line during the lab.

This confirmed that the endpoint was available for security monitoring and that the SCA results could be reviewed for the system.

## 2. Security Configuration Assessment Results

The Wazuh SCA assessment used:

CIS Microsoft Windows Server 2022 Benchmark V2.0.0

The overall SCA score was:

26%

The assessment displayed:
- 96 checks passed
- 263 checks failed
- 0 checks not applicable

The results showed that there were several Windows Server security configuration settings that did not meet the selected benchmark.

## 3. Failed Security Check Investigated

I investigated the following failed check:

Ensure Enforce password history is set to 24 or more

Check ID: 2700

The check evaluates the password-history configuration.

The purpose of this security setting is to prevent users from repeatedly reusing previous passwords.

## 4. Security Risk

The security concern associated with the failed check is password reuse.

If password history is not configured according to the benchmark, a user may be able to reuse previous passwords more easily.

Password-history controls can help reduce the risk associated with repeatedly using the same passwords.

## 5. Verification

I verified the Windows security settings using the Windows Security Policy console: secpol.msc

The console displayed the password policies configured on Windows Server 2022.

This allowed me to compare the Windows configuration with the requirement identified by the Wazuh SCA assessment.

## 6. Recommended Remediation

Wazuh recommended configuring the password-history policy to retain:

24 or more previous passwords

The recommendation was related to the Windows password policy and could be configured through the appropriate Group Policy/Security Policy settings.

## 7. Remediation Decision

I decided to defer the recommended remediation.

The reason for deferring the change was that this is my lab environment and I am currently the only administrator using the computer. I decided not to change the password-history configuration during this lab.

The decision to defer does not mean that password history is unimportant. The benchmark identifies it as a security control, and the appropriate setting should be evaluated based on the organization's security requirements and environment.

## 8. SCA Score After Investigation

The SCA score remained: 26%

There was no change because I did not apply the recommended remediation.

## 9. Lessons Learned

This lab helped me understand how Wazuh SCA can compare a Windows Server configuration against a recognized security benchmark.

I learned how to:
- Review the overall SCA score.
- Identify passed and failed security checks.
- Investigate an individual benchmark requirement.
- Identify the security purpose of a failed configuration.
- Verify Windows security settings.
- Compare the actual system configuration with a benchmark requirement.
- Decide whether a recommended configuration change should be applied or deferred.

One important lesson was that a failed benchmark check does not automatically mean that the system has been compromised. It indicates that the current configuration does not meet the selected security benchmark requirement.

## 10. Technical Issues

I did not encounter any technical issues during this lab.

The Wazuh SCA assessment completed successfully, and I was able to verify the password-policy setting on Windows Server 2022.
## Conclusion

This lab successfully demonstrated the use of Wazuh Security Configuration Assessment to evaluate a Windows Server 2022 system against the CIS Microsoft Windows Server 2022 Benchmark V2.0.0.

WinServ22 received an SCA score of 26%, with 96 checks passed, 263 failed, and 0 not applicable.

I investigated Check ID 2700, which required password history to be set to 24 or more. I verified the Windows password-policy settings using secpol.msc and decided to defer the remediation.

This lab improved my understanding of security benchmarks, configuration weaknesses, and the importance of investigating a failed security control before making configuration changes.
