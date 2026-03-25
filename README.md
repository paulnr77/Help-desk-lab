# Help-desk-lab
My help-desk lab environment 
# Help Desk Home Lab

This project documents my help-desk and system administration home lab built on Proxmox using a Dell PowerEdge R610. The lab was designed to simulate a small enterprise environment and develop practical skills in Active Directory, DNS, Group Policy, domain-joined client management, and common help-desk troubleshooting.

## Project Goals

- Build a realistic help-desk lab environment
- Deploy and manage Windows Server 2019
- Configure Active Directory Domain Services and DNS
- Join a Windows client to the domain
- Create users, groups, and Organizational Units
- Apply Group Policy Objects
- Practice common help-desk scenarios such as account lockouts, password resets, DNS issues, and GPO troubleshooting
- Document troubleshooting and lessons learned in a professional portfolio format

## Lab Environment

### Hardware
- Dell PowerEdge R610
- Windows PC with dual NICs
- Cisco router
- Windows laptop used as domain client

### Software and Platforms
- Proxmox VE
- Windows Server 2019 Standard Evaluation (Desktop Experience)
- Active Directory Domain Services
- DNS
- Group Policy Management
- Windows domain-joined client

## High-Level Architecture
-Air gapped lab for better security of my home network
- NIC 1 on the Windows PC connected to the home network
- NIC 2 on the Windows PC connected to the lab network
- Proxmox host connected to the lab router
- Windows Server 2019 deployed as `DC01`
- `DC01` configured as Domain Controller and DNS server
- Physical Windows laptop joined to the domain as a client

## Core Components

- **Proxmox Host**: virtualization platform running on Dell R610
- **DC01**: Windows Server 2019 Domain Controller
- **DNS**: hosted on DC01 for domain name resolution
- **Domain**: `lab.local`
- **Client**: physical Windows laptop joined to the domain
- **AD Structure**: Users, Groups, Computers, and Servers OUs
- **GPOs**: password policy, account lockout policy, user restrictions, and drive mapping

## Skills Demonstrated

- Hypervisor and VM deployment
- Windows Server installation and configuration
- Active Directory and DNS deployment
- Domain join and client management
- Group and user administration
- Group Policy configuration
- Troubleshooting authentication, DNS, and VM startup issues
- Help-desk style issue resolution

## Key Troubleshooting Highlights

### 1. KVM virtualization not available
- Error: `KVM virtualisation configured, but not available`
- Cause: virtualization disabled in BIOS
- Fix: enabled virtualization in Dell R610 BIOS

### 2. QEMU exited with code 1
- Error related to unsupported CPU AES feature
- Cause: older host CPU did not support requested CPU feature set
- Fix: changed VM CPU model to `kvm64` and removed TPM

### 3. Groups not appearing in AD searches
- Cause: groups were initially created in the wrong context
- Fix: recreated groups correctly in Active Directory and verified with PowerShell

### 4. Domain unavailable during client login
- Cause: DNS misconfiguration on the domain controller
- Fix: changed DC DNS from `::1` to `127.0.0.1`, disabled IPv6, restarted DNS and Netlogon, and re-registered SRV records

### 5. SRV records missing
- Cause: DNS registration issue
- Fix: used `ipconfig /registerdns` and `nltest /dsregdns` to restore AD DNS records

## Validation

Examples of validation commands used in the lab:

```powershell
ipconfig /all
nslookup lab.local
nslookup -type=SRV _ldap._tcp.dc._msdcs.lab.local
nltest /dsgetdc:lab.local
gpresult /r
whoami /groups
dcdiag
