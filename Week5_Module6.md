# VPC
 * services that enables you to provision a logically isolated section of the AWS Cloud where you can launch AWS resources in a virtual network that you define
 * Build your own network
      -IP addresses
      -subnets
      -routing rules
      -network
      -security rules
  * belongs to a single AWS region; can be deployed in any AWS region
  * can host supported resources from any AZ w/in its Region
      - VPC spans all the AZ in its Region
 
 # CIDR
  * when create a VPC, your provide the set of priviate IP addresses that you want instances in your VPC to use
  * specify the set of addresses through Classless Inter-Domain Routing (CIDR)
  * can assign IP addresses w/ a a CIDR of /28 which equals 16 IP addresses or a range down to /16 to provide 65,536 IP addresses
  * VPC supports IPv4 and IPV6 addressing and has different CIDR block size limits for each
  * be default all VPCs and subnets must have IPv4 CIDPR Blocks; can optional associated an IPv6 CIDR block w/ your VPC
  * VPC cna operate in dual-stakc mode:  resources can communicate over IPv4, IPv6 or both
  * IPv4 and IPv6 are independent of each other so will need to configure routing and security in VPC for IPv4 and IPv6 separately

# Subnets:  Dividing your VPC
  * subnet is a segment or partition of a VPC's IP address range where you can allocate a group of resources
  * Subnets are not isolation boundaires; they are containers for routing policies
  * Subnets are a subset of the VPC CIDR block; when you create a subnet you specify the CIDR block for the subnet and is a subset of the VPC CIDR block
  * Subnet CIDR blocks cannot overlap
  * Each subnet resides entirely w/in one AZ and cannot span AZ
  * You can add on or more subnets in each AZ or in a Local Zone
  * AWS reserves 5 IP addresses in each subnet
        - first 4 IP address and last IP address in each subnet CIDR block (i.e. if have a CIDR block w/ 256 IP addresses, only 251 are available)
  * because VPC subnets are mapped to specific AZ, subnet placement is one way to properly distribute EC2 instances across multiple locations


# VPC Design Best Practices
  * Create on subnet per available AZ for each group of hosts that have unique routing requirements
  * Divide your VPC network range evenly across all available AZ in a Region
  * Do not allocate all network addresses at once.  Instead, ensure that your reserver some address space for future use.
  * Size your VPC CIDR and subnets to support significant growth for the expected workloads
  * Ensure that your VPC network range (CIDR block) does not overlap w/ your organizations' provide network ranges

# Single VPC Deployment
  * There are limited use cases where deploying one VPC might be appropriate
  * Small, single applications managed by a small team
  * High performance computing (HPC)
  * Identity management
  * for most use cases, there are 2 primary patterns for organizing your infra:  multi-VPC and multi-account

# Multiple VPCs
  * Best suited for
      1.  Single team or single org, such as managed service providers
      2.  Limited teams, which makes it easier to maintain standards and manage access
  * Exceptions
      - Governance and compliance standards might require greater workload isolation

# Multiple Accounts
  * Best suited for 
      1.  Large org adn org w/ multiple IT teams
      2.  Medium-sized org that anticipate rapid growth
  * Why?
      - it can be more challenging to manage access and standards in more complex org

# Amazon VPC quotas
  * Default Quota:  5 VPCs per Region per account but you can request a quota increase

# KEY TAKEAWAYS
  1.  Amazon VPC enables you to provision VPC, which are logically isolated sections of the AWS Cloud where you can launch your AWS resources
  2.  a VPC belongs to only on Region and is divided into subnets
  3.  a subnet belongs to one AZ or Local Zone.  It is a subnet of the VPC CIDR block
  4.  You can create multiple VPCs in the same Region or in different Regions and in the same account or different accounts
  5.  Follw best practices when you design your VPC
