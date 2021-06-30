
# Database Layer Considerations
    Things to consider
      1.  Scalability
          *Database Throughput is a measure of performance and indicates how much processing a db engine can perform in a given amount of time.
          - Measured in terms of transactions per second, queries per second or megabytes per second
          - Choose a db solution that can handle the needed throughput at launch and can easily scale up later
          - Want to be able to scale up or down based on demands of service to be cost efficient and maintain standard w/ AWS Well-Architected Framework
      2.  Total storage requirements
          * How large does the db need to be?  Will it be storing GB, TB or PB of data?
          * Diff db arch support diff max data capacities
      3.  Object size and type
      4.  Durability
          * What level of data durability, data availablility and recoverability is required?
              Durability = assurance that data won't be lost
              Availability = assures access to data when want to
          * Are there regulatory obligations?
          
    Database Types
      * Two categories of db options
        1.  Relational
              - Traditional example include MS SQL Server, Oracle DB, MySQL
        2.  Non-Relational
              - Traditional examples include MongoDB, Cassandra, Redis
              
    Relational Database
        Benefits
          1.  Ease of use
          2.  Data integrity
          3.  Reduced data storage
          4.  Common language (SQL)
          
        Ideal for
          1.  Need for strict schema rules, ACID compliance and data quality enforcement
          2.  Do not need extreme read/write capacity
          3.  Do not need extreme performance 
          
    Non-Relational Databases
      Benefits
        1.  Flexibility
        2.  Scalability
        3.  High performance
        4.  Highly functional APIs
        
      Ideal for
        1.  Need db to scale horizontally to handle massive data volume
        2.  Data does not lend itself well to traditional schemas
        3.  Read/write rates exceed what can be economically supported via traditional RDBMS
        
        - use various data models includeing key-value, graph, document, in-memory, and search
        - are dynamic (i.e. a row doesn't need to contain data for each column)
        - can scale horiztonally by increasing number of servers it runs on
        - each object can have  a different structure 
            Examples include 
                1.  Structured data (i.e. relational db records)
                2.  Semi-structured data (i.e. JSON)
                3.  Unstructred data (photo files or email messages)


# Amazon RDS
    * RDS
        -fully managed relational DB service
        -Characteristics
            1. used for interactions w/ the db is transactional in nature or light data analytics
            2. ideal data size range is up to the low-TB range; can provision additional storage as needed (i.e. Amazon Aurora)
            3.  provides mid ot high throughput w/ low latency
            4.  handling business transactions and online analytics processing (OLAP)
        -Uses and DB Types
            Works well for applications that:
                1.  Have more complex data
                2.  Need to combine and join datasets
                3.  Need enforced syntax rules
        -Six DB types supported
                1.  MS SQL Server
                2.  PostgreSQL
                3.  Oracle
                4.  Aurora
                5.  MySQL 
                6.  MariaDB
        -DB Instance Sizing
            - all RDS DB engines run on a server except for Aurora (which as  serverless option)
            - T Family
                    -burstable instances
                    -moderate networking performance
                    -smaller or variable workloads
            - M Family
                    -general purpose instances
                    -high performance networking
                    -CPU intensive workloads
            - R Family
                    -memory optimized instances
                    -high performance networking
                    -query intensive, high connectiong counts workloads
            - R Family
        - High Availability by using Multi-AZ deployments
            -Benefits
                1.  Enhanced durability
                2.  Increased availability
                3.  Fail over to standby occurs automatically
            -Automated Failover Conditions
                1.  Loss of availability in primary AZ
                2.  Loss of network connectivity to primary
                3.  Compute unit failure on primary
                4.  Storage failure on primary


# Amazon DynamoDB
        - fully managed non-relational key-value and document database service
        - Benefits
              1.  Performance at any scale
              2.  Serverless
              3.  Enterprise-ready
        - Works well for applications that
              1.  Have simple high-volume data (high-TB range)
              2.  Must scale quickly
              3.  Don't need complex joins
              4.  Require ultra-high throughput and low latency
        - Key Features
              1.  NoSQL tables
              2.  Items can have differing attributes
              3.  In-memory caching
              4.  Support for peaks of more than 20 millions requests per second
        - By default Amazon DynamoDB replicates your data across multiple AZ in a single region.
        - DynamoDB global tables provides a fully managed solution for deploying a multi-Region, multi-master db
            - Don't need to build and maintain own replications solutions.  When create a global table, specify the AWS Regions where want the table to be available.  DynamoDB performs all necessary tasks to create identical tables in these regions
            - DynamoDB propagtes ongoing data changes to all the tables
        - DynamoDB Consistency Options
            1.  Eventually Consistent
                * default setting
                * all copies of data usually reach consistency w/in 1 second
            2.  Strongly Consistent
                * optional feature
                * used for apps that require all reads to return a result that reflects all writes before the read
                * Disadvantages:  
                    a. may not be available if there is an outage or network delay because must wait for all write ops to complete which can take longer than an eventually consistent read
                    b. consume more resources
            -can specify the read consistency you want for each read request
        
# Section 4 Takeaways
    1.  DynamoDB is a fully managed non-relational key-value and document NoSQL db service
    2.  DynamoDB is serverless, provides extreme horizontal scaling and low latency
    3.  DynamoDB global tables ensures that data is replicated to multiple Regions
    4.  DynamoDB provides eventual consistency by default (usually fully consistent for reads 1 second after the write)
    5.  Strong consistency is optional for DynamoDB but can have some drawbacks.
                   
            
        
                
        
      
  
