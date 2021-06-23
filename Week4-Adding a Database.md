
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
                
        
      
  
