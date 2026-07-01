# 🖥️ Windows Server Home Lab — Active Directory & Group Policy

Part of my [project portfolio](https://github.com/Th3Slime).

## 📌 Overview
A from-scratch Windows Server home lab covering the core responsibilities of a Systems/Windows Administrator: standing up a domain, structuring it with OUs and groups, enforcing security through Group Policy, controlling file access correctly, and hardening the environment. Built entirely in VMs on my own hardware.

## 🎯 Objectives
- Build a working Active Directory domain from a bare Windows Server install
- Enforce security and configuration standards at scale using GPOs
- Control file/folder access correctly (NTFS + share permissions together, not just one)
- Practice the kind of administrative change a real sysadmin ticket would ask for

## 🛠 Environment
Windows Server (VM, promoted to Domain Controller) · Windows 10/11 Pro/Enterprise client VM(s) · AD DS · GPMC · FSRM

---

## Phase 1: Core Active Directory Setup

**1.1 Windows Server Setup**
- Install Windows Server on VM, configure static IP, set computer name, install updates

**1.2 Domain Controller Promotion**
```powershell
# Install AD DS role
Install-WindowsFeature -Name AD-Domain-Services -IncludeManagementTools

# Promote to Domain Controller
Install-ADDSForest -DomainName "example.local"
```

**1.3 Organizational Structure**
- Created OUs: IT, HR, Sales, Marketing
- Created user accounts in respective OUs, security groups, and group membership

## Phase 2: Group Policy Implementation

**2.1 GPO Management Setup** — installed GPMC, created and linked GPOs to appropriate OUs

**2.2 GPO Activities**
| Activity | What it does |
|---|---|
| Password Policy | Enforce strong passwords — min length 8, complexity required, max age 90 days |
| Drive Mapping | Auto-map network drives via Group Policy Preferences, targeted to specific groups |
| Desktop Wallpaper | Default corporate wallpaper per department, user modification blocked |
| Control Panel Restriction | Disabled for standard users, allowed for specific admin accounts |
| USB Storage Restriction | USB mass storage disabled by default, approved devices allowed via device ID |

## Phase 3: Client Configuration
- Built Windows 10/11 client VM, joined to domain
- **Testing methodology:** created test users per OU, logged in as each, verified policy application with `gpresult /r`, tested restrictions directly rather than trusting GPMC alone

## Phase 4: Network File Sharing

**4.1 File Server Setup** — shared folders built to real access-control scenarios:
| Folder | Access rule |
|---|---|
| Marketing | Read-only for interns |
| HR | HR staff only, invisible to everyone else |
| VendorFiles | Vendor write access, no visibility into anything else |
| Software | IT read access |
| Licenses | Senior IT only (subfolder of Software) |

**4.2 Permission Management** — NTFS permissions + share permissions configured together, least privilege enforced

**4.3 FSRM Implementation** — installed File Server Resource Manager, configured quota templates, file screening, and storage reports

## Phase 5: Advanced Security Policies
- **Account Policies:** password policy, account lockout (5 attempts / 30-min lockout), Kerberos policy
- **User Rights Assignment:** logon rights, system privileges, security options
- **Fine-Grained Password Policies (PSOs):** stricter requirements for admin groups, standard policy for regular users — a single domain-wide password policy can't do this; PSOs are the correct tool

## Phase 6: Service Accounts & Automation
- Dedicated service accounts with restricted logon rights
- Auto-logon configured for a kiosk scenario: browser auto-starts full-screen on boot, session persists

## Phase 7: Access-Based Enumeration (ABE)
```powershell
# Enable ABE on shared folder
Set-SmbShare -Name "Marketing" -FolderEnumerationMode AccessBased
```
Tested that users only see folders they actually have access to, across different security groups, and validated permission inheritance.

## 🧰 Monitoring & Troubleshooting — Useful Commands
```powershell
gpresult /h report.html                                    # Group Policy Results
gpupdate /force                                             # Force Group Policy Update
nslookup example.local                                      # DNS Verification
Test-ComputerSecureChannel                                  # Domain Connectivity
Get-GPOReport -All -ReportType HTML -Path C:\GPOReport.html  # GPO Reporting
```

## 🔍 Key Findings / Lessons Learned
- NTFS and share permissions are evaluated together — the more restrictive one wins. Getting scenarios like "vendor can upload but not browse" right meant deliberately separating share-level and NTFS-level permissions.
- Fine-grained password policies (PSOs) are the correct way to run different password rules for admins vs. standard users.
- GPOs need to be verified against an actual domain-joined client (`gpresult /r`, re-login) — reviewing them in GPMC alone isn't proof they work.

## 📸 Evidence
*(Add screenshots here: AD Users & Computers OU structure, GPMC policy list, `gpresult` output, NTFS/share permission dialogs, ABE in action)*

## 📚 Skills Demonstrated
Active Directory administration · Group Policy management · NTFS & share permission design · FSRM · Access-Based Enumeration · account lockout / password policy hardening · service account configuration · least-privilege access control

## 🔗 Related Projects
- [Network-Scanner-with-nmap](https://github.com/Th3Slime/Network-Scanner-with-nmap)
- [Password-Auditing-with-John-the-Ripper](https://github.com/Th3Slime/Password-Auditing-with-John-the-Ripper)
- [Firewall-Configuration-with-UFW-Uncomplicated-Firewall-](https://github.com/Th3Slime/Firewall-Configuration-with-UFW-Uncomplicated-Firewall-)
