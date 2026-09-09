# Tickets

Support tickets worked end to end in the Lumen Systems lab environment. Each one follows the same method, and each one is written up whether or not the path to the answer was clean.

**Environment:** Windows Server 2025 (`DC01`), Windows 11 (`CLIENT01`), domain `LUMEN.LOCAL`
Build notes: [../lab/Day1-environment-setup.md](../lab/Day1-environment-setup.md)

---

## Index

### Account & Access

| ID | Issue | What it demonstrates |
|---|---|---|
| [LUM-001](./account-access/LUM-001-ntfs-subfolder-access-denied.md) | A subfolder denies access while the parent share opens normally | Share and NTFS permissions are separate layers · `icacls` · comparing against a working peer folder · Effective Access verification before changing anything · least-privilege check after the fix |
| [LUM-002](./account-access/LUM-002-ad-account-lockout.md) | User is locked out and cannot sign back in | Event 4740 vs 4771 · `auditpol` subcategories and why 4771 can be silent · identity verification before unlocking · 20-minute recurrence check |

### Group Policy

| ID | Issue | What it demonstrates |
|---|---|---|
| [LUM-003](./group-policy/LUM-003-gpo-not-applying-ou-scope.md) | A department GPO reaches one user but not their teammate | `gpresult /r /scope:user` · side-by-side comparison with a working account · user object sitting outside the linked OU |
| [LUM-004](./group-policy/LUM-004-mapped-drive-missing-item-level-targeting.md) | Mapped drive is missing although the UNC path opens fine | GPO delivery vs preference execution · item-level targeting · why `gpupdate /force` does not rebuild an access token |

### Employee Lifecycle

| ID | Issue | What it demonstrates |
|---|---|---|
| [LUM-005](./employee-lifecycle/LUM005-account-onboarding.md) | New hire needs an account, group access and a working desktop on day one | Provisioning against a request form rather than assumptions · OU placement driving GPO scope · reading the domain's real password policy instead of the Windows default · verifying access without logging in as the user |
| [LUM-006](./employee-lifecycle/LUM-006-employee-offboarding.md) | Contract employee leaves and their access has to be closed out | Disable before delete · password reset and group removal as separate revocation layers · Kerberos tickets outliving a disabled account · retention period documented rather than assumed |

### Networking

| ID | Issue | What it demonstrates |
|---|---|---|
| [LUM-007](./networking/LUM-007-dns-name-resolution-failure.md) | Internet and the mapped drive work, but the server's full name will not resolve | Isolating name resolution from connectivity · `nslookup` against a specific server to separate client fault from server fault · AD SRV records under `_msdcs` · single-label fallback (mDNS / LLMNR / NetBIOS) masking a DNS failure · `gpresult` reporting cache while `gpupdate` reports reality |

---

## Method

Every ticket here follows the same sequence.

1. **Reproduce** the symptom in the user's own session. A report is not evidence.
2. **Narrow the scope.** What still works? Who else is affected? What worked yesterday? You have to know how far normal extends before you can find where abnormal begins.
3. **Rule out the common cause first,** then keep going when it turns out not to be that.
4. **Verify the diagnosis before changing anything.** Effective Access, event logs, a peer comparison. Fixing on a hunch and confirming a cause are different things.
5. **Resolve,** with both the PowerShell and GUI paths documented.
6. **Confirm the fix,** and confirm nothing else opened up that should not have.
7. **Check for recurrence** where the failure mode allows it to come back.

## On the dead ends

Wrong turns are left in rather than edited out. In LUM-001 I queried a share that did not exist because I had assumed the file server was split by department. The command failed, and that failure is what gave me an accurate picture of the structure.

LUM-007 went further and left in the checks that came back healthy while the fault was still live. Three of them did. Treating those as evidence of a working system would have sent the whole investigation somewhere else, so they are recorded as part of the diagnosis rather than trimmed out of it.

A write-up where everything worked first try teaches nobody anything, including me.

## Ticket format

All tickets follow [../templates/ticket-template.md](../templates/ticket-template.md).

## Priority definitions

| Priority | Meaning |
|---|---|
| P1 | Service down for multiple users, or a business-critical function blocked |
| P2 | A single user fully blocked from working |
| P3 | Degraded service with a workaround available |
| P4 | Request or low-impact question |
