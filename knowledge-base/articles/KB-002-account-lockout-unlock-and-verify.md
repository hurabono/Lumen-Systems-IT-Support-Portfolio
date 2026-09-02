# KB-002: Unlocking a locked AD account and confirming it stays unlocked

**Category:** Account & Access
**Applies to:** Active Directory domain accounts
**Last updated:** 2026-08-24
**Related ticket:** LUM-002

---

## Issue

A user reports they cannot sign in and see a message that their account is locked out. They may say they mistyped their password, or they may have no idea why it happened.

## Why the recurrence check matters

Unlocking an account is one command. What separates a resolved ticket from a ticket that reopens in thirty minutes is knowing *why* it locked:

- **Human error** — the user mistyped their password at the logon screen. One-off. Unlock and close.
- **Stale cached credential** — an old password is still stored in Credential Manager, a mapped drive, a scheduled task, or a phone mail client. It auto-retries and re-locks the account on a cycle.

Closing without checking is a guess dressed up as a resolution.

## Resolution steps

**1. Confirm the lockout state on the domain controller**

```powershell
Search-ADAccount -LockedOut | Select-Object Name, SamAccountName, LockedOut
```

**2. Check the lockout policy values for this domain**

```powershell
Get-ADDefaultDomainPasswordPolicy
```

Note the threshold, duration, and observation window. Defaults vary by environment. This command is **read-only**; changing a value means editing the Default Domain Policy in `gpmc.msc`.

**3. Find the source**

Filter the DC's Security log:

| Event ID | Meaning |
|---|---|
| 4740 | Account was locked out. Includes `Caller Computer Name`, which identifies the source workstation |
| 4771 | Kerberos pre-authentication failed. One per individual failed attempt |
| 4625 | Failed logon |

```powershell
Get-WinEvent -FilterHashtable @{LogName='Security'; Id=4740,4771,4776; StartTime=(Get-Date).AddHours(-3)} |
  Select-Object TimeCreated, Id, Message | Format-List
```

**If 4771 returns nothing, do not conclude there is no evidence.** Check whether auditing is even enabled:

```powershell
auditpol /get /subcategory:"Kerberos Authentication Service"
```

A default Windows Server install logs the lockout result (4740) but does not record individual failed attempts (4771) unless that subcategory is turned on. To enable it:

```powershell
auditpol /set /subcategory:"Kerberos Authentication Service" /success:enable /failure:enable
auditpol /set /subcategory:"Logon" /success:enable /failure:enable
```

Note that enabling auditing is **not retroactive**. It only records events from that point forward.

Even with 4740 alone, `Caller Computer Name` is enough to identify the source workstation and run a baseline investigation.

**4. Verify the caller's identity before unlocking**

```powershell
Get-ADUser <samaccountname> -Properties Department, Title, Office, OfficePhone |
  Format-List Name, Department, Title, Office, OfficePhone
```

Confirm against what the caller tells you. Never unlock on a name alone.

**5. Unlock**

```powershell
Unlock-ADAccount -Identity <samaccountname>
Search-ADAccount -LockedOut          # empty result = unlocked
```

GUI: `dsa.msc` → the user → Account tab → tick **Unlock account** → Apply → OK

**6. Confirm the user can sign in**

**7. Monitor for recurrence**

```powershell
1..20 | ForEach-Object {
  $u = Get-ADUser <samaccountname> -Properties LockedOut, BadPwdCount, LastBadPasswordAttempt
  "{0:HH:mm:ss}  Locked={1}  BadPwdCount={2}  LastBad={3}" -f `
    (Get-Date), $u.LockedOut, $u.BadPwdCount, $u.LastBadPasswordAttempt
  Start-Sleep -Seconds 60
}
```

`Locked=False` and `BadPwdCount=0` across the window means no recurrence. Close the ticket.

If `BadPwdCount` climbs on its own with the user doing nothing, it is a stale credential. Go to the next section.

## If it recurs: finding the stale credential

Check these on the source workstation identified by event 4740:

- Credential Manager (`control /name Microsoft.CredentialManager`)
- Mapped drives created with saved credentials (`net use`)
- Scheduled tasks running as the user
- Mail clients on the user's phone still holding the old password

## Note on the client-side delay message

`Delaying next attempt for security reasons` at the Windows logon screen is a **local sign-in throttling feature on the client**, separate from the domain Account Lockout Policy. Any failed attempt already sent is still recorded on the DC regardless.

## Key point

> Anyone can unlock an account. Telling a one-off typo apart from a stale credential that will re-lock the account every thirty minutes is the actual Tier 1 skill.
