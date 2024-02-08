# Azure Database Migration Project

## Overview

Welcome to the Azure Database Migration Project! This comprehensive endeavor showcases the creation and implementation of a cloud-based database system on Microsoft Azure, highlighting practical skills in cloud engineering. Our journey begins with the setup of a production environment database, followed by a smooth migration to Azure SQL Database.

Additionally, we emphasize the creation of a dedicated development environment, kept separate from the production system, serving as a sandbox for testing and experimentation. This environment mirrors the production setup, allowing for controlled exploration of new concepts without impacting critical systems.

Furthermore, our project includes the automation and scheduling of backups to Azure storage, ensuring consistent protection and simplifying recovery processes. This strategic backup approach enhances data security and resilience.

## Virtual Machine Setup

In the initial phase, we create a Windows Virtual Machine (VM) to emulate the functions of a Windows server, resembling an on-premise system within a company. This VM serves as a secure repository for the company's database. Here's a quick rundown of the setup process:

- **Virtual Machine Creation:** Utilize your chosen virtualization platform to create a new Windows VM, allocating sufficient resources (CPU, RAM, Storage) with cost/use case in mind.
- **Firewall Rules:** Set up Windows Firewall to allow RDP traffic (port 3389) and open necessary ports for SQL Server (port 1433) - this is default.
- **Remote Connection to the VM:** Connect to the VM using Remote Desktop Connection and enter the VM's static IP address and login credentials - this can be done by downloading the RDP file via the connections menu when viewing the VM resource.

## SQL Server Installation

With the VM set up, it's time to install SQL Server. Follow these steps:

