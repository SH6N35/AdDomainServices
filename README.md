## Active Directory Domain Services (AD DS), DNS, & DHCP Setup

## 🎯 Objective
Provision, configure, and validate an enterprise-grade on-premises identity and core network services infrastructure using Windows Server and a virtualized homelab environment.

---

## 🛠️ Environment & Architecture
- **Hypervisor:** Ubuntu Linux (KVM / QEMU) managed via Cockpit (VirtIO drivers for storage and networking)
- **Operating System:** Windows Server 2022 (Desktop Experience)
- **Domain Name:** `ApexTech.com` (`homelab.local`)
- **Server Static IP:** `192.168.122.22`

---

## 📋 Implementation Steps

### 1. Provisioning the Virtual Machine
- Deployed a new VM in KVM with **4 GB RAM**, 2 vCPUs, and a dedicated virtual disk using VirtIO storage controllers.
- Attached the Windows Server ISO and configured the network bridge interface.

### 2. Promoting the Domain Controller & DNS Configuration
- Installed the **Active Directory Domain Services (AD DS)** role via Server Manager.
- Promoted the server to a Domain Controller, creating a new forest named `ApexTech.com`.
- Ensured the **Domain Name System (DNS) server** role was automatically installed and integrated with Active Directory to handle domain lookups and service records.

### 3. Configuring the DHCP Server Role
- Installed the **DHCP Server** role to automate IP address leasing for future client VMs.
- Authorized the DHCP server in Active Directory.
- Created an active IPv4 scope (`192.168.122.100` to `192.168.122.200`) and configured the default gateway and DNS server options pointing back to the Domain Controller.

### 4. Post-Promotion Housekeeping & OU Structure
- Updated the Domain Controller's IPv4 network properties to point its **Preferred DNS Server** to loopback (`127.0.0.1`), ensuring it queries its own local authoritative zones.
- Opened **Active Directory Users and Computers (`dsa.msc`)** and established an enterprise Organizational Unit (OU) structure:
  - Root OU: `Homelab`
  - Sub-OUs: `Workstations`, `Servers`, `Users`

---

## ✅ Verification & Testing

1. **Verify DNS SRV Records:**
   Opened **DNS Manager (`dnsmgmt.msc`)** and verified that the `_msdcs`, `_ldap`, and `_kerberos` service records populated successfully under `ApexTech.com`.

2. **Test Name Resolution:**
   Ran `nslookup apextech.com` and confirmed the server resolved correctly to its static IP address.

3. **Verify DHCP Leases:**
   Checked the DHCP Management Console to confirm active scope bindings and authorization status.

---

## 📸 Lab Evidence
### Server Manager & Installed Roles
![Server Manager Dashboard](<Server Dash.png>)

### Active Directory Organizational Units (OUs)
![Active Directory OU Structure](<AD users.png>)

### DNS Manager & Forward Lookup Zone
![DNS Forward Lookup Zone](<DNS manager.png>)

### Static IP & Loopback DNS Configuration
![IPv4 Network Properties](<IPconfig.png>)
