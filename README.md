# Azure DataBase Migration
  ![Github License](https://img.shields.io/badge/license-MIT-yellowgreen.svg)


  ## Project Background 📝

Shifting data from one source to another (A to B) is an example of a data migration process. There are many reasons why an industry may want to accomplish this: upgrading an old legacy system (a collection of dashboards/reports pulling from multiple sources), safety concerns etc. Though it may sound as trivial as a simple 'lift and shift' problem, in reality it is a more complex procedure surpassing simplistic approaches as a simple 'copy and paste'.

A common migration process involves shifting data from on-premise to a cloud service. Several cloud providers offer superior data handling stratergies alongside security and access control. These services provide a much more attractive solution to companies then to developing on-hand softwares and tools which can be heavily involved and may waste companies resources and time since alot of it will be 'reinventing the wheel'
 
The following project simulates a typical migration scenerio. It mimics migration to Azure Cloud services. The project is broken into 8 milestones. Each covering topics such as Azure Virtual Machine, Azure Entra ID for access control, Geo-replication and failover testing for backup security, -----------------

  ## Table of Contents 🗒

  * [Milestones](#milestones-💻)

    * [Mileston 1](#mileston-1)
    * [Mileston 2](#mileston-2)
    * [Mileston 3](#mileston-3)

  * [Usage](#usage-🏆)
  
  * [Contributors](#contributors-😃)

  * [Questions](#questions)

  * [License](#license-📛)
  
  ## Milestones  💻

 ### Mileston 1
 
This milestone mainly focused on the installation, setup and testing of the production environment in the form of an Azure Virtual Machines. The purpose is to setup an environment that will allow carrying out of the migration tasks in a safe and workable configurations without comprimising on-premise database and security. The key steps taken for this are highlighted as shown below:

  * **Setup Azure Virtual Machine** : a seperate virtual machine was setup as the testing production environment labelled `my-azure-mig-rg`. A windows 11 image was used with size `Standard DS2 v3` with 2 CPU cores and 120 GB hard disk space. For region `UK (South)` was picked and Geo-Replication was enabled. It was ensured that for security purposes a SQL authentication method is used and to prevent unecessary connections to the VM, the `Remote Desktop Protocol(RDP)` was enabled for remote access and inbond network traffic managed by the `network security group (NSG)` firewall on `port 3389`.

  ![Alt text](image.png)

   * **Setup Microsoft SQL Server Management System (SSMS) and SQL Server** : the SQL Server focuses on the core database engine and functionality, while SSMS environment provides a user-friendly interface, visual tools, and scripting capabilities for database management and administration. For the purpose of enabling periodic backups using `SQL Server Agent` (see [Mileston 4](#mileston-4)), the `Developer` version of SQL Server version 2022 was installed. (link : https://www.microsoft.com/en-us/sql-server/sql-server-downloads). For the purpose of the experiments, the sample database `AdventureWorks ver. 2022` was used. This can be obtained from (https://learn.microsoft.com/en-us/sql/samples/adventureworks-install-configure?view=sql-server-ver16&tabs=ssms) where the `.bak` file can be restored in SSMS.

---

 ### Mileston 2
 
In this milestone, the on-premie local DB (`AdventureWorks`) is migrated to the database in the Azure's cloud system via the Azure SQL Database services. This milestone is broken down into the following steps: a) setting up of Azure SQL Database, b) ensuring connection between the local DB and the cloud data in the Azure SQL database, c) comparing schemas of local and cloud DB and d) migrating

* **Create an Azure SQL Database** : an Azure SQL Database of the `Basic` pricing tier with `5 DTUs` (mixture of CPU, memory, read and write operations), a storage size of `2GB`, with `Pulic Access` enabled and a firewall rule setup to allow direct connection from Azure Data Studio. For security and backup, a differential backup frequence of `24hrs` was enabled with `Point-in-time recovery (PITR)` retenetion of 7 days and `geo-redundant` backup storage was set.

![Alt text](image-3.png)

* **Connection Local DB with Cloud in Azure Data Studio** : initially, the local DB was uploaded in Azure Data Studio services. Azure Data Studio tools allows seamless transition of data with powerful and user-friendly database management tools. To enable connection to the cloud DB, an addittional `firewall rule` was added to the networking setting of the Azure SQL database. Here, the IP address of the virtual VM was registered for both `StartIP` and `EndIP` options. Once confirmed, the experiment would progress to comparison of the schemas for the two databases.

* **Schema Comparison** : the comparison and synchronisation of database schemas was carried out using the `SQL Server Schema Compare` extension in Azure Data Studio. This extension simplifies the comparion of database schemas for seamless database migration.

* **Migration** : initially the Azure Data Studio services was installed. Connnections to both the local DB and the Azure SQL database were enabled. The migration process was accomplished using the `Azure SQL Migration extension`. As shown in the figure below, the tables are successfully migrated from the local DB to the Azure SQL Databse.

![Alt text](image-2.png)

The purpose of adopting the use the extension for the migration process is that it simplifies and streamlines the entire process. The major underlying processes are automated by the extension. One does not have to reinvent the wheel. Some of the key tasks the extension automates are :

  * Ensuring a reliable connection between the local DB and the Azure SQL database
  * Provide integration capabilities across seperate networks (under the hood the extension makes use of Microsoft Integration Runtime:  https://microsoft.com/en-gb/download/details.aspx?id=39717)
  * A validation procedure to ensure the migration settings are correct and error-free and to identify issues that may arise whilst the process.


  ---

 ### Milestone 3

Once the migration is accomplished, this milestone aims to seperately store the production database on Azure. For that, a seperate VM is setup that will act as the production environment. It is common in enterprises to have the main database (storing client information for example) while the development environment conserved for testing purposes. 

Addittionaly, an automatic backup solution for the the development environment is also setup. This serves as a great testing ground in the development environment. It ensures any incorrect modification of the data during experimenting is easily reverted via quick recovery.

So, breifly, two seperated backup methods are explored. These aim at ensuring that the on-premise data on the production environment is kept reserved in case of accidental loss. One method aims to save the backup on cloud using Azure Blob storage. The second to store a local copy on the VM using automatic backup solution in SSMS. Automatic backup solutions provided in SSMS are of type `full` (backup of entire database), `differential` (backup of entire database since previous `full` checkpoint) and `transaction log` (backup at specified point in a backup chain). This experiment uses the full configuraiton.

* **Cloud Backup of On-Premise Data** : The backup of the local DB was created using `Azure Blob Storage` services. These offer highly scalable and durable cloud storage for unstructured data with flexible storage tiers balancing performance and cost requirements. For the purpose of this experiment, the `General Purpose V2` storage was selected with `Geo-Redundant` storage enabled for the replication. For the Azure Storage container security, the `public access level` was set. This option allowed read access to both the contents of the container and the list of blobs. 

![Alt text](image-6.png)

* **Local Backup of On-Premise Data** : The local DB hosted on the production environment was also backuped locally using full backup confiugration in SSMS.

![Alt text](image-7.png)

This was followed by setting up of periodic backups for the local DB in SSMS and have these stored in Azure Blob storage created in the previous task. This was accomplished using 

1) create a maintenance plan using SQL Server Maintenance Plan -> for automating routine maintenance tasks for SQL Server Database and then upload those to blob storage


---

### Milestone 4

Disaster recovery



## Usage 🏆

TODO --> 

## License 📛 

Copyright @ MIT. All rights reserved.

Licensed under the MIT license.

## Contributors 

Mahed Javed - ksfmahed@outlook.com

## Tests 🧪

To run tests, run these commands:

```
none
```

## Questions

For additional questions, contact me at the email provided below. 

- GitHub: [mahedjaved](https://github.com/mahedjaved/)
- Email:  mahed95@gmail.com
