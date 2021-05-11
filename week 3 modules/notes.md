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


*
