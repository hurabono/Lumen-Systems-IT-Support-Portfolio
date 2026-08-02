# SOP-001: New Hire Onboarding

**Category:** Account & Access
**Owner:** IT Support Manager
**Last Updated:** 2026-01-15

## Purpose
Standardize IT account provisioning for new employees to ensure Day 1 readiness.

## Trigger
HR submits a "New Hire" ticket at least 3 business days before the employee's start date, including: full name, department, title, manager, location, and start date.

## Procedure
1. **Verify request** — confirm ticket includes manager approval and all required fields.
2. **Create Entra ID / AD account** — use naming convention `firstname.lastname@lumensystems.com`; add to appropriate security groups based on department.
3. **Assign Microsoft 365 license** — E3 for standard employees, E5 for department heads and above.
4. **Provision hardware** — check Asset Inventory for available spare; if none, submit procurement request (see `SOP-006-new-asset-intake.md`).
5. **Configure device** — image via Intune Autopilot, confirm VPN profile and MFA enrollment.
6. **Grant department-specific access** — shared drives, line-of-business apps, per manager's request.
7. **Update Employee Directory** — add new entry with Employee ID, department, manager, and start date.
8. **Day 1 handoff** — deliver device and send welcome email with login instructions.

## SLA
Complete provisioning no later than 9:00 AM on the employee's start date.

## Related
- `employees/employee-directory.md`
- `assets/asset-inventory.md`
- `SOP-002-vpn-account-provisioning.md`
