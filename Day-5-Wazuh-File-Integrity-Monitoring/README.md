# Day 5: Wazuh File Integrity Monitoring

## Project Overview

The objective of this lab was to investigate Wazuh File Integrity Monitoring (FIM) on a Windows Server 2022 endpoint. I reviewed the existing Syscheck configuration, created a controlled test file, modified the file, and deleted it to determine whether Wazuh could detect the changes.

The monitored endpoint was WinServ22.

## Lab Environment

- Wazuh Server: All-in-one deployment
- Wazuh Server IP: 192.168.114.19
- Windows Endpoint: Windows Server 2022
- Wazuh Agent: WinServ22
- Agent ID: 003
- FIM Module: Syscheck

## 1. FIM Configuration

Before performing the test, I reviewed the existing Wazuh agent configuration.

The configuration contained several monitored Windows directories, including:

- %WINDIR%
- %WINDIR%\System32
- %WINDIR%\System32\drivers\etc
- %WINDIR%\System32\wbem
- %WINDIR%\System32\WindowsPowerShell\v1.0
- %PROGRAMDATA%\Microsoft\Windows\Start Menu\Programs\Startup

The Startup directory was configured with:

xml
<directories realtime="yes">%PROGRAMDATA%\Microsoft\Windows\Start Menu\Programs\Startup</directories>


This allowed me to perform the FIM test without changing the existing configuration.

## 2. Controlled FIM Test

I created a harmless test text file in the monitored Startup directory.

The purpose was to determine whether Wazuh could detect a file being added to the system.

I then modified the file and later deleted it.

The three activities tested were:

1. File creation
2. File modification
3. File deletion

## 3. FIM Results

Wazuh successfully detected all three activities.

| Activity | Result |

| File creation | Detected |
| File modification | Detected |
| File deletion | Detected |

The file creation event displayed:

Rule ID: 554

Severity: Low

Alert description: File added to the system

This demonstrated that Wazuh was able to monitor changes to the configured directory and generate an alert when a new file was introduced.

## 4. Security Analysis

The file activity was expected because I intentionally created, modified, and deleted the test file.

Therefore, I did not consider the activity to be a security incident.

However, file integrity alerts can be important in a real SOC environment. An unexpected file being added to a monitored system directory could require investigation, especially if the file were an executable, script, or other potentially sensitive file.

The important lesson is that the SOC analyst must consider the context surrounding an alert before determining whether it represents malicious activity.

## 5. Investigation Process

The investigation followed a controlled process:

1. Confirmed that the Wazuh agent was available.
2. Reviewed the existing Syscheck configuration.
3. Identified a directory already configured for monitoring.
4. Created a harmless test file.
5. Confirmed that Wazuh detected the file creation.
6. Modified the test file.
7. Confirmed that Wazuh detected the modification.
8. Deleted the test file.
9. Confirmed that Wazuh detected the deletion.
10. Reviewed the alert information provided by Wazuh.

No changes to the Wazuh configuration were required for the test.

## Lessons Learned

This lab helped me understand how Wazuh File Integrity Monitoring can detect changes to files on a Windows endpoint.

I learned that FIM can identify different types of file activity, including:

- New files being added
- Existing files being modified
- Files being deleted

I also learned that an FIM alert does not automatically mean that malicious activity has occurred. The analyst needs to understand whether the change was expected or unexpected and investigate the surrounding context.

The controlled test also demonstrated the importance of having monitored directories configured before a file-change investigation begins.

## Technical Issues

I did not experience any technical difficulties during this lab.

The FIM tests completed successfully, and Wazuh detected the file creation, modification, and deletion activities.

## Conclusion

The Week 9 Day 5 lab successfully demonstrated Wazuh File Integrity Monitoring on the Windows Server 2022 WinServ22 endpoint.

I confirmed that Wazuh detected the creation, modification, and deletion of a controlled test file. The file creation event was associated with Rule ID 554, had Low severity, and displayed the description “File added to the system.”

This lab improved my understanding of how FIM can help SOC analysts identify changes to monitored endpoints and investigate whether those changes are expected or potentially suspicious.
