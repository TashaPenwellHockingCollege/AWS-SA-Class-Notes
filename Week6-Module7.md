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
