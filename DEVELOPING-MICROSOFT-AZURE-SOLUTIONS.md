# Developing Microsoft Azure Solutions

## Sections of the exam
## **Create and manage Azure Resource Manager (ARM) Virtual Machines**
  - Virtual Machines
    - Manage through Portal, PowerShell (mainly for DevOps) or REST API.
    - Consists of Virtual Network, a Cloud Service wrapper with a Virtual Machine in that wrapper, with a storage account that contains an Image, OS Disk, and Data Disk(s).
  - Deployment
    - Do it in the Azure market place.
    - Can select existing VM images
    - Can use PowerShell and deploy using PowerShell too
    - There are also JSON ARM Templates which allow for intelliSense and fast set up
  - ARM Templates Portal 
    - Custom deployment allows you to create a template from scratch
  - Desired State Configuration
    - Allows you to preset/set the VM to a desired state
    - Usually will be seen in the template.
  - Uploading Images
    - Create a custom image from an existing machine using Sys prep or the system preparation tool
    - This allows you to create a template of an existing machine that we can continue to use within Azure as a new virtual machine
  - Availability Sets
    - Availability sets are NOT created by default
    - You have to create the Availability Set WITH the virtual machine
    - VMs may be impacted by unplanned outages such as power cuts, networks etc. Or during planned maintenance, we need to understand were patching the underlying host OS. These will run as a VM but we yet we still need to patch the Host we still need the VMs to be available. When you create a availability set you will have upgrade domains and fault domains. 
    - Planned vs unplanned maintenance
      - Configure multiple VMs in an availability set for redundancy
      - Configure each application tier into separate availability sets
      - Combine a Load Balancer with availability sets
      - Use multiple storage accounts for each availability set
    - Upgrade domains
      - Is for underlying patching
      - Five (non-user configurable) domains by default
      - The upgrade domains are groups of VMs that are rebooted at the same time for patching.
      - Groups of VMs and hardware that can be rebooted at the same time
    - Fault domains
      - A fault domain is a rack of servers. So 2 VMs in the saem availability set means Azure will provision them into 2 different racks. 
      - Separate hardware & network (Incase of a network switch, or power outage, or hardware failure)
      - 2 domains by default
      - Group of VMs that share a common power source and network switch 
  - Storage replication options
    - Locally redundant
	  - Default
	  - up to 3 nodes in same data center
	- Zone redundant storage
	  - up to 3 data centers in the same region
	- Geo-redundant storage
	  - Data center in a different region
	- Read-access geo-redundant storage
	  - Data center in a different region 
	  - Secondary data can be read (read-only not write).
  - ARM VM Storage
    - Configure disk caching
      - Input/output operations per second (IOPS)
      - Throughput (Mbps)
      - Read/write vs Read and striping
  - Storage capacity
    - Sizing and egress traffic limits (there are scalability and performance targets regarding storage for a VM)
  - Azure File Service
    - SMB file shares from the VM
    - Can be used from the File system I/O APIs
  - Premium vs Standard
    - Prem: high-performance, low-latency disk support for I/O intensive workloads
  - VM Networking
    - Network Security Group (NSG)
      - A virtual network device which has ACL rules that allow/deny network traffic
      - Can only be applied to resources within the region it was created
      - Diagnostic logging capability
      - Associating (NSG to Network Interface Card (NIC), NSG to subnet) - ONE NSG to a NIC or subnet
    - User Defined Routes (UDRs) / Route tables
      - Specify the next hop for packets flowing to a specific subnet
      - Force tunnelling to the internet via your on-premises network
      - Network Virtual Appliance
      - Routes selected in order of 1. UDR, 2. BGP (with ExpressRoute), 3. System Route
      - Use of virtual appliances in your Azure environment
      - System Routes
        - 3 Default Rules: Local vNet, On-premises, Internet
          - Any subnet to another w/in a VNet.
          - From VMs to the internet
          - From VNet to another VNet via VPN gateway
          - From a VNet to another VNet through VNet Peering
          - From a VNet to on-premises network via VPN gateway
    - Application Gateway
      - Load balancing
        - Websocket traffic
        - Sticky session apps
        - SSL offload
  - VM Scale Sets
    - Identical set of VMs
      - PaaS-like autoscale
      - Focus is load and elastic in and out (can scale up and down)
    - Scaling
      - PaaS-like autoscale using autoScaleSettings in ARM template
        - Rules using metricTriggers
      - Can combine Desired State Configuration extension
      - Initial scale setting using ARM template: "sku" : { "name" : "Standard_A0", "tier": "Standard", "capcity": 3 }
        - Can configure a Vms scale set's scaling capabilites in the template.
   - Notes:
     - All deployment slots for the same Web App are hosted within the same Virtual Machine (VM). This is especially important to remember when stress testing a non-production deployment slot before swapping it into production. A significant amount of load on a non-production deployment slot can affect the performance of the live, production Web App.
