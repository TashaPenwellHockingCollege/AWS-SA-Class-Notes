# Choosing AMI to Launch EC2 Instance
 * AMI
    provides information needed to launch an instance.  This includes
       1.  Template for root volume
             contains OS and other installed SW (applications, libraries, utilities, etc)
       3.  Launch permissions
             controls which AWS account can access the AMI
             can also make it available to the public
       5.  Block device mappings
             spceifies any storage  volumes attached to the instance
  * Benefits of using AMI
    1.  Repeatability
            an AMI can be used repeatedly to launch isntances w/ efficiency/precision
            
    3.  Reusability
            instances launched from same AMI are identically configured
    5.  Recoverability
            create an AMI from a configured instance as a restorable backup
            can replace a failed instance by launching new instance from same AMI
            provide a way to backup a complete EC2 instance configuration which can use to launch a replacement instance if there's a failure 
    * Choosing an AMI
      Choice should be based on 5 key characteristics
        1.  Region:  each AMI exists in a specific Region so will need to select an AMI that's in the Region where want instance to run; can copy an AMI from one Region to another
        2.  OS:  MS or Linux
        3.  Storage Type of root device:  all AMIs are categorized as either Amazon EBS-backed (persists independently of Instance's lifetime) or Instance store-backed (where data only exists during instance's lifetime)
        4.  Architurecture:  select architectures that best fits workload; choices are 32-bit or 64-bit and either an X86 or Advanced RISC machine (ARM) instruction set
        5.  Virtualization type:  AMIs use one of two types of virtualization; differences in between PV and HVM are in how they boot and whether can take advantage of special HW extensions for better performance
             1.  Paravirtual (PV)
             2.  Hardware Virtual Machine (HVM):  gives best performance
    * Obtaining an AMI 
        4 difference sources
           1.  QuickStarts:  AMIs built by AWS; offer choice of Windows or Linux OS
           2.  My AMIs:  any AMIs you create
           3.  AWS Marketplace:  pre-configured templates from 3rd parties
           4.  Community Built AMIs:  AMIs shared by others, use at own risk
    * Instanced Store-Backed vs Amazon EBS-Backed AMI
       AMAZON EBS-BACKED INSTANCE
          * Boots faster
          * 16 TiB is max size of root device
          * Can stop the instance
          * Can change the instance type by stopping the instance
          * Charged for instance usabe EBS volume usage adn storing AMI as an EBS Snapshot
       INSTANCE STORE-BACKED INSTANCE
          * takes longer to boot; because all image parts to restore has be retrieved from S3
          * 10 GiB is max size of root device
          * Can't stop the instance, only reboot or terminate
          * Can't change the instance type because instance can't be stopped
          * Charged for instance usage and storing AMI in S3

