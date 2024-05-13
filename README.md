# Azure Cloud Service integration with Snowflake

### Creating the Azure storage Account
An Azure Storage Account is a cloud-based storage solution provided by Microsoft Azure. It offers a scalable, secure, and highly available storage platform for various data types, including blobs, files, queues, tables, and disks. These storage services can be used to store and manage data for applications running on Azure or on-premises.

Key features of Azure Storage Accounts include:

****Blob Storage****: Ideal for storing large amounts of unstructured data, such as images, videos, documents, and backups.

****File Storage:**** Provides fully managed file shares that can be accessed via the standard SMB (Server Message Block) protocol. It's suitable for applications that require shared file storage.

****Queue Storage****: Offers a messaging queue service for communication between application components, enabling reliable and asynchronous message delivery.

****Table Storage****: A NoSQL key-value store for storing structured data, often used for flexible data modeling and fast access to large datasets.

****Disk Storage****: Provides durable and high-performance block storage for virtual machines and other Azure services.

![image](https://github.com/Rohit-Ganjoo/WorldCup2023/assets/143324573/f7958afa-8d2a-401c-936f-6e47ee33a0f1)

### Hierarchy of the file system has to be enabled for easy access and addressing of files:
![image](https://github.com/Rohit-Ganjoo/WorldCup2023/assets/143324573/c079bb11-839c-4b2a-8e75-7a09430fb623)

## Azure Data Factory:
Azure Data Factory (ADF) is a cloud-based data integration service provided by Microsoft Azure. It enables users to create, schedule, and orchestrate data workflows at scale, allowing organizations to efficiently move, transform, and process data across various sources and destinations. ADF supports both cloud-based and on-premises data sources and can handle data movement and transformation tasks in a code-free or code-centric manner.

Key features of Azure Data Factory include:

****Data Integration****: ADF allows users to create data pipelines to ingest, transform, and load data from disparate sources such as databases, files, and applications.

****Data Orchestration****: Users can design complex data workflows using a visual interface to orchestrate the execution of various data activities, dependencies, and triggers.

****Data Transformation****: ADF provides capabilities for data transformation using built-in activities like mapping, data conversion, and data manipulation.

****Data Movement****: It supports efficient and scalable data movement between on-premises and cloud-based data stores, including Azure Blob Storage, Azure Data Lake Storage, Azure SQL Database, Azure Synapse Analytics (formerly Azure SQL Data Warehouse), and more.

****Data Monitoring and Management****: ADF offers monitoring dashboards, logging, and alerts to track the performance and health of data pipelines. It also integrates with Azure Monitor and Azure Data Factory Monitor for deeper insights and troubleshooting.

****Integration with Azure Ecosystem:**** ADF seamlessly integrates with other Azure services like Azure Synapse Analytics, Azure Databricks, Azure Machine Learning, Azure Logic Apps, and Azure Functions, enabling end-to-end data processing and analytics workflows.

![image](https://github.com/Rohit-Ganjoo/WorldCup2023/assets/143324573/cdfd5edd-09f0-4666-8339-a63908426837)

All the Files from the external HTTP has to be ingested using Azure Data Factory's Source option and Sink is connected to the Azure Storage account. This helps the file to move from external source to Storage Account.
![image](https://github.com/Rohit-Ganjoo/WorldCup2023/assets/143324573/93196564-75ea-49ec-815e-d24ab9c8e989)

## Creating a link Service with Azure Data Factory and Azure Storage Account( Blob Storage)
****Create Linked Service for Azure Storage Account:**** Before configuring the sink in your pipeline, you need to create a linked service for your Azure Storage Account. In Azure Data Factory, go to the "Manage" tab and select "Linked services." Then, choose "New" and select "Azure Blob Storage" as the type of linked service. Follow the prompts to configure the connection to your storage account.

****Create a Pipeline:**** In Azure Data Factory, create a new pipeline or open an existing one where you want to configure the sink.

****Add Copy Activity:**** Within the pipeline, add a Copy Activity. This activity will be responsible for moving data from a source to the sink.

****Configure Source:**** Configure the source of your data. This could be another data store or service from which you want to extract data.

****Configure Sink:**** In the Copy Activity, configure the sink to use Azure Delta Lake Version 2. To do this, select "Azure Delta Lake" as the sink dataset. Then, choose "Azure Delta Lake Version 2" as the version. After that, select your Azure Storage Account as the storage destination.

****Validate Pipeline:**** Once you've configured the sink, validate the pipeline to ensure there are no errors or issues.

****Debug Pipeline:**** Before running the pipeline in production, it's a good practice to debug it to ensure everything works as expected. You can debug the pipeline by clicking the "Debug" button in the Azure Data Factory interface. This will run the pipeline in debug mode, allowing you to see the data flow and troubleshoot any issues.

****Run Pipeline:**** After successfully debugging the pipeline, you can run it in production to start the flow of data from the source to the Azure Delta Lake sink in your Azure Storage Account.

![image](https://github.com/Rohit-Ganjoo/WorldCup2023/assets/143324573/9ff55adb-e5f8-40e2-8d45-07abf058a541)

## Connection of Azure Blob with Snowflake:

## Snowflake:
Snowflake is a cloud-based data warehousing platform that provides a fully managed service for storing, processing, and analyzing large volumes of data. It offers elasticity and scalability, separating compute and storage resources to optimize performance and cost-effectiveness. Snowflake's multi-cluster shared data architecture enables concurrent access to shared data without data duplication, enhancing collaboration and data consistency. With built-in security features, native integrations, and performance optimization capabilities, Snowflake simplifies data management and analytics, empowering organizations to derive actionable insights from their data efficiently and securely.

### Creating a Snowflake Warehouse:
In Snowflake, a "warehouse" refers to a computing resource used for processing queries and running tasks such as data loading, transformations, and analytics. Snowflake provides a fully managed, scalable, and elastic computing environment known as a "virtual warehouse."

![image](https://github.com/Rohit-Ganjoo/WorldCup2023/assets/143324573/c7073d72-d5ea-4235-bbe1-c7c933c3722e)
Extra Small warehouse with Auto Suspend and Auto Active Option enabled.

- Now Create a new Worksheet:
- Create Database:
```sql
CREATE OR REPLACE DATABASE azure;
```

- Create Schema: 
```sql
CREATE OR REPLACE SCHEMA worldcup;
```


- Creating a storage Integration with Azure Blob Storage
- Tenent_id can be found in Active Directory.
```sql
CREATE STORAGE INTEGRATION snow_azure_int
  TYPE = EXTERNAL_STAGE
  STORAGE_PROVIDER = AZURE
  ENABLED = TRUE
  AZURE_TENANT_ID = 'e7e7c7fe-52e1-4d71-9fdb-23d4e4d01k98'
  STORAGE_ALLOWED_LOCATIONS = ('azure://2023worldcup.blob.core.windows.net/2023worldcup/Raw Data');

```



- Describing the storage Integration to get the concent URL and Tenant App Name.
```sql
DESC STORAGE INTEGRATION snow_azure_int;
```
![image](https://github.com/Rohit-Ganjoo/WorldCup2023/assets/143324573/8aeb9e78-48a0-42df-9baf-4b7696844725)

-- Creating a file format with type csv.
```sql
CREATE OR REPLACE file format fileformat
    type = csv
    field_delimiter = ','
    skip_header = 1
    empty_field_as_null = TRUE; 
```
### Adding role to the Snowflake account so that it can contribute to the Blob Storage

![image](https://github.com/Rohit-Ganjoo/WorldCup2023/assets/143324573/62f38ed9-99be-4e69-b1ee-e3563df54f87)

![image](https://github.com/Rohit-Ganjoo/WorldCup2023/assets/143324573/5bdda56e-bea0-4791-b4a2-fd21e18bec98)


- Creating the stage that will hold the data
```sql
create or replace stage azure.public.stage_azure
    STORAGE_INTEGRATION = snow_azure_int
    URL = 'azure://2023worldcup.blob.core.windows.net/2023worldcup/Raw Data'
    FILE_FORMAT = fileformat;

```


- list the dataset that is present in the container(Storage Account)
```sql

LIST @azure.worldcup.stage_azure;
```
![image](https://github.com/Rohit-Ganjoo/WorldCup2023/assets/143324573/c42edf5e-2edc-4b36-9a4a-fa195924b2ba)



- Loading the players table:
```sql
create table players(
player_name varchar(100),
team_name varchar(100),
batting_style varchar(100),
bowling_style varchar(100),
player_role varchar(100)
);

copy into players
from (select $1,
$2,
$4,
$5,
$6
from  @azure.public.stage_azure/Players);

select * from players;

```



- Loading the Batting table:
```sql

create or replace table batting(
match_no int,
match_between varchar(100),
team_innings varchar(100),
batsman_name varchar(100),
batting_position int,
dismissal varchar(100),
runs int,
balls int,
Fours int,
Sixes int,
strike_rate varchar(100));


copy into batting 
from (select $1,
$2,
$3,
$4,
$5,
$6,
$7,
$8,
$9,
$10,
$11
from  @azure.public.stage_azure/Batting);
```


- Loading the Bowling data
```sql
create table bowling(
match_no int,
match_between varchar(200),
bowling_team varchar(200),
bowler_name varchar(100),
over int,
maidens int,
runs int,
wickets int,
economy double
);


copy into bowling 
from (select $1,
$2,
$3,
$4,
$5,
$6,
$7,
$8,
$9
from  @azure.public.stage_azure/Bowling);

```


- Loading Matches Data

```sql
create or replace table matches(
match_no int,
date varchar(20),
venue varchar(200),
team1 varchar(100),
team2 varchar(100),
winner varchar(100)
);

copy into matches 
from (select $1,
$2,
$3,
$4,
$5,
$6
from  @azure.public.stage_azure/Matches);
```

## Batting Stats for the world Cup 2023
Query Here:
![image](https://github.com/Rohit-Ganjoo/WorldCup2023/assets/143324573/ce2b4dda-fbea-4a02-a25f-99651db51c44)

![image](https://github.com/Rohit-Ganjoo/WorldCup2023/assets/143324573/fe72fe5f-b211-4820-a85b-33db386b02b3)

## Batting Analysis 
> Highest Run Scorers:

 1. Virat Kohli  (765)
 2. Quinton de-Kock (706)
 3. Rohit Sharma (597)

 
 
> Top Batsman with Highest Strike Rate:

 1. Gus Atkinson (166.67)
 2. Glenn Maxwell(162.81)
 3. Haris Rauf(152.17)


>Top Boundary Hitters

 1. Rohit Sharma(450) 31(6)/66(4)
 2. Quinton de-Kock(416) 26(6)/65(4)
 3. David Warner(380) 28(6)/53(4)

 
> Highest Scorers in Single innings

 1. Glenn Maxwell 201*
 2. Mitchell Marsh 177
 3. Quinton de-Kock 174
 
 

> Top Batting Average Batsman

 1. Virat Kohli 95.63
 2. Kane Williamson 85.3
 3. KL Rahul 75.3

## Bowling Stats for the world Cup 2023
Query Here:
![image](https://github.com/Rohit-Ganjoo/WorldCup2023/assets/143324573/956e90b4-934d-45d5-8d45-72e0abdf05aa)

![image](https://github.com/Rohit-Ganjoo/WorldCup2023/assets/143324573/3c5a556e-e25d-4b8d-883f-0ffc454a222c)

