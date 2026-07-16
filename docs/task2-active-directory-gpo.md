# Task 2: Active Directory Domain Services & Group Policy Management

## 1. Domain Setup and Promotion to Domain Controller

To centralize authentication and resource management, **`Nabin-2022`** was promoted to a Domain Controller:

1. Installed the **Active Directory Domain Services (AD DS)** role via Server Manager.
2. Promoted the server to a Domain Controller, creating a new forest: **`Nabin.local`**.
3. Set a **Directory Services Restore Mode (DSRM)** password to enable recovery access in case of failure.
4. Restarted the server to finalize the configuration.

![Installing AD DS role](../screenshots/task2-active-directory-gpo/01-addds-role-installation.jpeg)

## 2. Organizational Unit (OU) and User Creation

Using **Active Directory Users and Computers**:

- Created an OU named **`Department_IT`** to logically group IT department objects and simplify policy scoping.
- Created a user account **`Nabin_User`** inside that OU, with **"User must change password at next logon"** enabled for baseline account security.

![Active Directory Users and Computers console](../screenshots/task2-active-directory-gpo/02-addds-users-and-computers.jpeg)
![Creating OU Department_IT](../screenshots/task2-active-directory-gpo/03-ou-department-it-creation.jpeg)
![OU and user inside AD DS](../screenshots/task2-active-directory-gpo/04-ou-and-user-nabin-user.jpeg)

## 3. Group Policy Object (GPO) Creation and Configuration

A GPO named **`Security_Lockdown`** was created via **Group Policy Management** and linked to the `Department_IT` OU, so the policy applies only to users within that unit.

**Policy configured:**

| Field | Value |
|---|---|
| Policy Name | Prevent access to the command prompt |
| Path | User Configuration → Administrative Templates → System |
| Status | Enabled |

After the policy applied, users under `Department_IT` were no longer able to open Command Prompt — a simple but effective example of reducing the local attack surface for standard users via centralized policy instead of per-machine configuration.

![Group Policy Management console](../screenshots/task2-active-directory-gpo/05-group-policy-management-console.jpeg)
![GPO linked to Department_IT OU](../screenshots/task2-active-directory-gpo/06-gpo-linked-to-department-it-ou.jpeg)
![GPO setting to block Command Prompt access](../screenshots/task2-active-directory-gpo/07-gpo-block-cmd-policy-setting.png)
