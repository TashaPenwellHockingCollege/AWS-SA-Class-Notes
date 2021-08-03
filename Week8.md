
# SCALING YORU COMPUTE RESOURCES

## What is Elasticity?
  * Elastic infra can expand and contract as capacity needs change
  * Turn on resources as need them, turn off when don't
  * Examples
      ** increasing the number of web servers when traffic spikes
      ** lowering write capacity on your db when traffic goes down
      ** handling the day-to-day fluctuation of demand throughput your architecutre

## What is Scaling?
  * ability to increase or decrase compute capacity of your app
  * technique that is used to achieve elasticity
  * Two types of scaling
      1.  Horizontal Scaling
            ** add (scaling out) or remove (scaling in) resources
      2.  Vertical Scaling
            ** increase or decerase the specs of an individual resource (i.e. change the CPU, RAM, etc of EC2 instance)
            ** can reach a limit
            ** not always a cost efficient or highly available approach
            ** easy to implement and can be sufficient for many uses caes (like short term)

## Amazon EC2 Auto Scaling
  * launches/terminates instances based on specified conditions
  * auto registers new instances w/ load balancers when specified
  * can launch across AZ

# Scaling Options
  1.  Scheduled
      * good for predictable workloads
      * scale based on date and time
      * use case: turning off your dev and test instances at night
  2.  Dynamic
      * good for changing conditions
      * supports target tracking
      * use case:  scaling baed on CPU utilization
  3.  Predictive
      * good for predicted demand
      * scaled based on ML
      * use case:  handling an increase in workload for ecommerce website during major sales event
  4.  Manually
      * add/remove EC2 instances manually
      * specify only the change in the maximum, minimum or desired capacity of auto scaling group

# Dynamic Scaling Policy Types
  1.  Simple Scaling:  single scaling adjustment
        * examples use cases:  new workloads, spiky workloads
  2.  Step Scaling:  adjustment depends on size of alarm breach
        * example use cases:  predictable workloads
  3.  Target Tracking Scaling:  Target value for specific metric
        * exmaple use case:  horizontally scalble apps, such as load-balanced apps and batch data-processing apps

# Auto Scaling Groups
  * Auto Scaling Group defines (you can specify)
    1.  Minimum instances capacity
    2.  Maximum instances capacity
    3.  Desired instances capacity
          ** trigger based setting and can flucturate in response to events like a threshold being breached
          ** reflects the number of instances running at that point
          ** can never be lowers than the min or higher than the max values
          
# EC2 Auto Scaling Purchseing Options
  * can specify what instance type it uses
  * specify what percentage of desired capacity should be fulfilled should be filled w/ on-demand, reserved and spot instances
  * EC2 Auto Scaling provisions lowest price combo of instances to meet desired capacity baed on preferences
  * can use just 1 instance type but it's best practice to use a ew instance types to avoid launching instances from instance pools that don't have sufficient capacity
  1.  On-Demand Instances
  2.  Reserved Instances
  3.  Spot Instances

# Automatic Scaling Considerations
  * multiple types of auto scaling
  * simple, step or target tracking scaling
     ** Simple Scaling Policies:  Increase or decrease the current capacity of the group based on a single scaling adjustment
      ** Step Scaling Policies:  Increase or decrease the current capacity of the group based on step adjustments that vary based on the size of the alarm breach
     ** Target tracking scaling policies increase or decrease the current capacity of the group based on a target value for specific metric
  * multiple metrics (not just CPU)
      ** recommend use a target tracking scaling policy to scale on a metric like avg CPU utilization or the RequestCountPerTarget metric from the Application Load Balancer
  * when to scale out and scale in
      ** use metrics  that decrese when capacity incrases and increase when capacity decreases 
      ** try to scale out early and fast and scale in slowly over time
  * use of lifecycle hooks
      ** enable you to perform custom actions by pausing instnaces when an Auto Scaling Group launches or terminates them
      ** when instance is paused it remains in a wait state until you complete the lifecycle action with the complete-lifecycle-action command the CompleteLifecycleAction operation or until the timout period ends which is 1 hour by default
      
