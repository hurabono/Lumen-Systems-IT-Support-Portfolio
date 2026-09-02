# Knowledge Base

This folder contains internal documentation written after resolving tickets — especially recurring issues — so the wider IT team (and future you) doesn't have to re-solve the same problem from scratch.

## Categories

| Category | Examples |
|---|---|
| Account & Access | Password resets, MFA setup, account lockouts |
| Hardware | Printer setup, monitor/dock issues, laptop imaging |
| Software | Office 365 install issues, application crashes |
| Network | VPN reconnect steps, Wi-Fi troubleshooting, DNS issues |
| Security | Phishing report process, malware removal steps |

## Naming Convention

`KB-[number]-[short-title].md` → e.g. `KB-014-vpn-reconnect-steps.md`

## Article Template

```markdown
# KB-[number]: [Title]

**Category:** [Category]
**Last Updated:** YYYY-MM-DD
**Author:** [Your Name]

## Issue
Brief description of the problem this article addresses.

## Applies To
Who/what this affects (e.g., "All Windows 11 laptops on Cisco AnyConnect VPN").

## Resolution Steps
1. Step one
2. Step two
3. Step three

## Notes
Any edge cases, related tickets, or escalation path if steps don't resolve the issue.
```

## Sample Articles

## Articles

| ID | Title | Category |
|---|---|---|
| [KB-001](./articles/KB-001-mapped-drive-missing-unc-works.md) | Mapped drive is missing but the UNC path opens | Account & Access / Group Policy |
| [KB-002](./articles/KB-002-account-lockout-unlock-and-verify.md) | Unlocking a locked AD account and confirming it stays unlocked | Account & Access |
