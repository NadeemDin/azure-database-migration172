# Azure Database Migration Project

## Overview

Welcome to the Azure Database Migration Project! This comprehensive endeavor showcases the creation and implementation of a cloud-based database system on Microsoft Azure, highlighting practical skills in cloud engineering. Our journey begins with the setup of a production environment database, followed by a smooth migration to Azure SQL Database.

Additionally, we emphasize the creation of a dedicated development environment, kept separate from the production system, serving as a sandbox for testing and experimentation. This environment mirrors the production setup, allowing for controlled exploration of new concepts without impacting critical systems.

Furthermore, our project includes the automation and scheduling of backups to Azure storage, ensuring consistent protection and simplifying recovery processes. This strategic backup approach enhances data security and resilience.

## Virtual Machine Setup

In the initial phase, we create a Windows Virtual Machine (VM) to emulate the functions of a Windows server, resembling an on-premise system within a company. This VM serves as a secure repository for the company's database. Here's a quick rundown of the setup process:

- **Virtual Machine Creation:**
   - Use your chosen virtualization platform to create a new Windows VM.
   - Allocate sufficient resources (CPU, RAM, Storage).

- **Windows Installation:**
   - Install the Windows Server operating system on the VM.

- **Network Configuration:**
   - Assign a static IP address to the VM.
   - Configure DNS and gateway settings.
   - Ensure the VM is on the same network as your local machine.

- **Firewall Rules:**
   - Set up Windows Firewall to allow RDP traffic (port 3389).
   - Open necessary ports for SQL Server (port 1433).

- **Remote Connection to the VM:**
   - Connect to the VM using Remote Desktop Connection.
   - Enter the VM's static IP address and login credentials.

## SQL Server Installation

With the VM set up, it's time to install SQL Server. Follow these steps:

- **SQL Server Installation:**
   - Download the SQL Server installation package.
   - Follow the installation wizard, selecting appropriate options.
   - Configure authentication and set a strong password for the SA account.

- **SQL Server Management Studio (SSMS):**
   - Download and install SSMS.

## Database Creation

We're using the AdventureWorks database, a comprehensive sample database for our project. Follow these steps:

- **AdventureWorks Database:**
   - Obtain the AdventureWorks backup file.
   - Save it in 'C:\Program Files\Microsoft SQL Server\MSSQL16.MSSQLSERVER\MSSQL\Backup'.
   - Restore the AdventureWorks database using SSMS.

## Migration Setup

Now, let's migrate both schema and data from our local database to Azure SQL Database.

## Azure SQL Database Setup

Prepare the target for migration by setting up the Azure SQL Database:

- **Creation of Azure SQL Database:**
   - Create an Azure SQL Database for migration.

- **SQL Server Authentication:**
   - Configure SQL Server to use SQL login.

- **Firewall Rules:**
   - Ensure the SQL Server has appropriate firewall rules.

## Azure Data Studio Configuration

Set up Azure Data Studio for seamless migration:

- **Installation of Azure Data Studio:**
   - Install and configure Azure Data Studio on the production Windows VM.

- **Connection to On-Premise Database:**
   - Use Azure Data Studio to establish a connection to the existing on-premise database.

- **Connection to Azure SQL Database:**
   - Connect to the newly created Azure SQL Database using Azure Data Studio.

## Schema Migration

Migrate the schema with the SQL Server Schema Compare extension:

- **Installation of SQL Server Schema Compare Extension:**
   - Install the extension within Azure Data Studio.

- **Schema Comparison and Migration:**
   - Leverage the extension to compare and migrate the schema from the on-premise database to the Azure SQL Database.

## Data Migration

Transfer data seamlessly using the Azure SQL Migration extension:

- **Installation of Azure SQL Migration Extension:**
   - Install the extension within Azure Data Studio.

- **Data Transfer:**
   - Utilize the extension to transfer data from the on-premise database to the Azure SQL Database.

## Validation

Ensure a successful migration with a comprehensive validation process:

- **Comprehensive Validation:**
   - Inspect data, schema, and configurations to guarantee a successful migration and data integrity.

## Database Backup and Restoration

In this section, we'll cover the essential steps for generating a full backup of the production database, securing it locally, and establishing a robust backup strategy for a development environment.

### 1. Production Database Backup

Begin by creating a safety net for unforeseen issues by generating a full backup of the production database hosted on the Windows VM. Follow these steps:

1. **Generate Backup:**
   - Use SQL Server tools to create a full backup of the production database.

2. **Local Storage:**
   - Once the backup is complete, store the resultant backup file in a designated location on the local computer.

### 2. Azure Blob Storage Configuration

Securely store your database backups in Azure Blob Storage for an extra layer of protection. Here's how:

1. **Configure Azure Blob Storage:**
   - Start by configuring an Azure Blob Storage account.

2. **Upload to Blob Storage:**
   - Upload the previously created database backup file to the Blob Storage container.

### 3. Development Environment Setup

Create a controlled experimentation environment, similar to a sandbox, for development purposes. Follow these steps:

1. **Provision New Windows VM:**
   - Provision a new Windows VM mirroring the development setup.

2. **Install SQL Server:**
   - Install SQL Server on the new VM to mimic the necessary database infrastructure.

3. **Restore Database Backup:**
   - Restore the database backup from the blob storage onto this new "sandbox" environment.
  
### 4. SQL Server Credential Creation

Before utilizing the SQL Server Maintenance Plan Wizard, it was necessary to create a SQL Server Credential. This step ensures secure access to the Azure Blob Storage. Execute the following SQL script:

```
CREATE CREDENTIAL [YourCredentialName]
WITH IDENTITY = '[Your Azure Storage Account Name]',
SECRET = 'Access Key';
```

### 5. Automated Backups for Development

Implement an automated backup strategy for your development environment using SSMS. Here's how:

1. **Management Task with SSMS:**
   - On your development Windows VM, use SSMS to establish a Management Task.

2. **Configure Backup Schedule:**
   - Configure a weekly backup schedule using the SSMS maintenance plan wizard to ensure consistent protection for your evolving work and simplify recovery for your development environment if needed. This was configured to upload to the Azure blob storage.

### 6. Disaster Recovery

Distaster can appear in the form of data deletion or corruption.
It is therefore essential to ensure regular backups are saved for recovery purposes.
A simulated deletion was ran within the production environment. 

1. **Table deletion in SSMS**
   - The Sales.SalesTerritoryHistory table was deleted directly from SSMS - to recover we can simply use the previously saved/automated backup created in section 5.

2. **Data Deletion or Corruption**
   - Luckily the database is backed up directly to Azure blob storage via task 5. We can access this and select restore, after completing the form and selecting a point of restoration; this will restore the database to the previous version and allow us to refresh the Azure data studio and regain deleted or corrupted information.
  
SQL queries below are methods of intentional data deletion or corruption.
  
```
     -- Intentional Deletion
DELETE TOP (100)
FROM Sales.SalesTerritoryHistory;

-- Data Corruption
UPDATE TOP (100) Sales.SalesTerritoryHistory
SET product_price = NULL
```

Note: To avoid significant data loss or corruption, it is important to always query or manipulate the database within a testing enviroment.




