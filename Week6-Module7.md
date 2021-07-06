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
