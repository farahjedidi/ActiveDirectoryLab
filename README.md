# Active Directory Lab

This repository contains the scripts and documentation for setting up an Active Directory Lab as illustrated in the provided architecture diagram.

![Lab Architecture](Architecture/ARCHITECTURE.png)

## Introduction

In this project, I set up an Active Directory lab environment using VirtualBox. The lab consists of a domain controller (Server 2019) and a client machine (Windows 10). The main goal is to configure and manage an AD DS environment.

## Setup Steps

### Requirements

1. VirtualBox
2. Windows Server 2019 ISO
3. Windows 10 ISO

### Network Configuration

- **Domain Controller (Server 2019)**
  - NIC 1: Internet (DHCP, gets address from home router)
  - NIC 2: Internal Network (Static IP: 172.16.0.1, Mask: 255.255.255.0, DNS: 127.0.0.1)

- **Client Machine (Windows 10)**
  - NIC: Internal Network (DHCP, gets address from DC)

### Domain Controller Setup

1. **Configure NIC Settings**
   - Set NIC 2 (Internal) to a static IP: 172.16.0.1
   - ![Screenshot](path/to/screenshot1.png)

2. **Install Active Directory Domain Services (AD DS) Role**
   - Open Server Manager, add roles and features.
   - Select Active Directory Domain Services.
   - Install and configure.
   - ![Screenshot](path/to/screenshot2.png)

3. **Promote Server to Domain Controller**
   - After installing AD DS, promote the server to a domain controller.
   - Add a new forest with the FQDN: `mydomain.com`.
   - Configure DNS and additional options.
   - Complete the wizard and restart.
   - ![Screenshot](path/to/screenshot3.png)

4. **Configure DHCP**
   - Add the DHCP role via Server Manager.
   - Configure a DHCP scope for the internal network (172.16.0.100-200).
   - Set gateway to 172.16.0.1 and DNS to 172.16.0.1.
   - ![Screenshot](path/to/screenshot4.png)

### Client Machine Setup

1. **Configure NIC Settings**
   - Set the NIC to use the internal network.
   - Obtain IP automatically from the DHCP server (DC).
   - ![Screenshot](path/to/screenshot5.png)

2. **Join Client to Domain**
   - Open System Properties, change settings to join the domain `mydomain.com`.
   - Provide domain admin credentials.
   - Restart the machine.
   - ![Screenshot](path/to/screenshot6.png)

### Creating Users with PowerShell

1. **Create Organizational Units and Users**
   - Use PowerShell scripts to automate the creation of OUs and users.
   - Example script:
     ```powershell
     # Create an OU
     New-ADOrganizationalUnit -Name "LabUsers" -Path "DC=mydomain,DC=com"

     # Create users
     for ($i=1; $i -le 1000; $i++) {
       $Username = "User$i"
       $Password = ConvertTo-SecureString "P@ssw0rd!" -AsPlainText -Force
       New-ADUser -Name $Username -AccountPassword $Password -PasswordNeverExpires $true -Enabled $true -Path "OU=LabUsers,DC=mydomain,DC=com"
     }
     ```
   - ![Screenshot](path/to/screenshot7.png)

## Conclusion

In this project, an Active Directory lab was set up using VirtualBox with a domain controller and a client machine. The lab demonstrates configuring AD DS, setting up DHCP, and automating user creation with PowerShell.

Feel free to explore and modify the setup to fit your learning objectives. Contributions and suggestions are welcome!

# Setting Up an Active Directory Home Lab

## Introduction
In this project, I set up a fully functional Active Directory environment using Oracle VirtualBox. By the end of this lab, I will have a domain controller and a client machine in a virtualized environment, which will help me understand Active Directory and Windows networking. The lab will include the following steps:

- Setting up the domain controller
- Configuring Active Directory Domain Services
- Creating user accounts using PowerShell
- Setting up the client machine
- Joining the client machine to the Active Directory domain
Steps to Set Up the Lab

## Lab Environment Architecture
![Architecture Diagram](https://i.imgur.com/dUrbQrj.png)

## Step 1: Set Up the Domain Controller


## Step 2: Configure Active Directory Domain Services
This is a critical step in setting up the Active Directory environment. Active Directory Domain Services (AD DS) is the core component of Active Directory, providing the methods for storing directory data and making this data available to network users and administrators.

## Step 3: Create User Accounts

## Step 4: Set Up the Client Machine

## Step 5: Join the Client to the Domain
Joining the client machine to the Active Directory domain is essential for centralized management and authentication.

## Conclusion
In this project, I set up a basic Active Directory lab environment with a domain controller and a client machine using Oracle VirtualBox. This setup can be used to practice and understand various Active Directory and networking concepts.
