# Investigation Notes

## Scenario

A SOC analyst receives an alert indicating that a new local administrator account may have been created.

The objective is to determine:

- Was a user account created?
- Was administrator access granted?
- Who performed the activity?
- Is the activity legitimate?

---

# Event ID 4720

## Description

A new local user account was created.

### Findings

Account Created:

SOCUser

Creator:

Dell

Log Name:

Security

Task Category:

User Account Management

Status:

Audit Success

---

## Key Evidence

Observed:

- Username
- SID
- Account creation time
- Account attributes

PowerShell confirmed the same event.

---

# Event ID 4732

## Description

A user was added to a security-enabled local group.

### Findings

User Added:

SOCUser

Target Group:

Administrators

Performed By:

Dell

Status:

Audit Success

---

## PowerShell Validation

User Creation

```powershell
Get-WinEvent -LogName Security |
Where-Object {$_.Id -eq 4720} |
Select-Object -First 1 |
Format-List
```

Administrator Group Addition

```powershell
Get-WinEvent -LogName Security |
Where-Object {$_.Id -eq 4732} |
Select-Object -First 1 |
Format-List
```

---

# Timeline

06:30

User account created

↓

06:33

User added to Administrators group

↓

SOC investigation initiated

↓

Evidence collected

↓

Account removed

---

# MITRE ATT&CK Mapping

| Activity | Technique |
|----------|-----------|
| Local Account Creation | T1136.001 |
| Account Manipulation | T1098 |
| Privilege Escalation | TA0004 |
| Persistence | TA0003 |

---

# SOC Analyst Observations

No malware activity was observed.

The investigation demonstrates how privilege escalation can be detected using native Windows Security logs.

Both Event Viewer and PowerShell produced consistent evidence.