- **SQL Server Installation:** Download the SQL Server installation package (https://learn.microsoft.com/en-us/sql/ssms/download-sql-server-management-studio-ssms?view=sql-server-ver16), follow the installation wizard, and configure authentication with a strong password for the SA account.
- **SQL Server Management Studio (SSMS):** Download and install SSMS.

## Database Creation

Utilizing the AdventureWorks database as a comprehensive sample database for our project, follow these steps:

- **AdventureWorks Database:** Obtain the AdventureWorks backup file, save it in 'C:\Program Files\Microsoft SQL Server\MSSQL16.MSSQLSERVER\MSSQL\Backup', and restore the AdventureWorks database using SSMS.

## Migration Setup

Now, let's migrate both schema and data from our local database to Azure SQL Database.

## Azure SQL Database Setup

Prepare the target for migration by setting up the Azure SQL Database:

- **Creation of Azure SQL Database:** Create an Azure SQL Database for migration.
- **SQL Server Authentication:** Configure SQL Server to use SQL login.
- **Firewall Rules:** Ensure the SQL Server has appropriate firewall rules.

## Azure Data Studio Configuration

Set up Azure Data Studio for seamless migration:

- **Installation of Azure Data Studio:** Azure Data Studio is already preinstalled via the installation of SSMS and can be accessed via tools within SSMS.
- **Connection to On-Premise Database:** Use Azure Data Studio to establish a connection to the existing on-premise database.
- **Connection to Azure SQL Database:** Connect to the newly created Azure SQL Database using Azure Data Studio.
- **SQL:** Queries can be ran within Azure Data Studio.

## Schema Migration

Migrate the schema with the SQL Server Schema Compare extension:

- **Installation of SQL Server Schema Compare Extension:** Install the extension within Azure Data Studio.
- **Schema Comparison and Migration:** Leverage the extension to compare and migrate the schema from the on-premise database to the Azure SQL Database.

## Data Migration

Transfer data seamlessly using the Azure SQL Migration extension:

- **Installation of Azure SQL Migration Extension:** Install the extension within Azure Data Studio.
- **Data Transfer:** Utilize the extension to transfer data from the on-premise database to the Azure SQL Database.

## Validation

Ensure a successful migration with a comprehensive validation process:

- **Comprehensive Validation:** Inspect data, schema, and configurations to guarantee a successful migration and data integrity.

## Database Backup and Restoration

In this section, we'll cover the essential steps for generating a full backup of the production database, securing it locally, and establishing a robust backup strategy for a development environment.

### 1. Production Database Backup

Begin by creating a safety net for unforeseen issues by generating a full backup of the production database hosted on the Windows VM. Follow these steps:

1. **Generate Backup:** Use SQL Server tools to create a full backup of the production database.
2. **Local Storage:** Once the backup is complete, store the resultant backup file in a designated location on the local computer.

### 2. Azure Blob Storage Configuration

Securely store your database backups in Azure Blob Storage for an extra layer of protection. Here's how:

1. **Configure Azure Blob Storage:** Start by configuring an Azure Blob Storage account.
2. **Upload to Blob Storage:** Upload the previously created database backup file to the Blob Storage container.

### 3. Development Environment Setup

Create a controlled experimentation environment, similar to a sandbox, for development purposes. Follow these steps:

1. **Provision New Windows VM:** Provision a new Windows VM mirroring the development setup.
2. **Install SQL Server:** Install SQL Server on the new VM to mimic the necessary database infrastructure.
3. **Restore Database Backup:** Restore the database backup from the blob storage onto this new "sandbox" environment.

### 4. SQL Server Credential Creation

Before utilizing the SQL Server Maintenance Plan Wizard, it was necessary to create a SQL Server Credential. This step ensures secure access to the Azure Blob Storage. Execute the following SQL script:

```
CREATE CREDENTIAL [YourCredentialName]
WITH IDENTITY = '[Your Azure Storage Account Name]',
SECRET = 'Access Key';
```

### 5. Automated Backups for Development

Implement an automated backup strategy for your development environment using SSMS. Here's how:

1. **Management Task with SSMS:** On your development Windows VM, use SSMS to establish a Management Task.
2. **Configure Backup Schedule:** Configure a weekly backup schedule using the SSMS maintenance plan wizard to ensure consistent protection for your evolving work and simplify recovery for your development environment if needed. This was configured to upload to the Azure blob storage.

### Disaster Recovery

Disasters can occur in various forms, such as data deletion or corruption. Hence, it is crucial to ensure regular backups are saved for recovery purposes. A simulated deletion was run within the production environment.

1. **Table Deletion in SSMS:** The Sales.SalesTerritoryHistory table was deleted directly from SSMS - to recover, we can simply use the previously saved/automated backup created.
2. **Data Deletion or Corruption:** Luckily, the database is backed up directly to Azure blob storage. We can access this and select restore, after completing the form and selecting a point of restoration; this will restore the database to the previous version and allow us to refresh the Azure Data Studio and regain deleted or corrupted information.

SQL queries below are methods of intentional data deletion or corruption.
  
```
-- Intentional Deletion
DELETE TOP (100)
FROM Sales.SalesTerritoryHistory;

-- Data Corruption
UPDATE TOP (100) Sales.SalesTerritoryHistory
SET product_price = NULL
```

### Geo-Replication

To enhance data protection and further prevent data loss, reduce downtime, and ensure data availability, geo-replication is used to create a replica of our production database and store it in another geographical location. This can be done pretty easily by accessing the original production database and selecting a replica from the selections provided in the Azure side menu. By simply filling out the forms and deploying, we can create the geo-replicated database which can be connected to should we need it.

### Failover

In Azure, failover is crucial for maintaining uninterrupted service availability and reliability. Failover automatically redirects traffic from a primary instance to a secondary one in case of failure. Azure provides tools like Traffic Manager, Load Balancer, and Site Recovery to facilitate failover. These services ensure continuous operation by distributing traffic, detecting and rerouting around failures, and orchestrating recovery processes. Implementing failover in Azure minimizes downtime, enhances business continuity, and instills confidence in the reliability of cloud-based applications and services.

To initiate a failover, follow these steps:

1. **Primary and Secondary Databases:** Ensure you have a primary and secondary database set up, such as the newly made/geo-replicated database from section 7.
2. **Access Azure Portal:** Navigate to the Azure portal and access the server associated with the primary database.
3. **Select Failover Groups:** Under the data management pane, select the failover groups option.
4. **Create Failover Group:** Create a new failover group and proceed by selecting failover. This action will switch the roles of the primary and secondary databases.
5. **Test Failover:** To test a successful failover, connect to the now primary server via Azure Data Studio, and validate the schema and data.