# Key Takeawyas
  1.  an elsatic infra can expand and contract as capcity needs change
  2.  EC2 Auto Scaling auto adds/removes EC2 instances according to policies that you define, schedules, and health checks
  3.  EC2 Auto Scaling provides several scaling options to best meet needs of your apps
  4.  When you config Auto Scaling group, you can specify the EC2 instnace types and he combo of pricing models that it uses


# Vertical Scaling with RDS:  Push-Button Scaling
 * Managed service that takes care of scaling relational db and keeps up w/ increasing demands of apps
 * Scale DB instances verticually up or down by changing the RDS instance class
   ** when being scaled up or down, it's temp unavailable while DB instances class is being modified (usually only lasts a few minutes)
       *** occurs during maintenance window for DB instance unless specify that modification should take place immediately
 * can separately modify your DB instances to increase the allocated storage or improve performance by upgrading storage type
 * can also use RDS Storage Autoscaling to auto scale storage capacity in respnse to growing DB workloads
 * from micro to 24Xlarge and everthing in between
 * Scale vertically w/ minimal downtime

# Horizontal Scaling w/ RDS:  Read Replicas
 * horizontal scale for read-heavy workloads
 * up to 5 read replicas and up to 15 Aurora replicas
 * Replication is asynchronous
 * Available for RDS, MySQL, MariaDB, PostgreSQL and Oracle
   ** have a built in replciation functionality
 * uses functionality to create a specity type of DB instnace called a read replica from a source DB instance 
 * updates made to the source DB instance are asynchronously copied to the read replica
 * can reduce load on source DB instnace by routing read queeries from your apps to the read replica
 * to improve performance of read-heavy db workloads can use read replicas to horizontally scale your source DB instance
 * if there's a disaster, can increse the avail of db by promoting read replica to become a standalone DB instance

# Scaling w/ Amazon Aurora
 * Aurora is a RDB engine built for for the cloud
 * Aurora is compatable w/ MySQL, and 
 * Each Aurora DB cluster can have up to 15 Aurora replicas
 * Amaozn Aurora further extends benefits of read replicas by employing a SSD backed virtualized storage layer
   ** the layer is purpose built for DB workloads
 * compatible w/ MySQL and PostgrSQL
 * RDS manages your Aurora db and handles tasks such as provisioning, patching, backup, recovery, failure detection and repair
 * features a distributed, fault-tolerant, self-healing storage system that auto scales up to 64 TB per DB instance
 * delivers high-performance and availability, point-in-time recovery, continuous backup to S3 and replication across 3 AZ 
 * Aurora DB cluster contains one or more DB instances and cluster volume that manages the data for those DB instances in an Aurora DB cluster
   1.  Primary DB instance
          ** supports read and write operations
          ** performas all data modification to the cluster volume
          ** each Aurora DB cluster has 1 primary DB instance
   2.  Aurora Replica
          ** connects to same storage volume as the primary DB instance
          ** supports read only ops
          ** each Aurora DB cluster can have up to 15 Aurora replicas in addition to primary DB instnace
          ** can choose DB class size and add Aurora replicas to increase read throughput
          ** can mofidy the DB instance class size and change number of Aurora replicas if workload changes
          ** models works well when DB workload is predictable because can change capacity manually based on expected workload
          
# Amazon Aurora Serverless
 * Responds to your application automatically
    ** scales capacity 
    ** starts up
    ** shuts down
 * Pay for the number of Aurora capacity units (ACUs) that are used
    ** each ACU is a combo of processing and memory capacity
    ** DB storage auto scales from 10 Gibibytes to 64 Tebibytes and it's the same as storage in a standard Aurora DB cluster
    ** DB endpoint connects to a proxy fleet that routes the workload to a fleet of resources
    ** Aurora Serverless scales the resources auto based on the minimum and maximum specification for capacity
    ** pay for number of ACUs used and are calculated on a per-second basis for the db capacity you use when the db is active and when you migrate between standard and serverless configurations
 * Good for intermitten and unpredictable workloads

