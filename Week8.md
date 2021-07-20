
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


Week 8 - Implementing Elasticity, High Availability, and MonitoringPart 1: Scaling your Databases







