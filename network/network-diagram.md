# Network Diagram

> This diagram uses [Mermaid](https://mermaid.js.org/) syntax, which renders automatically in GitHub's markdown viewer — no image file needed.

## Overview

Lumen Systems operates a hub-and-spoke network: Toronto HQ as the core, with site-to-site VPN tunnels to the Mississauga Warehouse and a remote-access VPN for the Vancouver Sales Office and traveling employees.

```mermaid
graph TB
    Internet((Internet))

    subgraph HQ["Toronto HQ"]
        FW_HQ[Meraki MX Firewall]
        SW_HQ[Meraki MS Switch Stack]
        WIFI_HQ[Meraki Wi-Fi APs]
        SRV[On-Prem Servers<br/>AD / File Server]
        Endpoints_HQ[HQ Workstations<br/>~120 devices]

        FW_HQ --- SW_HQ
        SW_HQ --- WIFI_HQ
        SW_HQ --- SRV
        SW_HQ --- Endpoints_HQ
    end

    subgraph WH["Mississauga Warehouse"]
        FW_WH[Meraki MX Firewall]
        SW_WH[Meraki MS Switch]
        Endpoints_WH[Warehouse Terminals<br/>~25 devices]

        FW_WH --- SW_WH
        SW_WH --- Endpoints_WH
    end

    subgraph Remote["Remote / Vancouver Sales"]
        VPNClients[AnyConnect VPN Clients<br/>Sales + Traveling Staff]
    end

    subgraph Cloud["Microsoft Cloud"]
        EntraID[Entra ID / Azure AD]
        M365[Microsoft 365]
        Intune[Intune MDM]
    end

    Internet --- FW_HQ
    FW_HQ ---|Site-to-Site VPN Tunnel| FW_WH
    Internet --- VPNClients
    VPNClients ---|AnyConnect Remote VPN| FW_HQ
    FW_HQ ---|HTTPS| EntraID
    FW_HQ ---|HTTPS| M365
    Endpoints_HQ -.->|Managed by| Intune
    Endpoints_WH -.->|Managed by| Intune
    VPNClients -.->|Managed by| Intune
```

## Key Paths (for troubleshooting reference)

| Scenario | Path |
|---|---|
| HQ user can't reach file server | Endpoint → HQ Switch → HQ Firewall (internal routing) |
| Warehouse can't reach HQ file server | Warehouse Switch → Warehouse Firewall → Site-to-Site VPN Tunnel → HQ Firewall → HQ Switch → Server |
| Remote sales rep can't connect | Client → Internet → AnyConnect Remote VPN → HQ Firewall |
| VPN connects but no internal access | Check split-tunnel/access group assignment — see `sop/SOP-002-vpn-account-provisioning.md` |

## IP Addressing (reference)

| Site | Subnet | VLANs |
|---|---|---|
| Toronto HQ | 10.10.0.0/22 | 10 (Corp), 20 (Wi-Fi), 30 (Servers) |
| Mississauga Warehouse | 10.20.0.0/24 | 10 (Corp) |
| VPN Client Pool | 10.99.0.0/24 | — |