# Horizontal Scaling:  Database Sharding
 * without shards, all data resides in one partition
    ** example:  Employee IDs in one db
 * With sharding, data is split into large chunks (shards) 
    ** Example:  even-number employee IDs in one db and the odd-number employee IDs in another db
 * Sharding can improve write performance
 * DB shards usually have the same type of HW, db Engine and data structure so they generate a similar level of performance
 * DB shards DO NOT have knowledge of each other (this is the big difference between sharding and other scale-out approaches such as db clusting or replication)
 * sharded db architecture offers scalability and fault tolerance
 * data can be split across as many db servers as necesssary
 * if 1 db shard has a HW issues or goes through failover, no other shards are impacted
    ** single point of failure or slowdown is physiclaly isolated
 * drawback is that data is spread across shards so data mapping and routing logic must be specifically engineered to read or join data from multiple shards compared to a nonsharded db


# Scaling with Amazon DynamoDB:  On-Demand
 * On-Demand / pay per request pricing model inspead of a provisioned pricing model
 * no more provisioning
 * can observe any increase or scale of the level of traffic
 * Use Case:  Spiky, unpredictable workloads.  Rapidly accomodates to need
 * Amazon DynamoDB On-Demand is a flexibile billing option
 * can serve thousands of requests per second w/o capacity planning
 * can change a table from provisioned capacity to on-demand one time per day but can go from on-demand capacity to provisioned as often as you want


# Scaling w/ Amazon DynamoDB:  Auto Scaling
 * Auto scaling is default for all new tables
 * auto adjusts read and write throughput capacity in response to dynamically chaging request volumes w/o any downtime
 * specify upper and lower bounds for desired throughput utilization
 * Use Case:  general scaling, good solution for most applications
 * works w/ Amazon CloudWatch to monitor actual throughput consumption
 * no additional cost to use DynamoDB auto scaling, jsut pay for DynamoDB and CloudWatch alarms

# How to Implement DynamoDB Auto Scaling
 1.  User creates scaling policy
          ** create a scaling policy in DynamoDB table or the Global Secondary Index (GSI)
          ** specifies if wnat to scale read capacity or write capacity or both
          ** specifies min and max provisioned capacity unit settings for the table or index
 2.  Consumed capacity metrics in DynamoDB are published to Amazon CloudWatch
         ** published consumed capacity metrics to Amazon CloudWatch  
         ** if table's consumed capacity exceeds  target utilization or falls below for a specific length  of time, CloudWatch triggers an alarm
 3.  CloudWatch uses SNS to send email to user
 4.  CloudWatch alarm invokes Application Auto Scaling
 5.  Application Auto Scaling issues "UpdateTable" request
          * service dynamically adjust provisioned throughput capacity on your behalf in response to actual traffic patterns
          * enables a table or a Global Secondary Indext (GSI)
          * increase the provisioned read and write capacity of DynamoDB to handle sudden increases in traffic w/o throttling 
 6.  DynamoDB processes request


# Scaling throughput capacity:  DynamoDB adaptive capacity
 * when data access is imbalanced, one partition can receive a higher volume of read and write traffic compared to other partitions (this is called a hot partition)
 * can occure if a single partition received more than 3,000 read capacity units and 1,000 write capacity units (in extreme cases)
 * enables reading and writing to hot partitions w/o throttling
 * auto increases throughput capacity for partitions that receive more traffic
         ** traffic cannot exceed the table's total provisioned capacity or the partition's max capacity
 * enabled auto for every DynamoDB table
 * Read Capacity Units (RCUs) and Write Capacity Units (WCUs)
 * DynamoDB Adaptive Capacity lets your app continue reading and writing to hot partitions w/o being throttled
        ** traffic cannot exceed table's total provision capacity or partitoin maximum capacity
 * Adaptive capacity works by auto increasing throughput capacity for partitions that receive more traffic
 * Adaptive capacity is enabled auto for every DynamoDB table so no need explicity enable/disable it


# Key Takeaways
* you can push-button scaling to vertically scale compute capacity for RDS DB instance
* can use read replicas or shards to horizontally scale your RDS DB instance
* with Amazon Aurora can choose the DB instance class size and number of Aurora replicas (up to 15)
* Aurora Serverless scales resources automatically based on the minimum capacity specs
* Amazon DynamoDB On-Demand offers a pay per request pricing model
* DynamoDB auto scaling uses Amazon Application Auto Scaling to dynamically adjust provisioned throughput capacity
* DynamoDB adaptive capacity works by auto increasing throughput capacity for partitions that receive more traffic


# Highly Available Systems
* can withstand some measure of degradation while remaining available
* have minimized downtime
* require minimal human intervention
* recover from failure or roll over to secondary source in an accepatble amount of degraded performance

