# Security Fundamentals Assessment

## Authorized Lab Security Assessment — Metasploitable 2

This repository contains my security fundamentals assessment performed in an authorized, intentionally vulnerable training environment.

The assessment was conducted from Kali Linux against a Metasploitable 2 virtual machine running in VMware.

> **Disclaimer:** This assessment was performed only against an intentionally vulnerable lab system that I was authorized to test. No unauthorized systems were targeted.

---

## 1. Assessment Objective

The objective of this assessment was to:

- Identify exposed network services
- Identify potentially vulnerable software
- Collect technical evidence
- Assess the security risks associated with the findings
- Recommend appropriate mitigations
- Document the assessment professionally

The assessment focused on reconnaissance, service enumeration, evidence collection, and risk assessment. Exploitation was not performed.

---

## 2. Lab Environment

| Component | Details |
|---|---|
| Assessment machine | Kali Linux |
| Target machine | Metasploitable 2 |
| Virtualization | VMware |
| Kali IP | 192.168.174.129 |
| Target IP | 192.168.174.128 |
| Environment | Authorized isolated training lab |

---

## 3. Tools Used

- Nmap
- SearchSploit
- curl
- smbclient
- Kali Linux
- VMware

---

## 4. Assessment Methodology

The assessment followed these general steps:

1. Identified active hosts on the lab network.
2. Identified the Metasploitable 2 target.
3. Enumerated TCP services using Nmap.
4. Identified service versions.
5. Investigated detected software for known security issues.
6. Tested SMB for anonymous access.
7. Captured screenshots as evidence.
8. Assessed the risk of each finding.
9. Recommended security mitigations.

---

## 5. Key Findings

### F-01 — Telnet Service Exposed

**Severity:** Medium

**Affected component:** TCP port 23 / Telnet

Nmap identified an open Telnet service on the target.

**Risk:**

Telnet is an insecure remote administration protocol because communication is not protected in the same way as SSH. Credentials and session information may be exposed to network interception.

**Recommended mitigation:**

- Disable Telnet where it is not required.
- Use SSH for secure remote administration.
- Restrict administrative services to trusted networks.
- Use strong authentication.

---

### F-02 — Vulnerable vsftpd 2.3.4

**Severity:** Critical

**Affected component:** TCP port 21 / vsftpd 2.3.4

Nmap identified vsftpd version 2.3.4. SearchSploit returned references to a known backdoor command-execution issue associated with this version.

**Risk:**

A vulnerable FTP service may allow unauthorized command execution and potentially lead to compromise of the affected system.

**Recommended mitigation:**

- Remove or upgrade vsftpd 2.3.4.
- Apply current security updates.
- Restrict unnecessary FTP exposure.
- Prefer secure file-transfer protocols such as SFTP where appropriate.
- Monitor FTP authentication and service activity.

**Note:** The vulnerability was identified during assessment, but no exploit was executed.

---

### F-03 — Outdated Apache and PHP

**Severity:** High

**Affected component:** TCP port 80 / Apache HTTP Server and PHP

The web service reported:

- Apache HTTP Server 2.2.8
- PHP 5.2.4

These are obsolete software versions.

**Risk:**

Outdated software may contain publicly known security vulnerabilities. Server version information can also assist attackers during reconnaissance.

**Recommended mitigation:**

- Upgrade Apache to a supported version.
- Upgrade PHP to a supported version.
- Apply security updates regularly.
- Remove unnecessary modules.
- Minimize unnecessary software version disclosure.

---

### F-04 — Anonymous SMB Access

**Severity:** High

**Affected component:** TCP ports 139/445 / Samba

Anonymous SMB enumeration was successful.

The assessment identified accessible shares including:

- `print$`
- `tmp`
- `opt`
- `IPC$`
- `ADMIN$`

Anonymous access to the `tmp` share was also successfully established and directory contents were listed.

**Risk:**

Unauthenticated network share access can expose files and directory information to unauthorized users.

**Recommended mitigation:**

- Disable anonymous/guest SMB access unless explicitly required.
- Require authentication.
- Apply least-privilege permissions.
- Restrict access to trusted users and networks.
- Disable unnecessary shares.
- Review permissions regularly.

---

## 6. Risk Summary

| Finding | Severity | Main Risk |
|---|---|---|
| Telnet exposed | Medium | Insecure remote administration |
| vsftpd 2.3.4 | Critical | Known backdoor command-execution vulnerability |
| Outdated Apache/PHP | High | Obsolete vulnerable software |
| Anonymous SMB access | High | Unauthenticated share access |

---

## 7. Recommended Remediation Priority

1. Upgrade or remove the vulnerable vsftpd 2.3.4 service.
2. Disable anonymous SMB access and review share permissions.
3. Upgrade the outdated Apache/PHP web stack.
4. Disable Telnet and use SSH.
5. Re-scan the system after remediation to verify that the identified weaknesses have been addressed.

---

## 8. Evidence

Screenshots collected during the assessment are stored in the [`evidence`](./evidence) directory.

Evidence includes:

- Nmap service enumeration
- Telnet service identification
- vsftpd version identification
- SearchSploit results
- Apache/PHP version information
- SMB enumeration
- Anonymous SMB access
- SMB share contents

---

## 9. Detailed Report

The complete assessment report is available here:

[Security Fundamentals Assessment Report](./Security_Fundamentals_Assessment_Report.docx)

---

## 10. Conclusion

The assessment identified multiple security weaknesses in the intentionally vulnerable Metasploitable 2 environment.

The findings demonstrate the importance of:

- Keeping software patched and supported
- Disabling unnecessary services
- Using secure remote administration protocols
- Requiring authentication for network shares
- Applying least-privilege access controls
- Regularly performing security assessments

This assessment was conducted in an authorized training environment for educational purposes.
