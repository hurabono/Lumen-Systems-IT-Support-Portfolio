# SOP-004: Password Reset

**Category:** Account & Access
**Owner:** Service Desk (Tier 1)
**Last Updated:** 2026-01-08

## Purpose
Standardize identity verification and reset process to prevent social-engineering-based account compromise.

## Trigger
Employee contacts service desk (phone, Teams, or ticket) unable to log in due to forgotten/expired password.

## Procedure
1. **Verify identity** — required before any reset:
   - Employee ID **and**
   - One additional verification: manager confirmation via Teams, or callback to the employee's directory-listed phone number (never a number provided in the request itself).
2. **Check account status** — confirm account isn't disabled/locked for a different reason (e.g., offboarding in progress, security hold).
3. **Reset password** — via Entra ID admin center; generate a temporary password.
4. **Force change on next login.**
5. **Confirm MFA is intact** — do not reset MFA as part of a standard password reset; MFA reset requires separate identity verification (see Security Escalation below).
6. **Document** — log ticket with verification method used.

## Security Escalation
If the request also involves resetting MFA/Authenticator, or the employee cannot be reached via directory-listed contact info, escalate to James Wilson (Security Analyst) before proceeding — this pattern matches common account-takeover attempts.

## Related
- `employees/employee-directory.md` (for callback verification)
- `tickets/account-access/` (see resolved examples)
