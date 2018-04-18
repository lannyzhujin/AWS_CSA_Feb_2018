# Kinesis
![](https://github.com/lannyzhujin/AWS_CSA_Feb_2018/blob/master/AWS_CSA-Associate/img/Kinesis.PNG)
## Kinesis Essentials:  
 - Kinesis is a real-time data processing service that continuously captures (and stores) large amounts of data that can power real-time streaming dash boards.
 - Using the AWS provided SDKs, you can create real-time dashboards, integrate dynamic pricing strategies, and export data from Kinesis to other AWS services.
 - Including:
     - EMR (analytics)
     - S3 (storage)
     - RedShift (big data)
     - Lambda (event driven actions) 

### Kinesis Components 
 - Stream
 - Producers (data creators)
 - Consumers (data consumers)
 - Shards (processing power) 
 
### Kinesis Benefits:  
 - Real-time processing:
     - Continuously collect and build applications that analyze the data as it's generated. 
 - Parallel Processing:
     - Multiple Kinesis applications can be processing the same incoming data streaming concurrently. 
 - Durable:
     - Kinesis synchronously replicates the streaming data across three data centers within a single AWS region and preserves the data for up to 24 hours. 
 - Scales:
     - Can stream from as little as a few megabytes to several terabytes per hour. 

### When to use Kinesis:  
 - Gaming:
     - Collect gaming data such as player actions and feed the data into the gaming platform, for example a reactive environment based off of real-time actions of the player. 
 - Real-time analytics:
     - Collect IOT (sensors) from many sources and high amounts of frequency and process it using Kinesis to gain insights as data arrives in your environment. 
 - Application alerts:
     - Build a Kinesis application that monitors incoming application logs in real-time and trigger events based off the data. 
 - Log / Event Data collection:
     - Log data from any number of devices and use Kinesis application to continuously process the incoming data, power real-time dashboards and store the data in S3 when completed. 
 - Mobile data capture:
     - Mobile applications can push data to Kinesis from countless number of devices which makes the data available as soon as it is produced. 

### Kinesis Producers:  
 - Producers are devices that collect data for Kinesis processing.
 - You build producers to continuously input data into a Kinesis stream.
 - Producers can include (but not limited to):
     - loT Sensors
	 - Mobile devices (cell phones)
 - You can have literally thousands of different producers and scale based on need.
     - The more data you want to process, the more "shards" you add to your Kinesis stream.
	 - Each "shard" can process 2MB of read data per second, and 1MB of write data per second. 

### Kinesis Consumers:  
 - Consumers consume the stream's data.
 - This is done concurrently (multiple consumers can consume the same data at the same time).
 - Consumers include (but are not limited to):
     - Real-time dashboards
	 - S3
	 - Redshift (data warehouse)
	 - EMR 

 - Any application (one you create) can consume the streams data. 
	 
 - Kinesis keeps 24 hours of streaming data stored by default, but can be configured to store up 
to 7 days. 

# Elastic MapReduce
![](https://github.com/lannyzhujin/AWS_CSA_Feb_2018/blob/master/AWS_CSA-Associate/img/EMR.PNG)
## Elastic MapReduce Essentials:  
 - Amazon EMR is a service which deploys out EC2 instances based off of the Hadoop big data framework.
 - EMR is used to analyze and process vast amounts of data.
 - EMR also supports other distributed frameworks, such as:
      - Apache Spark
	  - HBase
	  - Presto
	  - Rink 
### General EMR Workflow
 - Data stored in S3, DynamoDB, or Redshift is sent to EMR
 - The data is **mapped** to a "cluster" of ***Hadoop*** **Master/Slave nodes** for processing.
 - Computations (coded/created by the developer) are used to process the data.
 - The processed data is then **reduced** to a single output set of return information. 

### Other Important EMR Facts
 - You (the admin) has the ability to access the underlying operating system.
 - You can add user data to EC2 instances launched into the cluster via bootstrapping.
 - EMR takes advantage of parallel processing for faster processing of data.
 - You can resize a running cluster at any time, and you can deploy multiple clusters. 

### EMR Master node:  
 - A node that manages the cluster by running software components which coordinate the distribution of data and tasks among other (slave) nodes for processing.
 - The master node tracks the status of tasks and monitors the health of the cluster. 

### EMR Slave Nodes:  
 - There are two types of slave nodes: 
    - Core node:
    	- A slave node has software components which run tasks AND stores data in the Hadoop Distributed File System (HDFS) on your cluster.
		- The core nodes do the "heavy lifting" with the data. 
		
    - Task node:
    	- A slave node that has software components which only run tasks.
		- Task nodes are optional. 

### EMR Map Phase:  
 - Mapping is a function that defines the processes which splits the large data file for processing.
 - During the mapping phase, the data is split into 128MB "chunks".
 - The larger the instance size used in our EMR cluster, the more chucks you can map and process at the same time.
 - If there are more chucks than nodes/mappers, the chunks will queue for processing. 

### EMR Reduce Phase:  
 - Reducing is a function that aggregates the split data back into one data source.
 - Reduced data needs to be stored (in a service like S3) as data processed by the EMR cluster is not persistent. 

[[Home]](https://github.com/lannyzhujin/AWS_CSA_Feb_2018/blob/master/AWS_CSA-Associate/Home.md)