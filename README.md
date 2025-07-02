
This project simulated the behavior of a malicious insider on a Windows 11 host to generate detectable telemetry for use in SIEM-based detection and host triage. The exercise focused on five core behaviors that commonly indicate internal compromise or abuse:

Repeated failed logon attempts

Successful local logon

Persistence via scheduled task

Lateral movement attempts (RDP and SMB)

Privilege escalation behavior (vssadmin.exe)

Splunk successfully ingested Windows Security logs and captured key events across all five stages. Detection was confirmed through filtered Event ID 4688 entries and command line visibility. Wireshark was used in parallel to validate outbound connection attempts for lateral movement.

**Key Takeaways**

Telemetry Validation Is Non-Trivial: Significant time was spent enabling and validating audit policies, user permissions, and Splunk configuration to ensure full log visibility. These challenges reflect real-world deployment and monitoring issues.

Behavior-Based Detection Works: Even simple attacker simulations like launching mstsc.exe, net use, and vssadmin.exe triggered clear, filterable events when visibility was properly configured.

Repetition Builds Confidence: By recreating common attacker behaviors in a controlled environment and matching them to specific log entries, we built fluency in host telemetry interpretation — a core SOC skill.

**Next Steps**
Now that the environment is configured and core detection patterns are understood, a follow-up project will repeat this methodology with slight variations in behavior and process execution. The goal is to reinforce detection workflows while minimizing time spent on reconfiguration.

**System Configuration Status & Lessons Learned**

This project required extensive configuration work at the Windows OS and Splunk levels to enable full visibility into host activity. These changes were essential to support telemetry collection and SIEM-based analysis but introduced complexity worth documenting.

✅ Key Configuration Steps Completed
Audit Policy Changes

Enabled Audit Process Creation with command line logging (AuditProcessCreation) set to "Success and Failure"

Verified logs were being written by confirming Event ID 4688 entries with populated command lines

Splunk Permissions and Access

Granted the Splunk service account (“.\Steve”) the Log on as a service right via secpol.msc

Resolved permissions errors blocking access to the Security log by reconfiguring the Splunk input and restarting the service

Task Scheduler Validation

Created a scheduled task (Updater) to simulate persistence

Verified task creation via Event ID 4698, though visibility was inconsistent in Splunk despite successful execution

Command Prompt and PowerShell Use

Repeated use of administrator-level CMD and PowerShell was required for:

Task creation and inspection

Real-time testing of attacker behaviors

Updating audit policies and refreshing Group Policy with gpupdate /force

**Configuration Challenges Faced** 

Splunk License Limitation

Encountered license expiration message due to event volume during testing

Required restart and workaround to resume searching logs

Windows File Explorer Confusion

Difficulty accessing the C:\Windows\System32\Tasks directory via GUI due to File Explorer interface quirks and address bar issues

Event Visibility Gaps

Despite proper scheduled task creation and visible execution in Task Scheduler, corresponding 4698 logs were not always ingested by Splunk, possibly due to indexing delays or internal caching

Manual Restarts and Delayed Effects

Some system behaviors (e.g., scheduled task launch) did not trigger consistently after logoff or reboot, highlighting the unreliability of some persistence simulation techniques in test environments





