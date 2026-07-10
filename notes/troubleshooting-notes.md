# Troubleshooting Notes

## Problem

Event ID 4720 not appearing

### Solution

Ensure the user account was successfully created.

Verify:

```powershell
net user
```

---

## Problem

Event ID 4732 missing

### Solution

Confirm the account was actually added to Administrators.

```powershell
net localgroup Administrators
```

---

## Problem

PowerShell returns no events

### Solution

Use elevated PowerShell (Run as Administrator).

---

## Problem

Incorrect event displayed

### Solution

Retrieve only the latest event.

```powershell
Get-WinEvent -LogName Security |
Where-Object {$_.Id -eq 4720} |
Select-Object -First 1
```

or

```powershell
Get-WinEvent -LogName Security |
Where-Object {$_.Id -eq 4732} |
Select-Object -First 1
```

---

## Problem

User cannot be deleted

### Solution

Remove administrator membership first.

```powershell
net localgroup Administrators SOCUser /delete
```

Then delete the account.

```powershell
net user SOCUser /delete
```

---

## Problem

Access Denied

### Solution

Run Command Prompt or PowerShell as Administrator.

---

## Problem

Security logs not updating

### Solution

Refresh Event Viewer (F5) or reopen the Security log.

---

## Lessons Learned

- Event ID 4720 records account creation.
- Event ID 4732 records administrator group membership changes.
- Native Windows logs provide sufficient evidence for investigating local privilege escalation without requiring additional security tools.
