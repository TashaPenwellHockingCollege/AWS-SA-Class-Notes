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
