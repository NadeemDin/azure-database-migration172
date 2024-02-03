# Azure Database Migration Project

## Overview
This project aims to architect and implement a cloud-based database system on Microsoft Azure, showcasing hands-on expertise in cloud engineering. The project involves establishing a production environment database and migrating it to Azure SQL Database.

## 1. Virtual Machine Setup:

### 1.1 Provisioning the VM

- **Virtual Machine Creation:**
   - Created a new Windows VM using [Virtualization Platform].
   - Allocated sufficient resources (CPU, RAM, Storage).

- **Windows Installation:**
   - Installed the Windows Server operating system on the VM.

- **Network Configuration:**
   - Assigned a static IP address to the VM.
   - Configured DNS and gateway settings.
   - Ensured the VM is on the same network as the local machine.

- **Firewall Rules:**
   - Configured Windows Firewall to allow RDP traffic (port 3389).
   - Opened necessary ports for SQL Server (port 1433).

### 1.2 Remote Connection to the VM

- **RDP Connection:**
   - Connected to the VM using Remote Desktop Connection.
   - Entered the VM's static IP address and login credentials.

## 2. SQL Server Installation

### 2.1 SQL Server Installation

- Downloaded the SQL Server installation package.
- Followed the installation wizard, selecting appropriate options.
- Configured authentication and set a strong password for the SA account.

### 2.2 SQL Server Management Studio (SSMS)

- Downloaded and installed SSMS.

### 2.3 Database Creation

- **AdventureWorks Database:**
   - Obtained the AdventureWorks backup file and saved it in 'C:\Program Files\Microsoft SQL Server\MSSQL16.MSSQLSERVER\MSSQL\Backup'.
   - Restored the AdventureWorks database using SSMS.

## 3. Migration Setup:

I will create an Azure SQL Database with the intention of migrating both schema and data from my local database to this newly created Azure SQL Database.

## 4. Azure SQL Database Setup

### 4.1 Creation of Azure SQL Database

- Created an Azure SQL Database to serve as the target for database migration.

### 4.2 SQL Server Authentication

- Configured SQL Server to use SQL login as the chosen authentication method.

### 4.3 Firewall Rules

- Ensured that the SQL Server has appropriate firewall rules, including adding the local machine's IP address to the firewall settings.

## 5. Azure Data Studio Configuration

### 5.1 Installation of Azure Data Studio

- Installed and configured Azure Data Studio on the production Windows VM.

### 5.2 Connection to On-Premise Database

- Used Azure Data Studio to establish a connection to the existing on-premise database.

### 5.3 Connection to Azure SQL Database

- Established a connection to the newly created Azure SQL Database using Azure Data Studio.

## 6. Schema Migration

### 6.1 Installation of SQL Server Schema Compare Extension

- Installed the SQL Server Schema Compare extension within Azure Data Studio.

### 6.2 Schema Comparison and Migration

- Leveraged the extension to compare and migrate the schema from the on-premise database to the Azure SQL Database.

## 7. Data Migration

### 7.1 Installation of Azure SQL Migration Extension

- Installed the Azure SQL Migration extension within Azure Data Studio.

### 7.2 Data Transfer

- Utilized the extension to transfer data from the on-premise database to the Azure SQL Database, ensuring a smooth migration process.

## 8. Validation

### 8.1 Comprehensive Validation

- Conducted a thorough validation of the migrated database, inspecting data, schema, and configurations to ensure successful migration and data integrity.

## Additional Considerations

### 9. Security

- Ensured VM and SQL Server are secured following best practices.
