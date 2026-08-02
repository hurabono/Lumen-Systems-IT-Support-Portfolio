# Asset Inventory

Tracks all IT-managed hardware assets: assignment, lifecycle, and warranty status.

## Legend
- **Status:** Active / In Repair / Spare / Retired
- **Warranty Status:** flagged when < 3 months remaining

| Asset ID | Type | Make / Model | Serial Number | Assigned To | Department | Purchase Date | Warranty Expiry | Status |
|---|---|---|---|---|---|---|---|---|
| LUM-LT-1001 | Laptop | Dell Latitude 5440 | 5CD1234ABC | Amanda Rodriguez | IT | 2023-08-14 | 2026-08-14 | Active |
| LUM-LT-1002 | Laptop | Dell Latitude 5440 | 5CD1234ABD | Kevin Park | IT | 2023-08-14 | 2026-08-14 | Active |
| LUM-LT-1015 | Laptop | Lenovo ThinkPad T14 | PF3KX8Z1 | Rachel Nguyen | Sales | 2023-11-02 | 2026-11-02 | Active |
| LUM-LT-1022 | Laptop | Dell Latitude 5430 | 5CD9988XYZ | Tom Bradley | Warehouse Ops | 2022-05-20 | 2025-05-20 | ⚠️ Expired |
| LUM-DT-2004 | Desktop | HP EliteDesk 800 G9 | HP2024-0044 | Jennifer Lu | Finance | 2024-01-10 | 2027-01-10 | Active |
| LUM-MON-3010 | Monitor | Dell P2422H | DL2023-991 | Rachel Nguyen | Sales | 2023-11-02 | 2026-11-02 | Active |
| LUM-PRN-4001 | Printer | Canon imageRUNNER C3226i | CN-88213 | Front Office (Shared) | Facilities | 2021-09-01 | 2024-09-01 | ⚠️ Expired |
| LUM-PHN-5003 | Mobile | iPhone 14 | IMEI-556213 | Michael Chen | IT (Exec) | 2023-03-15 | 2025-03-15 | ⚠️ Expired |
| LUM-LT-1030 | Laptop | Dell Latitude 5440 | 5CD1234SPARE | *Unassigned* | IT | 2024-02-01 | 2027-02-01 | Spare |
| LUM-LT-0998 | Laptop | Dell Latitude 5420 | 5CD0021OLD | *Unassigned* | IT | 2020-06-01 | 2023-06-01 | Retired |

## Notes
- Assets flagged ⚠️ should be prioritized for replacement in the next procurement cycle (see `SOP-005-hardware-lifecycle.md`).
- Retired assets are wiped per the Data Sanitization Policy before disposal.
- New assets are logged here immediately upon receipt — see `sop/SOP-006-new-asset-intake.md`.
