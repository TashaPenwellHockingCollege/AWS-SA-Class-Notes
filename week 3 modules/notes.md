# Choosing AMI to launch EC2 instance

# Amazon Machine Image (AMI)
  * provides the information that is needed to launch an instance
      1. Template for the root volume
          a. contains the guest OS and other installed SW
      2. Launch permissions
          a. controls which AWS accounts can access the AMI
      3. Block device mappings
          a. specifies any storage volumes to attach to the instance
          
  * Benefits
      1.  Repeatability
            * AMI can be used repeated to launch instances with efficiency and precision
      2.  Reusability
            * iNSTANCES LAUNCHED FROM SAME AMI are identically configured
      3.  Recoverability
            * can create an AMI from a configured instance as a restorable backup
            * can replace a failed instanced by launching a new instance from same AMI

  * Choosing an AMI
      -Based on 5 things
          1.  Region
          2.  OS
          3.  Storage type of the root device
          4.  Architecture
          5.  Virtualization Type (PV or HVM)


  * 4 Sources to get started with AMIs
        1. QuickStarts
        2. Create own AMI from EC2 instances
        3. AWS Marketplace
        4. Community built AMIs (not vetted by AWS)


+++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++


# AWS Runtime Compute Choices
  1.  Virtual Machines (VMs)                              
       a. EC2                                                            
       b. Lightsail                                                      
  2.  Containers                                                          
       a. Elastic Container Service (ECS)                              
  3.  Platform as a Service (PaaS)
       a.  Elastic Beanstalk
  5.  Serverless
       a.  Lambda
       b.  Fargate
  6.  Specialized Solutions
       a.  AWS Outposts
       b.  AWS Batch
       
       
       
# EC2
   * Provides VM (servers)
   * Provisions servers in minutes
   * Auto scale capacity up/down as needed
   * Enables you to pay only for capacity that you use
   * VM that runs on a physical host
   * You choose different configurations of CPU and memory capaicty
   * Supports different storage options (Instance store or Elastic Block Store)
   * Provides network connectivity


# EC2 Use Cases
  * When need complete control of computing resources from OS to processor type
  * Options fro optimizing compute costs
      1.  On-Demand instances, Reserved Instances and Spot Instances
      2.  Savings Plan (commit to using specific capacity over long-term)
  * Ability to run any type of workload
     * Simple websites
     * Enterprise Applications
     * High performance applications



 # Provisioning EC2 Instance
       *Security group
       *Key pair
       *AMI
       *Instance Type
       *VPC
       *Assumed role
       *User data
       *Instance store or Amazon EBS
       
       
       
++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++

# Choosing an AMI to launch EC2 Instance
    Steps:
       1. Select your Starter AMI (Quick Start or other exising AMI)
       2. Launch an EC2 instance from the AMI to create an UNMODIFIED INSTANCE
       3. Connect to the instance adn manually modify it or run a script that modifies the instance (i.e. upgrade installed SW) to create a MODIFIED ("GOLDEN") INSTANCE
       4. Capture as a new AMI
              *for EBS backed do this by creating a new image and AWS automatically registers it for you
              *for Instanced-backed AMI, capture it by using EC2 AMI tools to create a bundle for the instance root volume

