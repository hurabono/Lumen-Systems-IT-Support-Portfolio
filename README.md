# Lumen Systems IT Support Portfolio

> **Disclaimer:** Lumen Systems is a fictional company created for portfolio purposes only. All employees, tickets, and scenarios in this repository are simulated to demonstrate real-world IT support workflows, troubleshooting methodology, and documentation practices. No real company data is used.

## Purpose

This repository simulates the day-to-day IT Support operations of a mid-sized Toronto-based company. I built the environment myself and run it as a real queue: users raise tickets through a Jira Service Management portal form, and I work each one end to end as the service desk agent. Reproduce the symptom, narrow the scope, form and test a hypothesis, verify the diagnosis before changing anything, then confirm the fix and check for recurrence.

Dead ends are documented rather than edited out, because the wrong turns are usually where the useful reasoning lives.

## Lab Environment

| Component | Detail |
|---|---|
| Hypervisor | VMware Workstation |
| Domain Controller | Windows Server 2025 (`DC01`) |
| Client | Windows 11 (`CLIENT01`) |
| Domain | `LUMEN.LOCAL` |
| Service Desk | Jira Service Management (portal intake, ticket types, priorities, components) |
| Roles configured | Active Directory Domain Services, DNS, DHCP |
| Also configured | Group Policy, NTFS permissions, SMB file shares, OUs, security groups |

Full build notes: [lab/Day1-environment-setup.md](./lab/Day1-environment-setup.md)

## Tickets

| ID | Issue | Category | What it demonstrates |
|---|---|---|---|
| [LUM-001](./tickets/account-access/LUM-001-ntfs-subfolder-access-denied.md) | Subfolder denies access while the parent share opens normally | File Server / NTFS | Share and NTFS permissions are separate layers; `icacls`, peer comparison, Effective Access verification, least-privilege check after the fix |
| [LUM-002](./tickets/account-access/LUM-002-ad-account-lockout.md) | User locked out and cannot sign back in | Active Directory | Event 4740 vs 4771, `auditpol` subcategories, identity verification before unlocking, 20-minute recurrence check |
| [LUM-003](./tickets/group-policy/LUM-003-gpo-not-applying-ou-scope.md) | Department GPO reaches one user but not another | Group Policy | `gpresult /r /scope:user`, comparing against a working peer, user object located outside the linked OU |
| [LUM-004](./tickets/group-policy/LUM-004-mapped-drive-missing-item-level-targeting.md) | Mapped drive missing although the UNC path opens fine | Group Policy Preferences | GPO delivery vs preference execution, item-level targeting, why `gpupdate /force` does not rebuild an access token |

## Knowledge Base

Written after resolving tickets, so the same problem does not have to be solved from scratch twice.

| ID | Title |
|---|---|
| [KB-001](./knowledge-base/articles/KB-001-mapped-drive-missing-unc-works.md) | Mapped drive is missing but the UNC path opens |
| [KB-002](./knowledge-base/articles/KB-002-account-lockout-unlock-and-verify.md) | Unlocking a locked AD account and confirming it stays unlocked |

## Repository Structure

```
Lumen-Systems-IT-Support-Portfolio/
├── README.md
├── company-profile.md              # Company background & IT environment
├── org-chart.md                    # IT department structure
├── assets/
│   └── asset-inventory.md          # Hardware asset lifecycle tracking
├── employees/
│   └── employee-directory.md       # Company-wide employee directory
├── network/
│   └── network-diagram.md          # Network topology (Mermaid diagram)
├── sop/
│   ├── SOP-001-new-hire-onboarding.md
│   ├── SOP-002-vpn-account-provisioning.md
│   ├── SOP-003-offboarding.md
│   └── SOP-004-password-reset.md
├── templates/
│   └── ticket-template.md          # Format every ticket follows
├── tickets/
│   ├── account-access/
│   └── group-policy/
├── knowledge-base/
│   ├── README.md
│   └── articles/
└── lab/
    ├── Day1-environment-setup.md
    └── screenshots/
```

## Where to Start

If you only read one file, read [LUM-001](./tickets/account-access/LUM-001-ntfs-subfolder-access-denied.md). It shows the full method, including the query I got wrong and what that mistake taught me about the environment.

## Skills Demonstrated

- Service desk operations in Jira Service Management: portal intake, ticket typing, prioritization (P1 to P4), components, work logs, resolution notes
- Structured troubleshooting: reproduce, scope, hypothesise, verify, resolve, confirm
- Active Directory administration: users, OUs, security groups, account lockout
- Group Policy: GPO scope and linking, Group Policy Preferences, item-level targeting
- File services: SMB shares, NTFS permissions, Effective Access, least privilege
- Windows event log analysis and audit policy configuration
- PowerShell for AD and file service administration
- Technical writing and knowledge base documentation
- ITIL-aligned incident and service request terminology

---

Built and documented by Stephanie (Heesu) Cho · Toronto, ON
