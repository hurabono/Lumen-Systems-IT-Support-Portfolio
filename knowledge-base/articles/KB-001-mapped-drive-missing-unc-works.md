# KB-001: Mapped drive is missing but the UNC path opens

**Category:** Account & Access / Group Policy
**Applies to:** Windows 11 domain clients, drives delivered by Group Policy Preferences
**Last updated:** 2026-08-26
**Related ticket:** LUM-004

---

## Issue

A user reports that a mapped network drive (for example `H:`) is not present in File Explorer. There is no error message. Typing the UNC path directly into the address bar opens the folder without any problem.

## Why this happens

A drive delivered by Group Policy Preferences involves two separate things that can each fail:

1. **Delivery** — the GPO container reaches the user. `gpresult` reports this.
2. **Execution** — the Drive Maps preference item inside the GPO actually runs. Item-level targeting is evaluated on the client *after* delivery, and `gpresult` says nothing about it.

A GPO showing as Applied therefore does not mean the drive mapping ran. The most common cause is item-level targeting on a security group the user is not a member of.

## Ruling out the permissions layer first

If the user can open the UNC path directly, the following are already proven healthy and do not need checking:

- Network connectivity between the client and the file server
- DNS resolution of the server hostname
- SMB share-level permissions
- NTFS ACLs on the target folder

Start at the Preferences layer instead. This saves most of the diagnostic time.

## Resolution steps

**1. Confirm there really are no mappings, in the user's own session**

```powershell
net use
Get-SmbMapping
```

Both returning empty means zero mappings, not a broken mapping.

**2. Confirm the share itself is reachable**

```powershell
Test-Path "\\DC01\CompanyData\HR"
```

`True` confirms the permissions layer is fine.

**3. Check whether the GPO was delivered**

Run at standard privilege, inside the user's session:

```cmd
gpresult /r /scope:user
```

If the GPO appears under Applied Group Policy Objects, delivery is not the problem. Go to step 4.
If it does not appear, this is a scope or OU problem instead. See LUM-003.

**4. Check item-level targeting on the preference item**

On the domain controller: `gpmc.msc` → edit the GPO → User Configuration → Preferences → Windows Settings → Drive Maps → double-click the drive → **Common** tab → Item-level targeting → Targeting.

Note which security group the item is filtered on.

**5. Check the user's membership of that group**

```powershell
Get-ADGroupMember -Identity "HR-Drive-Users" | Select-Object Name, SamAccountName
```

**6. Add the user if missing**

```powershell
Add-ADGroupMember -Identity "HR-Drive-Users" -Members skim
Get-ADGroupMember -Identity "HR-Drive-Users" | Select-Object Name, SamAccountName
```

GUI: `dsa.msc` → the OU → double-click the group → Members tab → Add → Check Names → OK

**7. Have the user sign out and back in**

`gpupdate /force` is **not** sufficient here. It refreshes policy but does not rebuild the user's access token, so any condition that depends on group membership keeps evaluating against the old token. A full sign-out and sign-in is required.

**8. Verify**

```powershell
net use
Get-SmbMapping
```

The drive letter should now be present.

## What to tell the user

Tell them up front that they need to sign out and back in, not just restart File Explorer or wait. Otherwise you will get a follow-up saying the fix did not work, when it actually did.

## Escalate if

- The GPO does not appear in `gpresult` at all (scope or OU issue, see LUM-003)
- The user is already in the targeted group and the drive still does not map after a fresh logon
- Multiple users in the same group are affected simultaneously

## Key point

> A GPO applying and a setting inside that GPO executing are two different claims. If a drive, printer, or shortcut is missing while `gpresult` looks perfectly healthy, look at the Preferences layer, not the GPO layer.
