# Azure database migration Project

## Overview
Within this project I will aim to architect and implement a cloud-based database system on Microsoft Azure, showcasing your hands-on expertise in cloud engineering.
I'll begin by establishing a production environment database. Subsequently, you'll migrate the database to Azure SQL Database

## Virtual Machine Setup:
This project aims to set up a Windows Virtual Machine (VM) to emulate the functions of a Windows server, replicating the operations of an on-premise system within a company. The VM serves as a repository for the company's database, simulating a secure and dedicated data storage solution.

### Provisioning the VM

1. **Virtual Machine Creation:**
   - Created a new Windows VM using [Virtualization Platform].
   - Allocated sufficient resources (CPU, RAM, Storage).

2. **Windows Installation:**
   - Installed the Windows Server operating system on the VM.

3. **Network Configuration:**
   - Assigned a static IP address to the VM.
   - Configured DNS and gateway settings.
   - Ensured the VM is on the same network as the local machine.

4. **Firewall Rules:**
   - Configured Windows Firewall to allow RDP traffic (port 3389).
   - Opened necessary ports for SQL Server (port 1433).

### Remote Connection to the VM

1. **RDP Connection:**
   - Connected to the VM using Remote Desktop Connection.
   - Entered the VM's static IP address and login credentials.

## SQL Server Installation

1. **SQL Server Installation:**
   - Downloaded the SQL Server installation package.
   - Followed the installation wizard, selecting appropriate options.
   - Configured authentication and set a strong password for the SA account.

2. **SQL Server Management Studio (SSMS):**
   - Downloaded and installed SSMS.

## Database Creation

1. **AdventureWorks Database:**
   - Obtained the AdventureWorks backup file and saved in 'C:\Program Files\Microsoft SQL Server\MSSQL16.MSSQLSERVER\MSSQL\Backup'.
   - Restored the AdventureWorks database using SSMS.

## Documentation

1. **README Update:**
   - Updated the README file in this GitHub repository.
   - Included sections for VM Setup, SQL Server Installation, and Database Creation.
   - Provided step-by-step instructions for each task, including configurations and settings.
   - Added any troubleshooting steps or considerations.

## Additional Considerations

1. **Security:**
   - Ensured VM and SQL Server are secured following best practices.

