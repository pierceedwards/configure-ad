<p align="center">
  <img src="https://i.imgur.com/pU5A58S.png" alt="Microsoft Active Directory Logo"/>
</p>

<h1 align="center">On-Premises Active Directory Deployed in the Cloud (Azure)</h1>

<p align="center">
  This tutorial outlines the deployment and configuration of an on-premises style Active Directory setup using Azure Virtual Machines.
</p>

---

## 🧰 Environments and Technologies Used

- **Microsoft Azure** – Virtual Machines and Virtual Networking  
- **Remote Desktop Protocol (RDP)** – VM access  
- **Active Directory Domain Services** – Domain and user management  
- **PowerShell** – Scripting and automation

## 💻 Operating Systems Used

- **Windows Server 2022** (for Domain Controller)
- **Windows 10 (21H2)** (for Client Machine)

---

## 🔄 High-Level Deployment Steps

- Deploy two Azure VMs (Domain Controller and Client)
- Configure Virtual Network and DNS
- Install and configure Active Directory Domain Services
- Promote server to Domain Controller
- Connect client to the domain
- Create organizational units, users, and groups

---

## 🚀 Detailed Deployment and Configuration Steps

### Step 1: Create Azure VMs

- Deploy two virtual machines:  
  - `DC-1` (Windows Server 2022) – Domain Controller  
  - `Client-1` (Windows 10) – Domain-joined client
- Ensure both VMs are in the **same Virtual Network**.

![VM Setup](https://github.com/user-attachments/assets/09dbeab3-4113-411e-a8b2-a6d33f0a07fb)

- Check that the `DC-1` and `Client-1` IPs are static/private within the subnet.

![VM Network](https://github.com/user-attachments/assets/364fe9d0-1467-4902-a8bd-830ab5bf98cc)

---

### Step 2: Update DNS for Client-1

Set `Client-1`'s DNS server to point to `DC-1`'s **private IP address**:

- Go to **Azure portal > Client-1 > Networking > DNS Servers**  
- Select **Custom** and enter `DC-1`'s IP (e.g., `10.0.0.4`)

![DNS Config](https://github.com/user-attachments/assets/d6d26e15-994e-4dda-b7d3-78f752ced38c)

Verify settings from within `Client-1`:

### Step 3: Promote DC-1 to a Domain Controller

- Log into DC-1 and open Server Manager

- Select Add Roles and Features > Active Directory Domain Services

- Complete the wizard and Install



### Step 4: Create Organizational Units and Domain Admin

- Open Active Directory Users and Computers

- Create OUs: _EMPLOYEES and _ADMINS
- Create a domain admin:
- Right-click _ADMINS > New > User
- Assign to "Domain Admins" group via Properties > Member of


### Step 5: Join Client-1 to Domain

- On Client-1: Settings > System > About > Rename this PC (advanced)
- Select Domain, enter your domain (e.g., mydomain.com)
- Check success from DC-1:
- Open Active Directory Users and Computers
- Navigate to Computers OU and confirm Client-1 is listed


### Step 6: Create Multiple Users via PowerShell
- Log in to DC-1, open PowerShell ISE as Admin
- Create script file: File > New, paste user creation script
- Run the script to auto-generate users under _EMPLOYEES

⚠️ Make sure the OU name in the script matches the one created


### 🧪 Final Notes
- A cloud-hosted domain controller can fully replicate on-prem environments
- AD setup in Azure is ideal for testing, training, and hybrid deployments
- Always configure proper DNS and security group rules for reliable domain comms




