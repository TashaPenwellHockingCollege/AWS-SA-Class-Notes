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


 # Creating a public subnet
  * Internet Gateway
           - allows communication between resources in your VPC and the Internet
           - horizontally scaled, redundant, and highly available by default
           - provides a targe in yoru subnet route tables for internet-routable traffic
           - supports IPv4 adn IPv6 traffic
           - Serves 2 purposes
                  1.  provides a target in your VPC route tables for Internet-bound traffic
                  2.  for instances assigned public IPv4 addresses, the IGW performs network address translation (NAT)
            -to make a subnet public, must first create an IGW and attach to VPC
   * Directing Traffic Between VPC Resources
           - route tables are required to direct traffic between VPC resources
           - each VPC has a main (default) route table 
                        * created automatically when you create the VPC
                        * at first every route table (including the main one) contains only a single local route
                        * the local route enables communications for all the resources in the VPC
                        * cannot modify local route in a route table
                        * when launch an instances in the VPC the local route auto covers that instance
                        * don't need to add the new instance to a route table
                        * can create additional custom route tables for your VPC
           - all subnets must be associated w/ a route table
                        * if don't explicitly assocate a subnet with a custom route table the subnet is implicity associated w/ the main route table
                        * a subnet can be associated w/ only one route table at a time
                        * multiple subnets can use the same route table
           - you can create custom route tables
            - Best Practice:  use custom route tables for each subnet to allow granular routing destination

# Remapping an IP address from one instance to another
      * Elastic IP Addresses
            - are static, public IPv4 addresses associated w/ yoru AWSaccount
            - can be associated w/ an instance or elastic network interface
            - can be remapped to another instnaces in your account (this allows consistent availability)
            - are useful for redundance when load balances are not an option
            - associating the Elastic IP address w/ a network interface has an advantage over associating it directly w/ the instance
                  ** can move all the attributes of the network interface from one instance to another in a single step


# Connecting Private Sunets to the Internet
      * NAT Gateways
            - enable instances in a private subnet to initiate outbound traffic to the internet or other AWS services
            - provents private instances from receiving inbound connection request from the internet
      * Creating  a NAT Gateway
            1.  Specify the public subnets tha twant to create it; in the public route table, the IGW is specified as the target for internet-bound traffic
            2.  Specify an Elastic IP address to associate w/ the NAT gateway
            3.  Update the private route table that is associated /w one or more of yoru private subnets so it points internet-bound traffic to the NAT Gateway
                  -NOTE: in the private route table, the NAT gateway ID is specified as the target for internet-bound traffic
            4.  Instances in private subnets will now be able to communicate w/ the Internet
      * AWS Recoomendation:  put web app instances inside a private subnet behind a load balancers that's placed in a public subnet
            * in some environments, need to attach web app instances to Elastic IP addresses directly; 
            * can also attach an Elastic IP address to a load balancer but in those cases the web app instances must be in a public subnet
            
 # Bastion Hosts
      * a server whos purposes is to provide access to a private network from an external network
      * must minimize the chances of penetration
      * usually runs on EC2 isntances in public subnet of your VPC
      * bastion host users connct to the bastion host to connect to the private subnet of instances
            
# Key Takeways
      1.  an IGW allows communciations between instances in your VPC and the Internet
      2.  Route tables control tgraffic from your subnet or gateways
      3.  Elastic IP addresses are static, public IPv4 addresses that can be associated w/ an instances or Elastic network interface.  They can be remapped to anothe rinstances in your account
      4.  NAT gateways enable instances in the private subnet to initiate outbound traffic to the internet or othe AWS services
      5.  A bastion host is a server whose purpose is to provide access to a private network from an external network such as the internet
                  