## Adding Compute w/ EC2
    * several compute arch to meet different needs
    * 4 Categories
        1.  for VM (provides higher infra control and customization)
                * ec2
                * Lightsail (ideal for simple workloads, quick deployments and getting started on AWS)
        2.  for Containers (provides higher infra control adn customization_
                * Elastic Container Service (ECS)
        3.  for PaaS (allow to focus more on application and less on infra; enable quick deployment)
                *Elastic Beanstalk (run web apps and services developed in Java, .NET, PHP, Node.js, Python, Ruby, and dockerless containers
        4.  for Serverless (allow to focus more on application and less on infra; enable quick deployment)
                * AWS Lambda (run code w/o creating/managing servers)
                * AWS Fargate (run containers through serverless compute engine)
                
          for Specialized Solutions (fully managed services) (address specific types of workloads like hybrid cloud and batch workloads)
                * AWS Outposts (provides way to run AWS infra and services on-premises)
                * AWS Batch (service that runs batch jobs at any scale)
                

# EC2
    * provides resiable compute capacity in the cloud
    * provides VM (servers)
    * provisions servers in minutes
    * auto scale capacity up or down as needed
    * enables you to pay only for capacity you use
    * is a VM that runs on physical host
    * can choose different configs of CPU and memory capacity
    * supports different storage options
        1.  Instance store
        2.  Amazon Elastic Block Store (EBS)
    * Provides network connectivty
    * each VM runs an OS (Linux or Windows)
        -referred to as the "guest OS"
    * can install and run apps on the OS in each VM
    * can run enterprise apps that span multipe VM
    * VM run on top of hypervisor layer maintained by AWS
        -HYPERVISOR:  Operating platform layer tha provides EC2 instances w/ access to actual physical HW resources that it needs to run (i.e. processors, memory, storage)
    * some EC2 instances use an instance store
        -INSTANCE STORE:  storage that's physically attached to the host computer and provides temp block level storage to an instance; aka Ephemeral (temporary) storage; data is deleted when stop EC2 instance
    * many EC2 instances use EBS for boot disk and storage needs
        - EBS provides persistent block-storage volumes; data will still be there after stop instances
    * EBS-optimized instances provide faster access to attached EBS volume by minimizing the IO contention between the volume and other traffic from isntance
    * EC2 instances can have connectivity to other resources such as other EC2 instances, AWS service and the internet
    * can configure the degree of network access to suit your needs and balance accessibility needs and security requirements

## EC2 Use CAses
    * complete control of computing resources including the OS and processor type
    * options for optimzing compute costs
        - On-Demand Instances, Reserved Instances, Spot Instances
        - Savings Plans
    * Ability to run any type of workload
        -simple websites
        -Enterprise applications
        -High performance computing (HPC) applications

## Provisioning an EC2 Instance
    * essential instance launch config parameters
    * must make key decisions about config details
        -main para need to specify to launch secure instance include 
            1. AMI:  defines base SW for instance
            2. Instance Type: combo of CPU, memory, storage and networking capacity
            3. VPC:  where want instance to run in the network; what network address it should have and what levels of access and security it requires
            4. Assumed Roles:  attach roles via IAM; the EC2 instance assumes the IAM role and uses it to grant users and applications on the instance permissions to access AWS services
            5. User Data:  customize instance config by specifying a user data script; runs auto when instance is launched; use for auto patching the OS or installing a web server when instance first comes online
            6.  Instance Store or Amazon EBS:  need to specify the type of storage used for storing the root volume of instance and any additional storage volume would like to have
            7. Security Group:  configure a new security group or use an exisitng one; defines which network ports traffic is allowed on
            8. Key Pair:  encryption key pair is usually also specified at launch; used for secure shell and remote desktop protocol connections to instances
            

## Choosing an AMI to launch an EC2 Instance
    * can create an AMI from EC2 instance
    Steps:
        1.  start w/ source AMI (from any of the 4 sources).  launch an EC3 instance from the AMI.  At this point have an *unmodified instance
        2.  Connect to the instnace and configure it w/ specific OS and application settings.  Results is referred to as *Golden Instance
        3.  Capture as a new AMI; for an EBS-backed AMI you capture it by creating a new image and AWS auto registers it for you; for instanced backed AMI, you capture it by using EC2 AMI tools to create a bundle for the instance root volume then upload the bundle into an S3 bucket then you must manually register it.  After AMI is registered can use it to launch new instances in same AWS region.  Think of it as a new Starter AMI
        4.  optional. copy new AMI to other Regions; lets you launch new EC2 instances in those locations using yoru AMI
    * another way to create AMI is to use EC2 image builder
    *EC2 Image Builder
        automates creation, management and deployment of up-to-date and compliant Golden VM Images
        *provides a GUI to create image-building pipelines
        *creates/maintains EC2 AMIs and on-premises VM images
        *Produces secure, validated and up-to-date images
        *Enforces version control
        *benefit is can create images w/ only the essential components
        *reduce exposure to security vulnerabilities
        *before use images in production, can use EC2 Image Builder to validate your images w/ tests provided by AWS or can use own tests
    * How EC2 Image Builder Works
        *Image Pipeline
            Specified Source Image --> One or more added build components to customize SW installation and config --> one or mroe added hardening test (provided by AWS or custom) to verify security on the image --> Configured build schedule to specify how often the image pipeline will produce new images --> New Golden Image --> defined automated distribution details (specify to which AWS REgions to distribute the golden image)


## Selecting an EC2 Instance Type
    * EC2 type defines configuration of CPU, memory storage and network performacne characteristics that proves a given level of compute performance
    * Instance type you choose will depend on workload's performance and cost requirements
    * names follow a standard convention; has multiple parts that describe the different characteristics
        m5d.xlarge
           m:  family
           5:  generation; generally instances w/ higher generation numbers are more powerful and provide better value for the price 
           d:  additional capabilities; optional part that indicates additional capabilities of the instance type; i.e. d indicates that the instance type uses a solid state drive (SSD) for the the root EBS volume instead of standard HDD
           xlarge:  size; defines performance specs of an instance across categories for CPU, memory storage and network performance
    * Instance types can be categorized as General Purpose, Compute optimized, Memory optimized, Accelerated computing or Storage optimized
            1.  General Purposes Instances
                provides a balance of compute, memory and networking resources
                Web/app servers
                Enterprise applications
                Gaming servers
                Cachinng fleets
                Analytics applications
                Development or test environments
                Example instance types:  M5, T3, or A1 Instances
            2.  Compute Optimized Instances
                work well for compute-bound applications and benefit from high-performacne processors
                batch processing
                distributed analytics
                high performance computing (HPC)
                ad server engines
                multiplayer gaming
                video encoding
                Example instance types:  C5, C5n Instances
             3. Memory Optimized Instances
                deliver fast performance for wkloads that process large data sets and memory
                in-memory caches
                High-performance databases
                bid data analytics
                Example instance types:  R5, X1, HMI
             4. Accelerated Computing Instances
                ML
                AI
                HPC
                Grphics
                Example instance types:  P3, G4, F1
            5.  Storage Optimized instance types
                designed for workloads that require high sequential read/write access to very large data sets on local storage
                optimized to deliver tens of thousancds of low-latency random IOPS
                high performance databases
                real-time analytics
                transactional workloads
                noSQL databases
                big data
                data warehouse
                log processing
                Example isntances types:  I3, D2, H1 
     
  * Choosing an Instance Type
        Choose instance type that meets
            1.  Performance Needs of application
            2.  Cost requirements

        When create a new instance
            1. Filter by the characteristics that you choose 
                        - use the Instance Type page in the EC2 Console
                           the page displays all the available instance types in a Region and provides search and filtering capabiliteis based on attribute values
            3. the latest generation in an instance family typically has a better price-to-performance ratio
            

        if already have an existing instance
            1.  get recommendations for optimziing the instance type from the AWS Compute Optimizer service to get recommendations on how to optizime the instance type
            2.  evaluate recommendations and modify instance as recommended


* AWS Compute Optimizer
        AWS services that recommends optimial instance type, isntance size and auto scaling group config
        analyzes workload patterns and makes recommendations
        classifies instance findings as Under-provisioned, Over-provisiioned, Optimzied or None
        analyzes the config and utilization
        uses Amazon ML to analyze workloads
        generates EC2 instance type and size recommendations
        when activated, will analyze running AWS compute resrouces and deliver recommendations
        classifes findings for EC2 instances as Under-provisioned, Over-provisioned, optimized or none
            "None" can occure if Compute Optimizer has been enabled for less than 12 hours, when instance has been running for less than 30 hours or instnace type isn't supported by Compute Optimizer
                
  ## Using User Data to Configure an EC2 Instance
  
  * When launching an EC2 instance have the option of passing user data into it     
  * With user data can use a script to initialize and EC2 instance auto when it starts
  		** i.e. use user data to patch or update SW that's already installed on the instance from the AMI; fetch and install SW license keys or install additional SW
  * when launching an EC2 instance, specify user data to run an initializtion script (shell script or cloud-init directive)
  * MS Windows uses EC2 Config or EC2Launch tools to process user data; this includes PowerShell Scripts
  		** Windows 2016 or new uses EC2Launch
		** Older version include EC2Config
	* Retrieving Instance Metadata
		-instance metadata is informatoin about your instance
		-accessible from your instance URL
		-can be retrieved from a user data script
		-can find info like public IP address, private IP address, public hostname, isntance ID, security groups, Region, AZ, user data that was specified at launch time and more
  * Configuring an EC2 Instance:  AMI vs User Data
  		- need to make important architectural decisions
  		- how much of instnace config should you pre-install in a base AMI?
  		- how much should you dynamically build with user data at boot time?
  		- Can build an AMI that contains all the configs for the instance (full baked AMI or full AMI)
  				** includes everything to serve workload, including the OS, app runtime SW and app itself
				** provisions a fully functional instance w/o additional boot-time config
		- Can build an AMI that simply contains a minimal OS (Just Enough OS AMI or JeOS AMI)
				** includes a config management agent that builds a fully functional system at instance launch
				** on first boot, the config agent downloads, installs, configures and integrats all required SW
		- Can also take a hybrid approach on building an AMI
				** build an AMI that contains subset of config that workload needs
					- i.e. an AMI can contain the OS and app runtime SW only or only the OS
				** uses user data to complete the instance's config on first boot based on the app requirements
		- Discovering ideal approach usually results from considering hte trade offs between simplicty and flexibility
		- also need to consider boot times
				** an AMI w/ OS only config will take a long time to boot when launch a new instance because users data will need to run
				** packaging pre-reqs into a custom AMI shortens boot times
		- also need to consider shelf life
				** when install more pre-reqs on an AMI, run a greater risk that your app will be vulnerable to a security risk if underlying AMI isn't kept up to date
				** assess the risk posed by updates to your dependencies
	* Tradeoffs
		- with a full AMI, the apps and all dependencies are pre-installed which shortens boot-times but increases AMI build times
		- a full AMI usually has a shorter lifespan becasue updates to SW in it will need a rebuild
		- with hybrid AMIs only pre-req SW and utilities are pre-installed which leads to a longer shelf life for the AMI
		- Hybrid AMI provides a balance between boost speed and AMI build time; rollbacks become easier
		- w/ an OS-only AMI, the approach is fully configurable and upgradable over time and shortens AMI build times but makes EC2 instances slow to boot becasue all required installations and config must be run at boot time
	* Many organizations decide on a hybrid approach where some configs are baked into a custom base AMI and other settings are configured dyanmically at launch


## Section 6:  Adding Storage to an Amazon EC2 Instance
	* EC2 Instances have 4 main storage options
		1.  Instance Store
		2.  Amazon EBS
		3.  Amazon Elastic File System (EFS)
		4.  Amazon FSx for Windows File Server
	* Can use any of these options to store a data volume
	* only instance sotre or SSD-backed EBS volume can contain a root volume
	* Instance store can only be used by the instance itself; think of it as the Hard Disk inside your computer
	* EBS can be attached to once instanced at a time but you can detach and move to a different instance; it's like an external hard disk for your computer
	* if want to share a data volume among multiple instances at same time can use Amazon EFS for Linux instances; use Amazon FSx for Windows File Serve rfor MS Windows instances
	*Instance Store (ephemeral storage)
		- An instance store provides non-persistent storage to an instance --> the data is stored on the same physical server where the instance runs
		- Characteristics
			1.  Temp block-level storage
			2.  uses HDD or SSD
			3.  Instance store data is lost when instance is stopped/terminated
		- example use cases
			1.  Buffers
			2.  Cache
			3.  Scratch data
		- consists of 1+ volumes that are exposed to block devices
		- the instance type determins the available sizes for an instance store and type of HW that you use for these instance store volumes
		- most instance types support instances stores but not all do
		- instances store backed instances cannot be stopped 
			** can only be rebooted or terminated
		- if instance reboots, data in the instance store persists
		- if instances is terminated, you lose data in the instance store
		
	* Amazon EBS
		- provides network-attached persistent storage to EC2 instance
		-Characteristics
			1.  persistent block-level storage
			2.  can attach to any instance in the same AZ
			3.  uses HDD or SSD
			4.  can be encrypted
			5.  supports snapshots that are persistent to S3
			6.  Data persistent independtnly from instance and can be encrypted
		- Example use cases
			1.  Stand-alone database
			2.  general application data storage
	* EBS SSD-backed Volume Types
		- EBS SSD-backed volumes are suited for use cases where the performance focuse is on IOPS
		- General Purpose SSD (gp2)
			* default EBS volume type for EC2 instances
			* balances price and performance for a wide variety of worklads
			* Use cases
				1.  Recommended for most workloads
					** Examples of transactional workloads suite for
						1. development or test environments
						2. low-latency interactive apps
						3. boot volumes
						
				2.  can be a boot volume
		- Provisioned IOPS SSD (io1)
			* highest-performance SSD volume
			* good for mission-critical, low-latency or high-throughput workloads
			* Use cases
				1.  Critical business apps that require sustained IOPS performance
				2.  Large db workloads
				3.  Transactional workloads
				4.  Can be a boot volume
	* EBS HDD-backed volume types
		- EBS HDD-backed volumes work well when the focus is on throughput
		* Throughput Optimized HDD (st1)
			- Low-cost volume type
			- Designed for frequently accessed, throughput-intensive workloads
			-Use Cases
				1.  Streamking workloads
				2.  Big data
				3.  Data warehouse
				4.  Log processing
				5.  It CANNOT be a boot volume
		* Cold HDD (sc1)
			- Lowest-cost HDD volume
			- Designed for less frequently accessed workloads
			- Use cases
				1.  Throughput-oriented storage for large volumes of infrequently accessed data
				2.  Use cases where the lowest storage cost is important
				3.  It cannot be a boot volume

	* EBS-Optimized Instances
		- certain EC2 types can be EBS-optimized
		- Benefits
			1.  Provides a dedicated network connection to attached EBS volumes
			2.  Increases I/0 performance
			3.  Additional performance is achieved if using an Amaozn EC2 Nitro System-based instance type
				* the Nitro System is a collection of AWS built HW and SW components that reduce virtualization overhead through high performance, high availability, high security and bare metal capabilities
		- Usage
			1.  For EBS-optimized instance types, optimization is enabled by default
			2.  For other instance types that support it, optimization must be manually enabled
		- for EBS-optimized instance types optimization is enabled by default
		- for othe EC2 instnace types that support this, you must manually enable it when you launch the instance by setting the Amazon EBS-optimizaed attribute
	* Shared File Systems for EC2 Instances
		- What if you have multiple instances that must use the same storage?
			-- EBS attached to only one instance
			-- S3 is an option but not ideal
			-- EFS (Linux) and FSx (Windows) for Windows File Server satisfy this requirement
	* Amazon EFS
		-provides a file system storage for Linux-based workloads
		-fully managed elastic file system
		-scales auto up or down as files are added and removed
		-Petabytes of capacity
		-Supports Network File System (NFS) protocols
			-Mounts the file system to the EC2 instance
		-Compatible w/ all Linux-based AMIs for EC2
		- provides full file system features such as strong read after write cosnsitency and file locking
		- Linux instances use NFS to connect to an EFS file system
		- EFS can be used w/ any AMIs that support NFS
		- multiple EC2 instances can acces an EFS file system at the same time
			-- allows EFS to provide a commond data source for workloads and apps running one 1+ app system
		- Use Cases
			1.  Home directories
			2.  File system and enterprise applications
			3.  Application testing and development
			4.  Databse backups
			5.  Web serving and content management
			6.  Media workflows
			7.  Big data anlaytics
	* to connect the EFS file syste to the guest OS of your instance must mount it

* FSx for Windows File Server
	- provides fully managed shared file system storage for Windows EC2 instances
	- native Windows compatibility
	- Net Technology File System (NTFS)
	- Native Server Message Block (SMB) protocol version 2.0 to 3.1.1
	- Distributed File System (DFS) Namespaces and DFS Replication
		** DFS is a service that enables organiztion of multple distributed SMB file shares into a distributed file system
	- integrates w/ MS Active Directory and suppors Windows access control lists (ACLs)
	- Backed by high-performance SSD storage

* FSx for Windows File Server Use Cases
	- supports a broad set of MS workloads
		** ie.
			1.  Home directories
			2.  Lift-and-shift app workloads
			3.  Media and entertainment workflows
			4.  Data analytics
			5.  Web serving and content management
			6.  SW development environments


## Key Takeaways
	1.  Storage options for EC2 instances include
		1.  Instance Store
		2.  EBS
		3.  EFS
		4.  FSx for Windows File Server
	2.  for a root volume, use instnace store or SSD-backed EBS
	3.  for a data volume that serves only one instance, use instance store or EBS storage
	4.  for a data volume that serves multip Linux instance use EFS
	5.  for a data volume that serves multiple Windows instances use FSx for Windows File Server
	
	
## Section 7:  Amazon EC2 Pricing Options
	* 5 ways to pay for EC2
		1.  On-Demand Instances
			* Pay for compute capacity by the second or by the hour 
			* No longer-term commitments
			* for spiky workloads or workload experimentation
		2.  Reserved Instances
			* Make a 1 year or 3 year commitment and receive a significant discount off on-demand prices
			* for committed and steady-state workloads
		3.  Savings Plan
			* Same discounts as Reserved Instances with more flexibility in exchange for a $/hour commitment
			*All EC2, Fargate and Lambda workloads
			*flexibile pricing model
		4.  Spot Instances
			* spare EC2 capacity at substantial savings off on-deamnd instance prices
			* for fault-tolerant, flexibile, stateless workloads
			* must be able to be stopped and started
			* Amazon can interrupt Spot Instance w/ a 2 minute notification
			* runs when capacity is available and the maximum price per hour for request exceeds the spot price
			* if running on Linux or Ubunto OS they're built in 1 second increments w/ a minimum of 60 seconds
			* all other OS are built in one hour increments rounded up to the nearest hour
			* SPOT BLOCKS:  variation of Spot Instances let you run a workload continuously for a finite duration of one to six hours; are not interrupted; will run for the duration selected, independent of the spot instance market price
		5.  Dedicated Hosts
			* physical server w/ EC2 instance capacity fully dedicated for your use
			* for workloads that require use of own SW licenses or single tenancy to meet compliance requirements

	EC2 Dedicated Options
		- EC2 dedicated options provide EC2 instance capacity on physical servers that are dedicated for your use (single-tenant HW)
		- DEDICATED INSTANCES
			*per instance billing
			*auto instance placement
			*Benefit:  isolates the hosts that run your instances
		- DEDICATED HOSTS
			* for host billing
			* visibility of sockets, cores and host ID
			* affinity between host and instance
			* targeted instance placement
			* add capacity by using an allocaiton request
			* Benefit:  enables you to use your server bound SW licens and address compliance requirements
	EC2 Cost Optimization Guidelines
		- to optimize the cost of EC2 instances, combine the available purchase options
			* use RI or Savings Plans for known, steady-state workloads
			* use On-Demand Instances for new or stateful spiky workloads
			* scale using Spot Instances for fault-tolerant, flexibile, stateless workloads
	KEY TAKEAWAYS
		-EC2 Pricing Models include On-Demand Instances, RI, Savings, Plans, Spot Instances, and Dedicated Hosts
		-Per-second billing is avail oly for On-Demand Instnaces, RI and Spot Instances that run on Linux or Ubunto OS
		-Use a combo of RI, Savings Plans, On-Demand Instances and Spot Instances to optimized EC2 compute costs
		

## EC2 Considerations
	PLACEMENT GROUPS
		* enable you to control where instances run in an AZ
		* they influence where a group of interdependent instances run -
			- increase network performance between them
			-reduce correlated or simultaneous failure
	PLACEMENT STRATEGIES
		*Cluster
			-packs instances close together inside an AZ
			-helps workloads achieve low latency network performanc	
		*Paritition
			-spreads instances across logical partitions
			-groups of instances in one partition don't share the underlying HW w/ group of instances in another partition		
		*Spread
			- strictly places a small group of instances across different underlying HW to reduce correlated failures
	LIMITATIONS
		*an instances can be launched in only on placement group at a time
		*instances launched on a dedicated host can't be launched in a placement group
		*instances w/ a tenancy of host cannot be launed in a placement group
	CLUSTER PLACEMENT GROUP
		*provides low-latency and high packet-per-second network performance between instances in the same AZ
		*instances are placed in the same high-bisection bandwidth segment of the network
		*provides per-flow throughput limit of up to 10 Gbps for TCP/IP traffic
		*Recommend for app that benefit from low network latency, high network throughput or both
		*Best Practice - Launch all instances in a single request; if try to add more instances to group laters increase chances of receiving an insufficient capacity error
	PARTITION PLACEMENT GROUP
		*partitions (groups divided into logical segments by EC2) placement group spreads instances across logical parittion to reduce likelihood of correlated HW failure
		*each partition has its own set of racks (network and power source)
		*each rack has its own network and power source
		*paritions can be in multiple AZ
		*recommended use for large distributed and replicated workloads like Apache Hadoop, Apache HBase, and Apache Cassandra
		*can have paritions in multiple AZ in the same region
		*can have a max of 7 partitions per AZ
		*the instances in a partition don't share ranks w/ the instances in other paritions so can limit impaced of single HW failure to only the associated partition
	SPREAD PLACEMENT GROUP
		* place instances across distinct physical racks to reduce correlated HW failure
		* each rack has its own network and power source
		* Groups can span multiple AZ
		* Recommended use for apps that have a small number of critical instances that should be kept separate from each other
		*can span multiple AZ up to 7 instances per AZ per group
		
		
		
		
				 		
				
				
				
    
        
        

        

