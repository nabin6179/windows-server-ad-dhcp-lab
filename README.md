# Windows Server 2022 – Active Directory, Group Policy & DHCP Lab

A hands-on infrastructure lab built on **Windows Server 2022**, covering server identity management via PowerShell, host-based firewall hardening, **Active Directory Domain Services (AD DS)** deployment, **Group Policy (GPO)** enforcement, and **DHCP** configuration for dynamic client addressing.

The goal of this lab was to simulate a small enterprise network environment end-to-end — from a fresh server build to a domain-joined client automatically receiving IP configuration and being subject to centrally managed security policy.

## Lab Environment

| Component | Detail |
|---|---|
| Domain Controller | Windows Server 2022 — `Nabin-2022` |
| Domain | `Nabin.local` |
| Client OS | Windows (domain-joined) |
| Hypervisor | VMware virtual machines |
| Core Roles | AD DS, DHCP Server |
| Management Tools | Server Manager, Active Directory Users and Computers, Group Policy Management Console, Windows Defender Firewall with Advanced Security |

## Architecture Overview

```
                     ┌────────────────────────────┐
                     │   Windows Server 2022       │
                     │   Nabin-2022                │
                     │                              │
                     │  • AD DS (Domain Controller)│
                     │  • DNS                       │
                     │  • DHCP Server               │
                     │  • Domain: Nabin.local       │
                     └───────────┬──────────────────┘
                                 │
                     ┌───────────┴──────────────────┐
                     │      Local Network Segment    │
                     │      192.168.10.0/24          │
                     └───────────┬──────────────────┘
                                 │
                     ┌───────────┴──────────────────┐
                     │   Windows Client VM           │
                     │   (obtains IP via DHCP,        │
                     │    joined to Nabin.local)      │
                     └────────────────────────────────┘
```

## Tasks Completed

### Task 1 — Server Identity & Host Firewall
- Renamed the server from its default install name to **`Nabin-2022`** using PowerShell (`Rename-Computer`) rather than the GUI, reflecting standard sysadmin practice for repeatable, scriptable configuration.
- Verified the new hostname post-reboot via `$env:COMPUTERNAME`.
- Configured **Windows Defender Firewall with Advanced Security** to permit inbound **ICMPv4 Echo Request** traffic (Domain/Private/Public profiles), enabling connectivity testing and troubleshooting.
- Verified reachability with a successful ping test from a peer host.

📁 [`screenshots/task1-server-identity-firewall/`](./screenshots/task1-server-identity-firewall) · 📄 [Full write-up](./docs/task1-server-identity-firewall.md)

### Task 2 — Active Directory Domain Services & Group Policy
- Installed the **AD DS** role via Server Manager and promoted the server to a Domain Controller, creating a new forest: **`Nabin.local`**.
- Set a DSRM (Directory Services Restore Mode) password for disaster-recovery access.
- Created an **Organizational Unit** (`Department_IT`) to logically scope IT department objects.
- Created a domain user (`Nabin_User`) inside the OU with **"User must change password at next logon"** enforced.
- Built a **GPO** (`Security_Lockdown`) linked to `Department_IT` that disables Command Prompt access for users in that OU — a practical example of least-privilege endpoint hardening via centralized policy rather than local configuration.

📁 [`screenshots/task2-active-directory-gpo/`](./screenshots/task2-active-directory-gpo) · 📄 [Full write-up](./docs/task2-active-directory-gpo.md)

### Task 3 — DHCP Configuration & Dynamic Addressing
- Installed the **DHCP Server** role and authorized it in Active Directory so it's trusted to lease addresses domain-wide.
- Created scope **`Client_Scope`**: `192.168.10.50` – `192.168.10.100` / `255.255.255.0`.
- Configured **Option 003 (Router)** and **Option 006 (DNS Server)** so clients automatically receive gateway and DNS information pointing back to the domain controller — required for domain logon, AD communication, and Group Policy processing.
- Verified end-to-end on a client VM using `ipconfig /renew` and `ipconfig /all`, confirming correct IP, subnet mask, gateway, and DNS assignment from the scope.

📁 [`screenshots/task3-dhcp-configuration/`](./screenshots/task3-dhcp-configuration) · 📄 [Full write-up](./docs/task3-dhcp-configuration.md)

## Skills Demonstrated
- Windows Server 2022 deployment and administration
- PowerShell-based system configuration
- Active Directory Domain Services (forest/domain creation, OU design, user provisioning)
- Group Policy Object creation, linking, and scoping
- Host-based firewall rule configuration (Windows Defender Firewall with Advanced Security)
- DHCP server deployment, scope design, and option configuration
- Client-side network verification and troubleshooting (`ipconfig`, `ping`)

## Notes
This lab was built in an isolated virtual environment for learning purposes and does not represent a production network. Screenshots are organized by task under [`screenshots/`](./screenshots), and detailed step-by-step write-ups (including exact commands and configuration values used) are in [`docs/`](./docs).
