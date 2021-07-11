# AWS Site-to-Site VPN
  * AWS Site-to-Site is a highly available soluation that enables you to securly connect your on-premises network or branch office site to your VPC
  *   uses internet protocol security (IPSec) communications to create encrypted VPN tunnels
  *   Provides 2 encrypted tunnels per VPN connection
  *   Charged per VPN connection-hour

# Static and Dynamic Routing
  * Static Routing
      * Requires you to specify all routes (IP prefixes)
      * Specify static routing if your customer gateway device does not support BGP
  * Dynamic Routing
      * Uses the Border Gateway Protocol (BGP) to advertise its routes to the virtual private gateway
      * Specifies dynamic routing if your customer gateway device supports BGP
      * recommend use BGP-capable devices becasue the BGP protocol offers robust liveness detection checks which can assist failover to the second VPN tunnel if the first tunnel goes down
      * devices that don't support BGP may also perform health checks to assist failover when needed

# Connecting Multiple VPNs
   * to maintain high availability on customer gateways can set up redundant customer gateway devices
   * Redundant customer gateway devices advertise the same prefix to the same virtual private gateway
   * AWS uses BGP routing to determine the path for traffic
   * if 1 customer gateway device fails, the virtual private gateway directs all traffic to the working customer gateway device
   * can use AWS VPN CloudHub to establish multiple VPN connection from multiple customer gateway devices toa single virtual private gateway
   * use config in different way to implement redundancy and failover on your side of the VPN connection

# Key Takeaways
1.  AWS Site-to-Site VPN is a highly available solution that enables you to securely connect your on-premises network or branch office site to your VPC
2.  AWS Site-to-Site VPN supports both static and dynamic routing
3.  You can establish multiple VPN connections from a multiplie customer gateway devices to a single virtual private gateway


# AWS Direct Connect (DX)
* provides you w/ a dedicated, private network connection capacity of either 1 Gbps OR 10 Gbps
* reduces data transfer costs
* improves application performance with predictable metrics

# DX Use Cases
* hybrid environments
     * network transfers won't compete w/ internet bandwidth at the data center
* Transferring large datasets
     * high bandwidth link reduces network congestion and improves app performance
     * reduce network fees from Internet connection that you pay to Internet Service Provider
     * data that is transferred over DX is charged at reduced DX data transfer rate instead of internet data transfer rates which can lower network costs
* Network performance predictability
     * for apps that require predictable network performance (i.e. audio, video streams that use real time data feed)
* Security and compliance
     * meet policies that require private network connections

# Enabling High Availability:  DX with backup VPN Connection
 * it's possible to configure the DX connection as  the primary path and the lower cost VPN connection as the backup
 * other options include
   1.  using multiple DX circuits and multiple VPN tunnels between separately deployed private IP address spaces
   2.  use multiple DX location for high availability
 * if use multiple AWS regions will also need multiple DX locations in at least 2 Regions
    * may consider using AWS Marketplace appliances that terminate VPNs

# Enabling High Resiliency for Critical Workloads w/ DX
 * Highly resilient, fault-tolerant network connections are key to a well-architected system
 * Redundant DX connnections are a good option
 * Things to consider for high resiliency of critical workloads
    1.  connecting from multiple data centers for physical location redundancy
    2.  Consider using redundance HW and telecommunications providers between remote connections
    3.  Best practice to use dynamically routed active/active connections for auto load balancing and failover across redundant network connections
    4.  Provision sufficient network capacity so the failure of one network connection doesn't overwhelm and degrade the redundant connections
    5.  for critical production workloads that require high resiliency, recommend having one connection at multiple locations to ensure resilience against connectivity failures due to HW failure or complete physical location failure

# Key Takeaways
1. AWS DX uses open standard 802.1q VLANs that enable you to establis a dedicated, private network connection from your premises to AWS
2. You can acess any VPC or public AWS service in an Region (except China) from you supported DX location
3. You can implement highly available connectivity between your data centers and your VPC by coupling one or more DX connections that you use for primary connectivity w/ a lower-cost, backup VPN connection
4. To implement a highly resilient, fault-tolerant architecutre, connect to your AWS network form multiple data centers so you can have physical location redundancy


## Connecting VPCs in AWS with VPC Peering
# Connecting VPCs
 * isolating some workloads is generally a good practice
 * may need to transfer data between two or more VPCs
 * common example of having multiple VPC environments is having one for Development, Test, and Production


# VPC Peering
 * One to One networking connections between 2 VPCs
    ** lets you route traffic between the VPCs privately
    ** Instances in either VPC can communicate w/ each other like they are in the same network
    ** can create a VPC peering connection between own VPCs, w/ a VPC in another AWS account or w/ a VPC in a different AWS Region
    ** inter-region VPC peering provides simple and cost-effective way to share resources between Regions or replicate data for geographic redundancy
 * no gateways, VPN connections adn separate network applicances needed
    ** w/ inter-Region VPC peering cloud resources communciate w/ each other using private IP addresses w/o requiring gateways, VPN connections or separate network applicainces
    ** traffic remains in the private IP address space
 * Highly available connections
 * No single point of failure or bandwidth bottleneck
    ** all inter-Region traffic is encrypted w/ no single point of failure or bandwidth bottleneck
 * Traffic always stays on the global AWS backbone; never goes across public internet
    ** reduces threats such as DDOS
 * Standard inter-region data transfer rates are charged for inter-Region VPC peering connections

# Establishing VPC Peering
 * frist VPC owner sends request to second VPCs owner
 * Second VPC owners must accpet peer request
 * to enable a flow of traffic between the VPC peers through private IP addresses, each owner must add a route to one or more route tables
 * route must point to the IP address range of the peer
 * may also need to update security group rules so traffice to and from VPC isn't restricted

# VPC Peering Connection Restrictions
 * use private IP addresses
 * can be established between diff AWS accounts
 * Cannot have overlapping CIDR blocks
 * can have only one peering resources between any two VPCs
 * do not support transitive peering (i.e. Development VPC cannot connect to Production VPC, has to be connected via Test VCP)


# CONSIDERATIONS FOR PEERING MULTIPLE VPCs
 * when connecting multiple VPCs, consider these network design principles
     1.  Only connect essential VPCs
     2.  Make sure yoru solution can scale

# KEY TAKEAWAYS
  1.  VPC Peering is a one to one networking connection between 2 VPCs that enables you to route traffice between them privately
  2.  You can establish peering relationships between VPCs across different AWS Regions
  3.  VPC Peering Connections
      * use private IP addresses
      * can be established between different AWS accounts
      * Cannot have overlapping CIDR blocks
      * can have only one peering resource between any two VPCs
      * do not supprot transitive peering relationships