# Elastic Load Balancing
* Managed load balancing service that distributes incoming app traffic across multiple EC2 instances, containers, IP address, and Lambda functions
* can be external-facing or internal-facing
* each load blanacers receives a default DNS name
* recognizes adn responds to unhealthy instances

# Types of Load Balancers
* Elastic Load Balancers come in three types
    1.  Application Load Balancers
    2.  Network Load Blanacer
    3.  Classic Load Balancer
* Application Load Balancers
  ** operates at the application layer (layer 7) in the Open Systems Interconenction model (OSI)
  ** works well for advanced load balancing of HTTP or HTTPS traffic
  ** routes traffic to targets like EC2 isntances, containers, IP addresses and Lambda functions based on the content of the request
  ** flexible application management
  ** advanced Load balancing of HTTP and HTTPS traffic
  ** operates at the rquest level (Layer 7)
* Network Load Balancer
  ** operates at Network TRansport Layers of OSI model Layer 4
  ** routes connections to targets such as EC2 instances, microservices and containers
  ** works well for load blaancing both TCP and UDP traffic
  ** TCP, UDP, TLS
  ** ultra-high performance and static IP addr4ess for your application
  ** load blaancting of TCP, UDP and TLS traffic
  ** operates at the connection level (Layer 4)
* Classic Load Balancer
  ** provides basic load balancing across multiple EC2 instances and operates in both application and network transport layer
  ** previous generation for HTTP, HTTPS, TCP adn SSL
  ** load balancing across multiple EC2 instances
  ** operates at both the rqeuest level and connection level
  ** recommend use the dedicated Application Load Balancer or Network Load Balancer 
  
# Implementing High Availability
  ** start w/ 2 AZ per AWS Region
  ** if resources in one AZ are unreachable application shouldn't fail
  ** uses a load balancer to distribute traffic among those resources
  ** if use data sources that only support primary or secondary failover then app may not benefit from using mor AZs than that
  ** because AZ are spread out physically, won't receive much benefit from duplicating resources in 3+ AZ in one AWS Region
  ** for heavy EC2 Spot Instances usage or for data sources that go beyond active/passive (such as Amaozn DynamoDB) there may be a benefit to using >2 AZ
  
# Amazon Route 53
  ** Amazon Route 53 is a highly available and scalable cloud DNS service
  ** translates domain names into IP addresses
  ** connects user requests to infra that runs inside and outside of AWS
  ** can be configured to route traffic to healthy endpoints or to monitor the heald of your application its endpoints
  ** offers registration for domain names
  ** has multiple routing options
  
# Amazon Route 53 Supported Routing
  1. Simple routing (aka "round robin")
      ** distributes number of requests as evently as possible between all participating servrs
  2.  Weighted Round Robin Routing
      ** assign weights to resource record sets to specify the frequency that different responses are served at
      ** can use this type of routing to do A/B testing (testing when send small portion of traffic to server where made SW change)
  3.  Latency Based Routing
      ** when you have resources in multiple AWS Regions and want to route traffic to the Region that provides the best latency and fastest experience for your users
  4.  Geolocation Routing
      ** choose the resources that serve your traffic based on geo location of users (origin of DNS queries)
      ** can localize content and present some or all of website in language of users
  5.  Geoproximity Routing
      ** used w/ Route 53 Traffic Flow
      ** lets you route traffic based on physical distanced between users adn resources 
      ** can route more or less traffic to each resource by specifiying a positive or negative bias
      ** when creating a traffic flow policy, can specify either an AWS Region, if using AWS resources or the latitude and longitude for each endpoint
  6.  Failover Routing (or DNS failover)
      ** use when want to configure active-passive failover
      ** Route 53 can help detect if website has an outage and redirect users to alternate working locations
      ** when enabled, Route 53 health checking agents monitor location/endpoint of app to determine availability
      ** can increase availability of customer-facing app
  7.  Multvalue Answer Routing
      ** route traffic to multiple resources like web servers in an approximately random way, can create 1 multivalue answr record for each resource
      ** optionally associate a Route 53 Health Check w/ each record
      ** i.e. you manage an HTTP web service w/ 12 web servers and each server has its own IP address.  No one web server can handle all the traffic, if you create a dozen multivalue answer records, Route 53 responds to DNS queries w/ up to 8 healthy records in response to each DNS query.  Also gives different answers to different DNS resolvers.  if a web server becomes unavailable after a resolver caches a response
      
