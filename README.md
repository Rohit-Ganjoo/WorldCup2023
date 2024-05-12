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

