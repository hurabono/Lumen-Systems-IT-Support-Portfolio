# SOP-002: VPN Account Provisioning

**Category:** Network / Access
**Owner:** Network Administrator
**Last Updated:** 2026-02-03

## Purpose
Ensure remote access is granted securely and consistently for employees requiring VPN connectivity.

## Trigger
Ticket submitted by employee or manager requesting remote access, OR automatically triggered during new hire onboarding for roles flagged as "remote-eligible."

## Procedure
1. **Confirm eligibility** — verify manager approval is attached (required for all non-IT/non-Sales roles).
2. **Create VPN profile** — provision Cisco AnyConnect profile tied to the employee's Entra ID account.
3. **Enforce MFA** — confirm the employee has completed Microsoft Authenticator enrollment; VPN access is blocked without MFA.
4. **Assign access group** — map to the correct network segment (e.g., Sales VLAN, Finance VLAN) based on department — see `network/network-diagram.md`.
5. **Test connection** — have the employee connect while on a call/screen share; confirm they can reach required internal resources (e.g., shared drives, ERP system).
6. **Document** — log the ticket with VPN profile name and access group assigned.

## Common Failure Points
- MFA not yet enrolled → redirect to self-service enrollment portal first.
- Split-tunnel misconfiguration → verify correct profile was assigned for the employee's department.
- Expired client certificate → escalate to Network Administrator (Tier 3).

## Related
- `network/network-diagram.md`
- `tickets/network/` (see resolved VPN tickets for real examples)
