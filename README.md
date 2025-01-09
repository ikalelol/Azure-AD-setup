# Active Directory in Azure Virtual Machines

This repository documents the process of setting up and managing an Active Directory (AD) environment within Azure Virtual Machines (VMs). This project is a step-by-step guide, including screenshots and insights based on personal experience.

---

## Overview

- **Purpose**: Centralize domain and user management using Active Directory.
- **Environment**: Azure VMs with Windows Server 2022 and Windows 10.
- **Technologies Used**:
  - Microsoft Azure
  - Active Directory Domain Services (ADDS)
  - Remote Desktop Protocol (RDP)
  - PowerShell

---

## Deployment Steps

### 1. Create Azure Resources
#### Domain Controller (DC)
- Create a Windows Server 2022 Datacenter VM.
- Admin credentials:
  ```
  Username: kaleadmin
  Password: NoTSecU3rPa$
  ```
- Use default settings.
DC VM Setup
![image](https://github.com/user-attachments/assets/571462b1-85f4-4750-9e8c-6f89d3964d3b))
VM Overview
![image](https://github.com/user-attachments/assets/4bbc6090-951d-4dc3-8b68-7fe42fa857a5))

#### Client VM
- Create a Windows 10 VM in the same resource group and virtual network (VNet) as the DC.
- Client VM Setup
- [image](https://github.com/user-attachments/assets/489a7825-507b-48fd-8e4b-57dc44c591c8))

### 2. Network Configuration
- Assign a static private IP to the Domain Controller for consistent connectivity.
![Static IP Configuration](![image](https://github.com/user-attachments/assets/5d4e3396-a133-4246-a0ad-225708b90a56))
- Ensure both VMs are in the same VNet and subnet.

### 3. Accessing the Server
- Use RDP from a local machine (Windows 10/11) with the Azure-assigned public IP or download the RDP file.
![RDP Connection](![image](https://github.com/user-attachments/assets/0fa58081-6eb2-4b90-abe8-470a33745e95))

### 4. Install Active Directory Domain Services (ADDS)
- Open **Server Manager** and add the ADDS role via **Add Roles and Features**.
![Add Roles and Features](![image](https://github.com/user-attachments/assets/d78d9961-93c3-4b67-a557-56c0586019c7))

- Promote the server to a Domain Controller:
  - Click the notification flag (top-right corner).
  - Configure a new forest (e.g., `gitkale.com`).
  - Use default settings.
![Promote to Domain Controller](![image](https://github.com/user-attachments/assets/26b3b516-7275-4ab7-b758-e96a71b29e20))
![New Forest Setup](![image](https://github.com/user-attachments/assets/4620e911-382c-44f2-a244-8fad2c933d0e))

### 5. Create Users and Organizational Units (OUs)
- Open **Active Directory Users and Computers (ADUC)**.
- Create:
  - OUs: `_Admins`, `_Employees`
  - Users:
    - `Kale_Admin` (Domain Admins group)
    - `Kale` (normal user)

### 6. Join Client VM to the Domain
- Verify both VMs are in the same VNet and subnet.
![VNet Topology](![image](https://github.com/user-attachments/assets/bd776c65-e5c6-422f-bc16-09ecfcd45178))

- From the Client VM:
  - Login via RDP.
  - Navigate to **System > Rename This PC (Advanced)**.
  - Change the domain to `gitkale.com`.
![Join Domain](![image](https://github.com/user-attachments/assets/91c05b4f-3167-48f7-a209-5b48f2d1b9ef))


- Restart the Client VM, reconnect, and log in with domain accounts.

### 7. Additional Configuration
- Create additional users using [this PowerShell script](https://github.com/Xinloiazn/configure-ad/blob/main/adscript.ps1).
- Set up Group Policy Objects (GPOs):
  - Restrict disabling the firewall and antivirus.
  - Disable auto-run from USB devices.

---

## Tips and Insights
- **Segregation of Duties**: Use separate accounts for administrative tasks (`Kale_Admin`) and everyday use (`Kale`).
- **Cleanup**: Delete Azure resources after testing to avoid unnecessary charges.

---

## Reference Repositories
- [Joeljjoseph1998/configure-ad](https://github.com/joeljjoseph1998/configure-ad)
- [Xinloiazn/configure-ad](https://github.com/Xinloiazn/configure-ad)

---

## Acknowledgments
This project helped me deepen my understanding of Active Directory and its implementation within Azure environments. It's been valuable for both personal learning and professional use.