## **Design and implement a storage and data strategy**
  - Azure Storage and Azure SQL Database are important in Microsoft Azure PaaS strategy for storage. 
  - Azure Storage 
    - https://docs.microsoft.com/en-us/azure/storage/storage-introduction#what-is-azure-storage
    - Enables storage and retrieval of large amounts of unstructured data
    - Documents, media in the Blob service, Table service for NoSQL data, queue service for reliable messages, file service for server message block file share scenarios. 
  - Azure SQL Database
    - Provides classic relational database features as part of an elastic scale service. 
  - Storage
  	- Blobs / Files 
  	  - A blob is acced from a container within a storage account. From there, we actually access the particular blob. 
  	  - A SAS token is the gateway that allows us with security to access our particular blob. (Like a valet key in a car that allows you to access a particular storage location)
  	  - Types
	  	  - Block Blob
	  	  - Page Blob
	  	  - Append Blob
	  - Files
	    - Files are accessible via SMB interface as a file share
	    - Can also access those files using the RESTful APIs
	  - Azure Blobs vs Azure Files
	    - Accessibility: Blob = REST APIs, Files = REST APIs, or SMB interface
	    - Endpoints: Blob = myaccount.blob.core.windows.net/mycontainer/myblob, Files = myaccount.file.core.windows.net\myshare\myfile.txt
	    - Capacity: Blob = Up to 500TB containers, Files = 5TB file shares
	    - Throughput: Blob = Up to 60 MB/s per blob, Files = Up to 60 MB/ps per share
	    - Billed capacity: Blob = based on bytes written, Files = based on file size
  	- Tables
  	  - Uses ODATA interface or a TableClient to interface with the Table storage
  	  - Uses an account that has tables, which has individual entities within it - you access the entities
  	  - Uses a SAS token as a gateway into the account for security
  	- Queues
  	  - QueueClient which interfaces to the Queue storage
  	  - Stored within an account, within a table, within a entities (entities are inside the queue)
  	  - Uses a SAS token as a gateway into the account for security 
  	- Redis Cache
  	  - Tiers: Basic, Standard & Premium
  	  - Concurrency
  	    - Optimistic vs pessimistic
  	  - Distributed app caching
  	    - Shared vs Private
  	    - Data persistence 
  	    - Clustering
  	- Monitoring 
  	- SQL DB
  	  - Tiers: Basic, Standard, Premium versions etc. You can migrate between the tiers using the Portal, PowerShell or REST API
  	  - SQL DB maintains global mapping information about all shards (DBs) in a shard set
  	  - Metadata used to route based on sharding key
  	- Search
  	  - Index: Persistent store of documents
  	  - Add data (Push JSON data with .NET SDK or REST API, Pull with indexers supporting Azure sotrage and .NET SDK or REST API)
  	  - Handle Results
  	    - Total hits and page counts
  	    - Layout results
  	    - Sorting and filtering
  - Access control with Shared Access Signature (SAS)
    - Application requests a SAS Token from the SAS Service
    - SAS Token gets generated and gets handed to Application
    - Application then makes a storage request to Azure Storage with the SAS Token
    - Azure Storage responds
  - Storage Access
    - Access through RESTFUL api:
    - Blobs: //[account].blob.core.windows.net/[container]/[blob]
    - Files: //[account].file.core.windows.net/[file]
    - Tables: //[account].table.core.windows.net/[table]([paritition key], [row key])
    - Queues: //[account].queue.core.windows.net/[queue]
  - Azure Storage Service Encruption (SSE)
    - Features
      - 256 BIT AES Encryption 
      - Block, Page and Append Blobs
      - General purpose and Blob Storage Accounts
      - All redundancy levels and all regions
      - Only applicable for ARM, not ASM
    - Limitations
      - The following are not encryped:
	      - Classic storage and classic migrated are not included. 
	      - Existing data before turned on is not included. 
	      - Tables, Queues, and Files data 
	  - i.e. If you want the encryption you have to enable it at the beginning right away.
