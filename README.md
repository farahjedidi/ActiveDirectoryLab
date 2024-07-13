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
  - NIC 2: Internal Network (Static IP: 172.16.0.1, Mask: 255.255.255.0, DNS: 127.0.0.1, manual setup)

- **Client Machine (Windows 10)**
  - NIC: Internal Network (DHCP, gets address from DC)

### Domain Controller Setup

1. **Configure NIC Settings**
   - **Purpose**: Setting a static IP for the internal NIC ensures that the domain controller has a fixed address within the internal network, which is necessary for reliable communication and services.
   - **Steps**: 
     - I accessed the network settings and configured NIC 2 (Internal) to use the static IP: 172.16.0.1 & subnet mask 255.255.255.0
     - ![Screenshot](https://i.imgur.com/imILsSe.png)
     - I did not set a default gateway for the internal NIC as the domain controller serves as its own gateway.
     - For DNS, I configured it to use the loopback address (127.0.0.1), which refers to the server itself.

2. **Install Active Directory Domain Services (AD DS) Role**
   - **Purpose**: AD DS is a server role in Windows Server that allows you to create a scalable, secure, and manageable infrastructure for user and resource management.
   - **Steps**: 
     - I opened Server Manager, added roles and features & selected the server.
     -  ![Screenshot](https://i.imgur.com/ZZlr6y5.png)
     -   ![Screenshot](https://i.imgur.com/jGgs1Fz.png)
       
     - Selected Active Directory Domain Services.
     -   ![Screenshot](https://i.imgur.com/RxdwfJL.png)
       
     - Proceeded with the installation and initial configuration.
     -   ![Screenshot](https://i.imgur.com/bG3rKs5.png)

3. **Promote Server to Domain Controller**
   - **Purpose**: Promoting the server to a domain controller is essential to manage and provide authentication services in the domain.
   - **Steps**: 
     - After installing AD DS, I promoted the server to a domain controller.
     -   ![Screenshot](https://i.imgur.com/yPQz7Az.png)
       
     - Created a new forest with the Fully Qualified Domain Name (FQDN): `mydomain.com`.
     -   ![Screenshot](https://i.imgur.com/QxNpX51.png)
       
     - Completed the wizard and restarted the server to apply changes.
       
4. **Create a new dedicated domain admin account**
   - **Purpose**: By creating a new dedicated domain admin account, we follow the best practices, ensuring a more secure, manageable, and compliant Active Directory environment.
   - **Steps**:
     - I accessed Administrative Tools and opened Active Directory Users and Computers.
     - I created a new Organizational Unit named "_ADMINS" in the domain mydomain.com .
     - Inside "_ADMINS", I created a new user with the username a-fjedidi.
     -  ![Screenshot](https://i.imgur.com/fdwc0tY.png)
     - I added this user to the Domain Admins group by navigating to the Member Of tab in the user properties.
     -   ![Screenshot](https://i.imgur.com/HKs437q.png)
     - I logged out and then logged back in using the new domain admin account (a-fjedidi).
  
5. ****Install & configure RAS/NAT Role****
   - **Purpose**: RAS/NAT (Routing and Remote Access / Network Address Translation) allows client machines on a private virtual network to access the internet by routing their traffic through the domain controller. This ensures that the internal network can communicate with external networks while maintaining a private IP addressing scheme.
   - **Steps**: 
     - I accessed Add Roles and Features from Server Manager and selected Remote Access as the role to install & Routing as the role service and proceeded with the installation.
     - ![Screenshot](https://i.imgur.com/04jmZyS.png)
     - Once the role installation was complete, I opened Routing and Remote Access from the Tools menu then selected NAT to allow internal clients to connect to the internet using one address.
     - ![Screenshot](https://i.imgur.com/yTQaJff.png)
     - I ensured the public interface was selected for connecting to the internet.
     - ![Screenshot](https://i.imgur.com/a4XzyBi.png)
     - After configuration, I verified the RAS/NAT setup was successful by checking the green status indicator.
     - ![Screenshot](https://i.imgur.com/pLsUU71.png)

          
6. **Install & configure DHCP Role**
   - **Purpose**: DHCP (Dynamic Host Configuration Protocol) allows the domain controller to assign IP addresses to devices within the internal network dynamically.
   - **Steps**: 
     - I installed the DHCP role via Server Manager.
     - ![Screenshot](https://i.imgur.com/fCAGeij.png)
     - Configured a DHCP scope for the internal network (IP range: 172.16.0.100-200).
     - ![Screenshot](https://i.imgur.com/daKGka8.png)
     - Set the gateway to 172.16.0.1 and DNS to 172.16.0.1 to ensure proper network configuration for clients.
     - ![Screenshot](https://i.imgur.com/xRuAYdF.png)
     - ![Screenshot](https://i.imgur.com/A5ghx0s.png)
     - DHCP successfully configured.
     - ![Screenshot](https://i.imgur.com/fvD7Wup.png)

### Creating Users with PowerShell

1. **Create Organizational Units and Users**
   - **Purpose**: Automating the creation of Organizational Units (OUs) and users with PowerShell saves time and ensures consistency in large environments.
   - **Steps**: 
     - Use the following PowerShell script to create an OU and multiple users:
     ```powershell
     
     ```
     - This script creates an Organizational Unit named "LabUsers" and 1000 user accounts within that OU.
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



## Conclusion

This setup creates a secure, isolated internal network where the DC provides domain services and DHCP addresses to the client. The RAS/NAT configuration allows the internal network to communicate with the external internet while maintaining network security. This architecture is ideal for practicing domain management and network administration in a controlled lab environment.


