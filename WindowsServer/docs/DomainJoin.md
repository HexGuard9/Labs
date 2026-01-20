# Joining Windows Clients to Domain - Lab Setup

This document describes the steps I followed to join Windows 11 clients to the Active Directory domain hosted on Windows Server.

---

## 1. Lab Overview

| Machine             | Role / Notes                           |
|--------------------|----------------------------------------|
| **WindowsServer**   | Domain Controller + DHCP + DNS server  |
| **Windows11** | Client 1 (already installed Windows)  |
| **Windows11 2** | Client 2 (pre-install snapshot)      |

> All clients use static IP addresses for initial configuration, pointing to the WindowsServer as their DNS.

---

## 2. Joining ClientWindows11 (installed Windows)

1. Log in as local administrator.  
2. Open **Settings → System → About → Rename this PC / Join a domain**.  
3. Click **Join a domain**.  
4. Enter the **Domain Name**: `yourdomain.local`  
5. Click **Next**.  
6. Enter **domain credentials** (administrator account on WindowsServer).  
7. After successful authentication, a message confirms the PC has joined the domain.  
8. Click **Restart now** to apply changes.  

> After reboot, login screen allows choosing **domain account** instead of local account.

---

## 3. Joining ClientWindows11 2 (pre-install / OOBE)

1. Boot the machine to **Out-Of-Box Experience (OOBE)**.  
2. Select **Region, Keyboard Layout, and License Agreement** as usual.  
3. When prompted for **Account Setup**:
   - Choose **Set up for work or school** (or similar option in your Windows version).  
   - Enter **domain name**: `yourdomain.local`.  
   - Enter **domain credentials** (admin account from server).  
4. Complete setup steps (skip creating local Microsoft account if prompted).  
5. Machine automatically joins domain during initial setup.  
6. Upon first login, Windows configures **domain profile** and network settings.  

> This method is useful for machines that have not been installed yet, or after reverting a snapshot to pre-install state.

---

## 4. Notes

- Clients must have **DNS pointing to the Domain Controller** before joining the domain.  
- Static IPs are recommended for lab environments to avoid DHCP conflicts.  
- For already installed Windows, domain join can also be done via:
  ```powershell
  Add-Computer -DomainName "yourdomain.local" -Credential (Get-Credential) -Restart
