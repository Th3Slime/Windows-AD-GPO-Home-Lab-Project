1.1 Windows Server Setup
Install Windows Server on VM

Configure static IP address

Set computer name

Install updates

1.2 Domain Controller Promotion
# Install AD DS role
Install-WindowsFeature -Name AD-Domain-Services -IncludeManagementTools

# Promote to Domain Controller
Install-ADDSForest -DomainName "example.local"

1.3 Organizational Structure
Create OUs: IT, HR, Sales, Marketing

Create user accounts in respective OUs

Create security groups

Set up group membership

Phase 2: Group Policy Implementation
2.1 GPO Management Setup
Install Group Policy Management Console (GPMC)

Create and link GPOs to appropriate OUs

2.2 GPO Activities
Activity 1: Password Policy
Enforce strong passwords

Minimum password length: 8 characters

Password complexity requirements

Maximum password age: 90 days

Activity 2: Drive Mapping
Map network drives automatically

Use Group Policy Preferences

Target specific user groups

Activity 3: Desktop Wallpaper
Set default corporate wallpaper

Apply to specific departments

Prevent user modification

Activity 4: Control Panel Restriction
Disable Control Panel access

Allow specific administrative users

Activity 5: USB Storage Restriction
Disable USB mass storage

Allow approved devices via device ID

Phase 3: Client Configuration
3.1 Windows Client Setup
Create Windows 10/11 VM

Join to domain

Verify GPO application

3.2 GPO Testing Methodology
Create test users in each OU

Log in with test accounts

Verify GPO application

Use gpresult /r to check policies

Test restrictions and configurations

Phase 4: Network File Sharing
4.1 File Server Setup
Install File Server role

Create shared folders:

Marketing (Read-only for interns)

HR (HR staff only)

VendorFiles (Vendor write access)

Software (IT read access)

Licenses (Senior IT only)

4.2 Permission Management
Configure NTFS permissions

Set share permissions

Implement least privilege access

4.3 FSRM Implementation
Install File Server Resource Manager

Configure quota templates

Set up file screening

Create storage reports
Phase 5: Advanced Security Policies
5.1 Account Policies
Password Policy configuration

Account Lockout Policy (5 attempts, 30-minute lockout)

Kerberos Policy settings

5.2 User Rights Assignment
Configure logon rights

Set system privileges

Implement security options

5.3 Fine-Grained Password Policies
Create PSO for administrative accounts

Apply stricter requirements to admin groups

Maintain standard policies for regular users
Phase 6: Service Accounts & Automation
6.1 Service Account Configuration
Create dedicated service accounts

Configure logon restrictions

Set up auto-logon for kiosk scenarios

6.2 Application Automation
Configure browser auto-start

Set full-screen mode

Implement session persistence
Phase 7: Access-Based Enumeration
7.1 ABE Implementation
# Enable ABE on shared folder
Set-SmbShare -Name "Marketing" -FolderEnumerationMode AccessBased
7.2 Testing Scenarios
Verify users only see accessible folders

Test with different security groups

Validate permission inheritance

Monitoring and Troubleshooting
Useful Commands
# Group Policy Results
gpresult /h report.html

# Group Policy Update
gpupdate /force

# DNS Verification
nslookup example.local

# Domain Connectivity
Test-ComputerSecureChannel

# GPO Reporting
Get-GPOReport -All -ReportType HTML -Path C:\GPOReport.html
