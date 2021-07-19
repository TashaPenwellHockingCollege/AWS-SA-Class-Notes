
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














