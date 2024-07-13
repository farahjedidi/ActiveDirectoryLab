# Active Directory Lab


![Lab Architecture](https://i.imgur.com/dUrbQrj.png)
## Introduction

In this project, I set up an Active Directory lab environment using VirtualBox. The lab consists of a domain controller (Server 2019) and a client machine (Windows 10). The main goal is to configure and manage an AD DS environment.

## Setup Steps

### Requirements

1. VirtualBox
2. Windows Server 2019 ISO
3. Windows 10 ISO

### Network Configuration

- **Domain Controller (Server 2019)**
  - NIC 1: Internet (DHCP, gets address automatically from home router)
  - NIC 2: Internal Network (Static IP: 172.16.0.1, Mask: 255.255.255.0, DNS: 127.0.0.1)

- **Client Machine (Windows 10)**
  - NIC: Internal Network (DHCP, gets address from DC)

### Domain Controller Setup

1. **Configure NIC Settings**
   - **Purpose**: Setting a static IP for the internal NIC ensures that the domain controller has a fixed address within the internal network, which is necessary for reliable communication and services.
   - **Steps**: 
     - Access the network settings and configure NIC 2 (Internal) to use the static IP: 172.16.0.1.
     - ![Screenshot]()

2. **Install Active Directory Domain Services (AD DS) Role**
   - **Purpose**: AD DS is a server role in Windows Server that allows you to create a scalable, secure, and manageable infrastructure for user and resource management.
   - **Steps**: 
     - Open Server Manager, add roles and features.
     - Select Active Directory Domain Services.
     - Proceed with the installation and initial configuration.
     - ![Screenshot]()

3. **Promote Server to Domain Controller**
   - **Purpose**: Promoting the server to a domain controller is essential to manage and provide authentication services in the domain.
   - **Steps**: 
     - After installing AD DS, follow the steps to promote the server to a domain controller.
     - Create a new forest with the Fully Qualified Domain Name (FQDN): `mydomain.com`.
     - Configure necessary DNS settings and additional options as per the wizard.
     - Complete the wizard and restart the server to apply changes.
     - ![Screenshot]()

4. **Configure DHCP**
   - **Purpose**: DHCP (Dynamic Host Configuration Protocol) allows the domain controller to assign IP addresses to devices within the internal network dynamically.
   - **Steps**: 
     - Add the DHCP role via Server Manager.
     - Configure a DHCP scope for the internal network (IP range: 172.16.0.100-200).
     - Set the gateway to 172.16.0.1 and DNS to 172.16.0.1 to ensure proper network configuration for clients.
     - ![Screenshot]()

### Client Machine Setup

1. **Configure NIC Settings**
   - **Purpose**: Configuring the NIC to use the internal network ensures that the client can communicate with the domain controller and receive an IP address via DHCP.
   - **Steps**: 
     - Set the NIC to use the internal network.
     - Ensure it is set to obtain an IP address automatically from the DHCP server (DC).
     - ![Screenshot]()

2. **Join Client to Domain**
   - **Purpose**: Joining the client machine to the domain allows it to be managed centrally by the domain controller and to utilize domain resources.
   - **Steps**: 
     - Open System Properties, change settings to join the domain `mydomain.com`.
     - Provide domain admin credentials when prompted.
     - Restart the client machine to apply changes.
     - ![Screenshot]()

### Creating Users with PowerShell

1. **Create Organizational Units and Users**
   - **Purpose**: Automating the creation of Organizational Units (OUs) and users with PowerShell saves time and ensures consistency in large environments.
   - **Steps**: 
     - Use the following PowerShell script to create an OU and multiple users:
     ```powershell
     
     ```
     - This script creates an Organizational Unit named "LabUsers" and 1000 user accounts within that OU.
     - ![Screenshot]()

## Conclusion

This setup creates a secure, isolated internal network where the DC provides domain services and DHCP addresses to the client. The RAS/NAT configuration allows the internal network to communicate with the external internet while maintaining network security. This architecture is ideal for practicing domain management and network administration in a controlled lab environment.


