# SOP-003: Employee Offboarding

**Category:** Account & Access / Security
**Owner:** IT Support Manager
**Last Updated:** 2026-01-20

## Purpose
Ensure all system access is revoked promptly and securely when an employee departs, and company assets are recovered.

## Trigger
HR submits an "Offboarding" ticket, ideally with 24–48 hours' notice; immediate/same-day for involuntary terminations (flagged **P1 — Security**).

## Procedure
1. **Confirm last day and type** — voluntary vs. involuntary (involuntary terminations require immediate account disable, coordinated with HR timing).
2. **Disable account** — disable (do not delete) Entra ID / AD account immediately at the specified time.
3. **Revoke VPN & MFA** — remove VPN profile and revoke MFA tokens/devices.
4. **Set up mail handling** — convert mailbox to shared mailbox or set auto-forward per manager's instruction; set out-of-office if requested.
5. **Recover assets** — coordinate laptop/phone return via Emily Zhao (Asset & Procurement); update `assets/asset-inventory.md` status to "Retired" or "Spare" pending wipe.
6. **Remove from security groups & shared drives.**
7. **Update Employee Directory** — mark record as terminated with departure date; archive per data retention policy.
8. **Notify manager** — confirm all steps complete.

## Involuntary Termination — Additional Steps
- Disable account **before** the employee is notified, coordinated precisely with HR/Manager timing.
- Revoke building access badge simultaneously (coordinate with Facilities).

## Related
- `employees/employee-directory.md`
- `assets/asset-inventory.md`