# Key Takeaways
  *  Can design your network arch to be highly available and avoid single points of failure
  *  Route 53 offers various routing options that can be combined w/ DNS failover to enable low-latency, fault-tolerant architecture


# Monitoring usage, operations and performance
  * monitoring helps keep track of how resources are operating and performing, along w/ resource utilization and app performance to make sure infra is meeting demand
  * monitoring can help decided what permissions to set for AWS resources to achieve security goals

# Monitoring Costs
  * create a more flexible and elastic architecture, should know where you are spending money
  * AWS monitoring and reporting tools
     1.  AWS Cost Explorer
     2.  AWS Budgets
     3.  AWS Cost & Usage Report
     4.  Cost Optimziation Monitor
  * AWS Cost Explorer
     ** manage AWS costs and usage w/ daily or monthly details
     ** can view data up to the last 13 months which helps you see patterns in how you spend on AWS resources over time
  * AWS Budgets
     ** set custom budgets that alert you when costs or usage exceed or are forecasted to exceed your budgeted amount
  * AWS Cost and Usage Report
     ** contains the most comprehensive set of AWS cost and usage data available
     ** includes metadata about AWS services, pricing and reservation
  * Cost Optimization Monitor
     ** solution arch that auto processes detailed billing reports
     ** provides granular metrics that can search, analyze and visualize in a dashboard that you can customize 
     ** provides insight into service usage and cost
     ** can break down this data by period, account, resource or tags

# Amazon CloudWatch
    * collects and tracks metrics for resources and apps
    * helps correlate, visualize and analyze metrics and logs
    * enables you to create alamrs and detect anomalous behavior
    * can send notifications or make changes to resources that you are monitoring
    * CloudWatch components can use
      1.  Metrics
      2.  Logs
      3.  Alarms
      4.  Events
      5.  Rules
      6.  Targets
 * Metrics
    ** data about the performance of your system
    ** can be kept for 15 months 
    ** can view both up-to-the minute data and historical data
 * Amazon CloudWatch Logs
    ** can use CloudWatch Logs to monitor, store and access log files from difference sources 
* Amazon CloudWatch Alarms
    ** can use an alarm to auto respond to monitored conditions
    ** single metric over a certain time period
    ** performs 1+ specified actions based on value of metric relative to threshold over time
    ** action is a notification sent to an Auto Scaling policy to a topic in SNS
    ** alarms invoke actions for sustained state changes only; don't inovke actions jsut becasue they are in a particular state; the state must have changed and been maintained for specified number of periods; this avoids triggering alarms on transient spikes
    
# Amazon EventBridge Events 
  * ingests a stream of real-time data from your own apps, AWS services or SaaS
  * routes data to targets such as Lambda
  * event = change in an environment
  * this can be an AWS environment, SaaS APN partner service or one of own custom apps or services
  * can set up scheduled events that are generated on periodic basis

# Amazon EventBridge Rules
 * matches incoming events and routes them to targets for processing
 * a single rule can route to multiple targets which are all processed in parallel
 * rules aren't processed in particualr order
 * sends info in JSON
 * can customize info in the JSON that sent to target by passing only certain parts or by overriding it w/ a constant
 
# Amazon EventBridge Targets
 * target processes events
 * targets can included EC2 instances, Lambda functions, Kinesis streams, ECS tasks, AWS Step functions state machines, SNS  topics, SQS queues, and built-in targets
 * targets receive events in JSON format; when you create a rule, you associate it w/ a specific event bus
 * the rule is matched only to events received by that event bus


# How CloudWatch and EventBridge Work
 1. CloudWatch acts a metrics repo
 2. an AWS service (i.e. EC2) puts metrics into the repo
 3. you retrieve stats based on those metrics; if put own custom metrics in repo can also retrieve stats on these metrics
     * can use metrics to calculate stats and preset data graphically in the CloudWatch console
 4.  can also create a rule  in EventBridge that matches incoming events and routes them to targets for processing; can configure alarm actions to stop, start or terminate EC2 instance when certain criterai are met
      
  