## **Design and implement Azure PaaS compute and web and mobile services**
  - Web & Mobile
    - App Service (Web apps, Mobile apps, API apps and logic apps)
      - Features:
        - Auto-patching and auto-scale
        - .NET, Java, Node.js, PHP, Python
        - Integrate with SaaS and on-premises (resources)
        - Continuous integration with VSTS, Github, BitBucket and more.
      - Web apps
        - Deployment
          - Hosting
            - Need to establish a resource group that has an App service plan.
            - From there you are going to deploy and host your Web application.
        - Diagnostics
          - Azure Application Insights
            - Monitor: WebApps, ASP.NET, Java Apps, Windows Services, Docker apps, JS, SharePoint Sites, Node, PHP, Ruby, Python, Objective-C etc.
          - DevOps Cycle
            - Azure Applications Insights helps devops.
            - Detect, Triage, Diagnose
            - Monitor performance, failures, usage
        - Web Jobs
          - Can be run using On-Demand or Scheduled initiation
          - A Web Job runs inside a web app (Runs on the same process)
          - Creating WebJobs
            - Uploaded in a zip file
            - Types: Python, Batch, PowerShell, Java, .NET
            - Scheduling: settings.job file at root of zip file or just use the Azure UI 
          - Configuration of WebJobs
            - Run it continously, or one time, or on-demand
        - Scaling
          - Azure Traffic Manager
          	- Can have multiple instances of these storage solutions and scale between them by using Azure Traffic Manager
          	- Azure Traffic Manager routes the traffic to different instances/websites
          - Scalable and global web app and database
            - Scale quickly with a slider bar, from a schedule, or based on CPU load
            - Route users globally to copies of Web Apps and SQL Databases
            - Improve performance by using a distributed cache layer
        - Resillience
          - Isolating a web app
            - Web app with Personally Identifiable Information (PII) and database
              - Secure the web app with the PII from malicious requests etc
              - Host resources isolated and securely
              - Block malicious requests through active defense firewalls
              - Access on-premises resources from a cloud environment with a secure connection'
      - Mobile apps
       	  - Adding mobile features to a web app
            - Native or Xamarin-native mobile client app that connects to an Azure Mobile App backend and shares data and APIs with an Azure Web App
          	  - Create cross-platform mobile clients easily and consistently
          	  - Share data and APIs as-is across mobile and web
          	  - Enable mobile back-end featuers for push notifications, offline data sync, and auto-scaling
      - API apps
      - logic apps
    - API Management
      - Create Managed APIs
        - Think of it as an area to host the APIs
        - API Gateway + Developer Portal + Publisher Portal
          - Gateway is like a proxy providing rate limits, applying policies, customize the developer portal
          - Developer Portal - developers come to a developer portal to access the API
          - Can add caching
          - Can trace API calls using the API Inspector 
    - Service Fabric
      - Service fabric can run on the public clolud, private cloud, hosted cloud, or on-premises.
      - Modernization with microservices
        - Individually built and deployed
        - Small, independently executing services
        - Integrate using published API calls for overall application's functionality
        - Fine-grained, loosely coupled application
      - Manager microservices at scale
      - CI/CD pipeline endpoint
      - 24x7 service availability
      - Stateful services
      - Containers and Docker
      - Multi-cloud
      - How to deploy and configure Service Fabric?
    - Cloud services
      - PaaS with VM control
        - Simple .NET runtime
        - Health, discovery, updates
        - OS Patching
        - The original PaaS offering from 2010. Best used when low-level OS access is required, but consider the newer PaaS models first.
    - Azure Functions
      - Async, event-driven, serverless experience
      - Respond to events occurring in other Azure services, SaaS products, on-premises systems
      - Only pay while function is executing 
      - Fully open source
## **Manage identity, application, and network services**
  - Azure AD
    - For the cloud and hosted within Azure
    - Supports SharePoint Online, Exchange online etc
    - Can interface to Azure AD using the Graph API (different to on-opremises solutions with AD which uses LDAP to interface with it)
    - Azure Active Directy is separate from Active Directory but can be synchronized to have single sign on etc.
    - **AD On-premises vs Azure AD**
      - Graph API (Azure AD)
        - Programmatic Access to Azure AD | RESTful
        - CRUD | Application must be registered and configured
        - Requests use standard HTTP Methods
      - OAuth (Azure AD)
        - AuthZ web apps and web APIs in Azure AD Tenant 
        - Access authorization, role-based assignment
        - For app and user authorization
      - OpenID Connect (Azure AD)
        - authorize etc
        - AuthZ protocol for SSO
        - Extends Oauth 2.0 for use as AuthN protocol
  - AD B2B/B2C
    - AD B2C:
      - How does Business 2 Consumer fit in?
      - What is it for?
        - Developers working on Consumer and citizen-facing mobile and web apps that reach out to the customers directly
      - Using social accounts such as FB, twitter etc using OAuth 
      - Manageability: Self-service and seamless identity experience
      - Can use Graph API to access Azure AD to look for that info
      - Can enable multi-factor authentication
      - Has protocol support (OAuth2 etc)
    - AD B2B:
      - What is Business 2 Business used for?
        - Mainly for IT pros providing access to their organisations data and application to partner organizations and collaborators
        - It's for partner users that are acting or behalf of representatives or employees from their orgnaisation
      - Manageability: Access reviews, email verification, allowlist/denylist, etc... govern access to host application and resources
  - Communicate
    - Azure Service Bus
      - Queues
        - FIFO Queues
        - Simple Client
      - Topics
        - Targeting Messages
        - Work with Queues
      - Relay
        - Expose OnPrem service to public
        - Leverage WCF
      - Notification Hubs 
        - Push notification infrastructure
        - Support for non-MSFT targets
      - **Service Bus Queue vs Storage Queue**
        - Service bus queues
          - FIFO guaranteed
          - Delivery once and only once
          - 60 second default locks can be renewed
          - Messages are finalized once consumed
          - Native integration with WCF and WF
        - Storage queues
          - Order not guaranteed
          - Delivery at least once, maybe multiple times
          - 30 second default locks, extendable to 7 days
          - In-place updates of content
          - Can integrate with WF through custom activity