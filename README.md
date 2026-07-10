# windows-security-user-management-investigation
## Overview

This project demonstrates how Windows Security Logs can be used to detect suspicious user account creation and privilege escalation activities.

The investigation focuses on two important Windows Security Event IDs commonly monitored by Security Operations Centers (SOC):

- Event ID 4720 – User Account Created
- Event ID 4732 – Member Added to Local Administrators Group

Both Event Viewer and PowerShell were used to investigate these events.

---

## Objectives

- Create a local Windows user
- Add the user to the Administrators group
- Investigate security logs
- Validate activity using PowerShell
- Understand privilege escalation detection

---

## Environment

- Windows 10 x64
- VMware Workstation Player
- Windows Event Viewer
- PowerShell
- Microsoft Defender enabled

---

## Lab Steps

### 1. Create a Local User

```powershell
net user SOCUser P@ssw0rd123 /add
```

---

### 2. Verify User

```powershell
net user SOCUser
```

---

### 3. Add User to Administrators

```powershell
net localgroup Administrators SOCUser /add
```

---

### 4. Verify Membership

```powershell
net localgroup Administrators
```

---

### 5. Investigate Event ID 4720

Navigate to:

Event Viewer

Windows Logs

Security

Filter Current Log

Event ID:

4720

Review:

- Creator
- Account Name
- Security Identifier (SID)
- Timestamp

---

### 6. Investigate Event ID 4732

Filter Security Log:

4732

Review:

- User added
- Target Group
- Administrator who performed action
- Time

---

### 7. Validate with PowerShell

Latest User Creation

```powershell
Get-WinEvent -LogName Security |
Where-Object {$_.Id -eq 4720} |
Select-Object -First 1 |
Format-List
```

Latest Administrator Group Addition

```powershell
Get-WinEvent -LogName Security |
Where-Object {$_.Id -eq 4732} |
Select-Object -First 1 |
Format-List
```

---

### 8. Cleanup

Remove user from Administrators

```powershell
net localgroup Administrators SOCUser /delete
```

Delete account

```powershell
net user SOCUser /delete
```

---

## Security Relevance

Attackers frequently:

- Create persistence accounts
- Create hidden administrator accounts
- Elevate privileges after initial compromise

Monitoring Event IDs 4720 and 4732 helps detect:

- Unauthorized account creation
- Insider threats
- Privilege escalation
- Persistence techniques

---

## Skills Practiced

- Windows Security Logs
- Event Viewer
- PowerShell Log Analysis
- Privilege Escalation Detection
- Windows User Administration
- SOC Investigation Workflow
