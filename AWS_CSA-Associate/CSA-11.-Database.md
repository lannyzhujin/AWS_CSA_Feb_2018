# RDS
![](https://github.com/lannyzhujin/AWS_CSA_Feb_2018/blob/master/AWS_CSA-Associate/img/DB.PNG)

## RDS Essentials:  
 - RDS is a fully managed Relational Database Service:
     - Does not allow access to the underlying operating system (fully-managed).
     - You connect to the RDS database server in the same way you would connect to a traditional on-premise database instance (i.e MySQL command line).
     - RDS has the ability to provision/resize hardware on demand for scaling. 
     - You can enable Multi-AZ deployments for backup and high available solutions.
     - Utilize Read Replicas (MySQL/PostgreSQL/Aurora) - to help offload hits on your primary database.
     - Relational databases are databases that organize stored data into tables.
     - The associated tables have defined relationships between them. 
 - Databases Supported By RDS:
     - MySQL
     - MariaDB
     - PostgreSQL
     - Oracle
     - MS SQL Server
     - Aurora:
         - Is a home grown Relational Database that has been forked from, and fully compatible with MySQL.
         - It has fives times better performance then MySQL. and a lower price point than commercial databases. 
 - Benefits of running RDS instead of a database on your own instance:
     - Automatic minor updates
     - Automatic backups (point-in-time snapshots)
     - Not required to managed operating system
     - Multi-AZ with a single click
     - Automatic recovery in event of a failover 
 - Storage Types
     - General Purpose (SSD) - Max 16TB, Baseline is 3 IOPS per GiB.  below 1 TiB volumes reserve a burst balance of 3000 IOPS.
     - Provisioned IOPS - Max 16TB, You specify the amount of storage you want allocated, and then specify the amount of dedicated IOPS you want.
     - Magnetic 

## RDS Multi-AZ Failover: 
 - Multi-AZ failover (Automatic AZ-Failover) synchronously replicates data to a backup (stand-by) database instance located in another availability zone (but in the same region). 
 - In the event of:
     - Service outage in an availability zone
     - Primary DB instance failure
     - Instance server type is changed
     - Manual failover initiated
     - Updating software version
     - **AWS will automatically switch the CNAME DNS record from the primary instance to the stand-by instance.**
	 
 - RDS backups are taken against the stand-by instance to reduce I/O freezes and slow down IF multi-az is enabled. 
 
 - In order for multi-az to work, your primary database instance must be launched into a "**subnet group**".
     - NOTE: An RDS instance must be launched into a subnet (inside a VPC), just like an EC2 instance. So the same security/connectivity rules, and highly available/fault tolerant concepts apply. 

## RDS Backups:  
 - AWS provides automated point-in-time backups against the RDS database instance.
 - Automated backups are deleted once the database instance is deleted and cannot be recovered (but you can take your own snapshots of backups before deleting).
 - Backups on database engines only work correctly when the database engine is "transactional", but do currently work for all supported database types.
 - MySQL requires InnoDB for reliable backups. 
 
## RDS Read Replicas:  
 - Read replicas are asynchronous copies of the primary database that are used for read only purposes (only allow "read connections").
 - When you write new data to the primary database, AWS copies it for you to the read replica.
 - You can create, and have multiple read replicas for a primary database.
 - Read replicas can be created from other read replicas (so no performance hit on the primary database)
 - MySQL, MariaDB, PostgreSQL, and Aurora currently support read replicas.
 - You can monitor replication lag using CloudWatch. 
 
### Benefits of using Read Replicas
 - Read replicas allow for all read traffic to be redirected from the primary database to the read replica. This will greatly improve performance on the primary database.
 - Read replicas allow for elasticity in RDS - you can add more read replicas as demand increases.
 - You can promote a read replica to a primary instance
 - MySQL:
     - Replicate for importing/exporting data to RDS
	 - Can replicate across regions 
	 
### When should you use Read Replicas?
 - High volume, non-cached database read traffic (elasticity).
 - Running business function such as data warehousing.
 - Importing/Exporting data into RDS.
 - Rebuilding indexes:
     - Ability to promote a read replica to a primary instance 



# DynamoDB
## DynamoDB Essentials:  
 - DynamoDB is a fully-managed, NoSQL database service provided by AWS.
 - It is similar to **MongoDB**, but is a home-grown AWS solution.
 - Is schemaless, and uses a key-value store.
 - You specify the required throughput capacity, and DynamoDB does the rest (being fully-managed). 

 - Being fully-managed means:
     - Service manages all provisioning (and scaling) of underlying hardware.
     - Fully distributed, and scales automatically with demand and growth.
     - Built as a fault tolerant highly available service.
         - On the back end, it fully synchronizes the data across all of the availability zones within the region you created the DynamoDB tables in. 
 - DynamoDB also easily integrates with other AWS services, such as Elastic MapReduce.
     - Can easily move data to a hadoop cluster in Elastic MapReduce. 

 - Popular use cases include:
     - IOT (storing meta data)
     - Gaming (storing session information, leaderboards) 
     - Mobile (Storing user profiles, personalization) 

# ElastiCache
## ElastiCache Essentials:  
 - ElasticCache is a fully managed, **in-memory cache engine**.
 - ElastiCache is used to improve database performance by caching results of queries that are made to a database.
 - ElasticCache is great for large, high-performance or high-taxing queries - and can store them inside of a cache (Elastic Cache Cluster) that can be accessed later (instead of repeat request continually hitting the primary database).
 - So it reduces load on the database, which increases performance.
 - ElastiCache allows for managing web sessions, and also caching dynamic generated data. 

 - Available engines to power ElastiCache include:
     - **Memcached** (Mem-Cache-D)
     - **Redis**
 - Generally, the application needs to be built to work with either Redis or Memcached.
 - Popular options like MySQL have Memcached plugins, which allow an application to easily work with ElastiCache (if using Memcached as the engine). 

# Redshift
## Redshift Essentials:  
 - Amazon Redshift is a **petabyte-scale data warehousing service**.
 - It is fully-managed and scalable.
 - Generally used for big-data analytics, and it can integrate with most popular business intelligence tools, including:
     - Jaspersoft
     - Microstrategy
     - Pentaho
     - Tableau
     - Business Objects
     - Cognos 
