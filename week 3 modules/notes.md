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
        