# SECURING YORU AWS NETWORKING ENVIRONMENT
      * Security Groups
            - are stateful firewalls that control inbound and outbound traffic to AWS resources
                  * divide instances and network interfaces into groups
                  * security group rules control inbound and outbound traffic to group members so configure the rules to restrict traffic and allow access only as needed
                  * traffic can be restricted by any IP, service port and sources or destination IP address
                  * stateful means that state info is kept even after a request is processed so if you send a request from your instances the response traffic for that request is allowed to flow in regardless of inbound security group rules
                  * responses to allowed inbound traffic are allowed to flow out regardless of outbound rules
            - act at the level of the instance or network interface
      * Default Security Groups
                  * when you create a security group, it has no inbound rules so you must add inbound rules to the security group
                  * these rules allow inbound traffic that originates from another host to reach your instance
                  * be default, a newly created security group includes an outbound rule that allows all outbound traffic
                  * you can remove the rule and add outbound rules that allow specific outbound traffic only
                  * if there are no outbound rules then no outbound traffic is allowed
      * Custom Security Group
            * when creating sec group can specify allow rules but not deny rules
      * Chaining Security Groups
                  * sec groups act as firewalls
                  * Three different tiers
                              1.  INBOUND RULES => Web Tier Security Group
                                    - Allow:  HTTP (port 80) or HTTPS (port 442)
                                    - Source: 0.0.0.0/0 (any)
                                    - Allow: SSH (port 22) to Web tier
                                    - Source:  Corporate IP range
                              2.  INBOUND RULE => Application Tier Security Group
                                    - Allow:  SSH (port 22) to Application Tier
                                    - Source:  Corporate IP Range
                              3.  INBOUND RULE => Database tier security group
                                    - Allow:  SSH (port 22) to DB tier
                                    - Source:  Corporate IP Range
                                          
                        All other ports are blocked by default
                  * can be configured to set different rules for different classes of instances
      * Network Access Control Lists (Network ACLs0
                  * act at the subnet level
                  * allow all inbound and outbound traffic by default
                        - can setup network ACL w/ rules similiar to sec group
                        - by default, allows all inbound and outbound IPv4 traffic and (if it applies) IPv6 traffic
                  * are stateless firewalls that require explicit rules for both inbound and outbound traffic
                        - no info about a request is maintained after a request is processed
                        - return traffic must be explicity allowed by rules
                  * optional layer of security for the VPC
                  * controls traffic going into and out of one or more subnets
                        - your VPC auto comes w/ a modifiable default network ACL
                        - has separate inbound and outbound rules
                        - each rule can allow/deny traffic
                  * each subnet in VPC must be associated w/ a network ACL and can be associated w/ only one network ACL at a time
                        - however, can associate a network ACL w/ multiple subnets
                  * when associate a network ACL w/ a subnet, it removes the subnets' previous association

      * Custom Network ACLs
            * Recommended for specific network security requirements only
            * be default denies all inbount and outbound traffic until add rules
      * Structure your Infra w/ Multiple Layers of Defense
            * best practice is secure infra w/ multiple layers of defense
            * by running infra in VPC can control which instances are exposed to the Internet
            * can protect infra at diff levels
                  - define sec groups to provide protection at the infra level
                  - define network ACLs to further protect infra at the subnet level
                  - secure instances w/ a firewall at OS level
                  - follow other sec best practices
      * Review:  How to Create a Public Subnet
            * to create a public subnet to allow communication between instances in VPC and Internet must
                  1.  Attach an IGW to your VPC
                  2.  Point your instance subnet's route table to the IGW
                  3.  Make sure that instances have public IP or Elastic IP addresses
                  4.  Make sure that your security gruops and network ACLs allows relevant traffic to flow

# Key Takeaways
      * Security groups are stateful firewalls that act at the instance level
      * Network ACLs are stateless firewalls that act at the subnet level
      * when set inbound and outbound rules to allow traffic to flow from the top tier to the bottom tier of your architecte, you can chain security groups teogether to isolate a security breach
      * you should structure your infra w/ multiple layers of defense
            
