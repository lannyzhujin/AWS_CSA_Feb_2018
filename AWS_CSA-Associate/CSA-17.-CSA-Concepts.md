# Implementation and Deployment
## How to Design Cloud Services & Best Practices: 
 - Design for failure, and create self-healing application environments.
 - Always design applications with instances in at least two availability zones.
 - Guarantee that you have "reserved" capacity in the event of an emergency by purchasing reserved instances in a designated recovery availability zone (AWS does not guarantee on-demand instance capacity).
 - Rigorously test to find single points of failure and apply high availability.
 - Always enable RDS Multi-AZ and automated backups (InnoDB table support only for MySQL).
 - Utilize Elastic IP addresses for fail over to "stand-by" instances when auto scaling and load balancing are not available.
 - Use Route 53 to implement failover DNS techniques that include:
     - Latency based routing
	 - Failover DNS routing
 - Have a disaster recovery and backup strategy that utilizes:
     - Multiple Regions
	 - Maintain up to date AMI's (and copy AMI's from one region to another)
	 - Copy EBS snapshots to other regions (use CRON jobs that take snapshots of EBS)
	 - Automate everything in order to easily re-deploy resources in the event of a disaster
	 - Utilize bootstrapping to quickly bring up new instances with minimal configuration and allows for "generic" AMI's
 - Decouple application components using services such as SQS (when available).
 - "Throw away" old or broken instances.
 - Utilize CloudWatch to monitor infrastructure changes and health.
 - Utilize MultiPartUpload for S3 uploads (for objects over 100MB).
 - Cache static content on Amazon CloudFront using EC2 or S3 Origins.
 - Protect your data in transit by using HTTPS/SSL endpoints.
 - Protect data at rest using encrypted file systems or EBS/S3 encryption options.
 - Connect to instances inside of the VPC using a bastion host or VPN connection.
 - Use IAM roles on EC2 instances instead of using API keys; Never store API keys on an AMI 

## Monitoring your AWS Environment:  
### Use CloudWatch for
 - Shutting down inactive instances.
 - Monitoring changes in your AWS environment with CloudTrail integration.
 - Monitor instances resources and create alarms based off of usage and availability:
     - EC2 instances have "basic" monitoring which CloudWatch supports out of the box, and includes all metrics that can be monitored at the hypervisor level.
	 - Status Checks which can automate the recovery of failed status checks by stopping and starting the instance again.
     - EC2 metrics that include **custom scripts** to work with CloudWatch:
    	 - Disk Usage; Available Disk Space
		 - Swap Usage; Available Swap
		 - Memory Usage; Available Memory 
		 
### Use CloudTrail for
 - Security and compliance.
 - Monitoring all actions taken against the AWS account.
 - Monitoring (and being notified) of changes to IAM accounts (with CloudWatch/SNS Integration)
 - Viewing what API Keys/Users performed any given API action against an environment (i.e view what user terminated a set of instances or an individual instance).
 - Fulfilling auditing requirements inside of organizations. 
 
### Use AWS Config for
 - Receiving detailed configuration information about an AWS environment.
 - Taking a point in time "snapshot" of all supported AWS resources to determine the state of your environment.
 - Viewing historical configurations within your environment by viewing the "snapshots".
 - Receiving notifications whenever resources are created, modified, or deleted.
 - Viewing relationships between resources, I.E what EC2 instances an EBS volume is attached to. 

 ## Architectural Trade-off Decisions:  
 ### Storage Trade-off Options
 - S3 Standard Storage
     - 99.999999999% durability and 99.99% availability, but is the most expensive.
 - S3 RRS
     - Reduce redundancy durability is 99.99%, but the storage costs is cheaper.
	 - Should be used for easily reproducible data, and you should take advantage of lost object notification using S3 events.
 - Glacier
     - Requires and extended timeframe to check-in and check-out data from archiving.
	 - Costs are significantly reduced compared to S3 storage options. 

### Database Trade-off Options
 - Running databases on EC2 instances:
     - Have to manage the underlying operating system.
	 - Have to build for high availability.
	 - Have to apply your own backups.
	 - Can use additional software to cluster MySQL.
 	 - Requires more time to manage than RDS. 
 - Managed RDS database provides:
     - Fully managed database updates and does not require managing of the underlying OS.
	 - Provides automatic point in time backups.
	 - Easily enable Multi-AZ failover, and when a failover occurs the DNS is switched from the primary instance to the standby instance.
	 - If Multi-AZ is enabled then backups are taken against the stand-by to reduce I/O freezes and updates are applied to the standby then is switched to the primary.
	 - Easily create read replicas. 

## Elasticity and Scalability: 
 - **Proactive Cycle Scaling**: Scaling that occurs at a fixed interval.
 - **Proactive Event-based scaling**: Scaling that occurs in anticipation of an event.
 - **Auto-scaling based on demand**: Scaling that occurs based off of increase in demand for the application. 
 - Plan to scale out rather than up (Horizontal scaling):
     - Add more EC2 instances to handle increases in capacity rather than increasing instance size.
	 - Be sure to design for the proper instance size to start.
	 - Use tools like Auto Scaling and ELB.
	 - A scaled service should be fault tolerant and operationally efficient.
	 - Scalable service should become more cost effective as it grows. 

 - DynamoDB is a fully managed NoSQL services from AWS:
     - With high availability and scaling already built in.
	 - All the developer has to do is specify required throughput for the tables. 
	 
 - RDS requires scaling in a few different ways:
     - RDS does not support a cluster of instances to load balance traffic across.
	 - Because of this there are a few different methods to scale traffic with RDS:
    	 - Utilize read replicas to offload heavy read only traffic.
		 - Increase the instance size to handle increase in load.
		 - Utilize ElastiCache clusters for caching database session information. 

# Security Architecture with AWS
## Shared Security Responsibility Model: 
 - AWS is responsible for portions of the cloud, and you as the customer have portions of the cloud that you are responsible for - thus creating shared security responsibility. 
 - Reduces the operational burden (on you) as AWS operates, manages, and controls the components from the host operating system and virtualization layer, down to the physical security of the facilities in which the services operate. 
 - As the customer (you), using AWS means you assume the responsibility and management of the guest operating system (including, updates and security patches), other associated applications software, as well as the configuration of the AWS-provided security group firewall. 
 - You are also responsible for your own coded applications and custom applications built on top of the cloud. 
### **AWS is responsible for (EC2 example)**
 - Facilities
 - Physical security of hardware
 - Network infrastructure
 - Virtualization infrastructure 
### **You (as the customer) are responsible for (EC2 example)**
 - Amazon Machine Images (AMIs)
 - Operating systems
 - Applications
 - Data-in-transit
 - Data-at-rest
 - Data stores
 - Credentials
 - Policies and configuration 

## AWS Platform Compliance and Security Services: 
 - The AWS cloud infrastructure has been architected to be flexible and secure with world-class protection, by using it's built-in security features:
    - **Secure access** — Use API endpoints, HTTPS, and SSUTLS
    - **Built-in firewalls** — Virtual Private Cloud (VPC)
    - **Unique users** — AWS Identity and Access Management (IAM)
    - **Multi-factor authentication (MFA)**
    - **Private subnets** — AWS allowing private subnets on your VPC
    - **Encrypted data storage** — Encrypt your data in EBS, S3, Glacier, Redshift, and SQL RDS
    - **Dedicated connection option** — AWS Direct Connect
    - **Perfect Forward Secrecy** — ELB and CloudFront offer SSUTLS cipher suites for PFS
    - **Security logs** — AWS CloudTrail
    - **Asset identification and configuration** — AWS Config
    - **Centralized key management** — Centralized key management service
    - **Isolated GovCloud** — US ITAR regulations using AWS GovCloud
    - **CloudHSM** — Hardware Security Model (HSM) hardware based cryptographic storage
    - **Trusted Advisor** — With premier support (identify security holes) 

##  Incorporating Common Conventional Security Products:  
**OS-side Firewalls**
 - IPTABLES
 - FirewallD
 - Windows Firewall 
**AntiVirus Software**
 - TrendMicro:
 - Integrates into AWS EC2 instances 

##  DDoS Mitigation:  
 - When mitigating against DOS/DDOS attacks, use the same practice you would use on your on-premise components:
	- Firewalls:
		- Security groups
		- Network access control lists
		- Host-based firewalls
	- Web application Firewalls (WAFS)
	- Host-based or inline IDS/IPS (Trend Micro)
	- Traffic shaping/rate limiting 
 - Along with your traditional approaches for DOS/DDOS attack mitigation, AWS provides capabilities based on its elasticity:
	- You can potentially use CloudFront to absorb DOS/DDOS flooding attacks.
	- A potential attackers trying to attack content behind a CloudFront distribution is likely to send most requests to CloudFront edge locations, where the AWS infrastructure will absorb the extra requests with minimal to no impact on the back-end customer web servers. 
 - We MUST have permission to do Port Scanning on any of your EC2 instances.
 - INGRESS Filtering on all incoming traffic onto their network. 

## Encryption Solutions:  
 - S3 has built-in features that allow you to encrypt your data:
	- AES-256 bit encryption that encrypts data-at-REST in an S3 bucket.
	- AWS will decrypt the data and sent to you when you download it. 
 - EBS encrypted volumes:
	- You can select to have all data encrypted that is stored on an EBS for volume.
	- If a snap-shot it taken, that snap-shot is automatically encrypted. 
 - RDS encryption:
	- Aurora, MySQL, Oracle, PostegreSQL, and MS SQL all support this feature.
	- Encrypts the underlying storage space for the instance.
	- Automated Backups are encrypted (as well as snap-shots).
	- Read Replicas are encrypted.
	- RDS provides SSL endpoints to encrypt a connection to a DB instance. 

## Complex Access Control:  
 - Through IAM policies, AWS gives us the ability to create extremely complex and granular permission policies for our users (all the way down to the resource level).
 - IAM polices with resource level permissions:
	- EC2: Create permissions for instances such as reboot, start, stop, or terminate based all the way down to the instance ID.
	- EBS volumes: Attach, Delete, Detach.
	- EC2 actions that are not one of these above are not governed by resource-level at this time.
 - This is not EC2 limited can also include services such as RDS, S3, etc. 
 - Additional security measures, such as MFA authentication are also available when acting on certain resources:
	- For example, you can require MFA before an API request to delete an object within an S3 bucket. 

## CloudWatch for the Security Architect:  
CloudWatch Security
 - Request are signed with an HMAC-SHA1 signature, calculated from the request and the user's private key.
 - CloudWatch control API is only accessible via SSL encrypted endpoints.
 - CloudWatch access is given via IAM permission policies, essentially giving users permissions that are only needed (only give access to CloudWatch if they need access to CloudWatch).
 - Use CloudWatch and CloudTrail to monitor changes inside the AWS environment.
	- We can ask CloudWatch to notify us (via SNS) if there has been changes, for example:
		- Changes to IAM security credentials.
		- Assigning access policies to users.
		- Adding/deleting users.
 - It is important to know how we can use CloutWatch for security in our AWS environment. 

## CloudHSM: 
 - HSM (Hardware Security Module) is a dedicated physical machine/appliance isolated in order to store security keys and other types of encryption keys used within an application.
 - The key is used within the domain of the HSM appliance instead of being exposed outside the appliance. 
 - HSM Appliances have special security mechanisms to make them more secure:
	- The security key is only used within the HSM.
	- An HSM client is used to expose the APIs of the HSM.
	- So an application can communicate with HSM to do the encryption (or decryption) of the data that we are requesting.
	- They appliance is physically isolated from other resources.
	- Tamper resistant (build to notify via advanced logging).
	- On AWS, even though they are hosting the appliance, AWS engineers have NO access to the keys (only to manage and update the appliance).
	- If the keys are lost or reset (to access the appliance), you will never be able to access the data stored on the appliance. 
 - Some types of keys that might be stored on HSMs:
	- Keys used to encrypt file systems
	- Keys used to encrypt databases
	- Keys used to provide DRM
	- Used with S3 encryption
 - When to use CloudHSM instead of something like Key Management Service?
	- Generally, compliance requirements require it or internal security policy require it.
	- Not even AWS engineers have access to the keys on the CloudHSM appliance, only access to "manage" the appliance.
 
![](https://github.com/lannyzhujin/AWS_CSA_Feb_2018/blob/master/AWS_CSA-Associate/img/HSM.PNG)

# Disaster Recovery:  

**Business disaster recovery key words: Very important for AWS CSA Exam**

**Recovery time objective (RTO)**: Time it takes after a disruption to restore operations back to its regular service level, as defined by the companies operational level agreement. (i.e If the RTO is 4 hours, you have 4 hours to restore service back to an acceptable level). 
**Recovery point objective (RPO)**: Acceptable amount of data loss measured in time. (i.e if the system goes down at 10PM, and RPO is 2 hours, then you should recover all data as part of the application as it was before 8PM). 

Not only should you design for disaster recovery for your applications running on AWS, you can also use AWS as a disaster recovery solution for your on-premise applications or data. The AWS services used should be determined based off of the business RTO and RPO operational agreement. 

**Pilot Light**: A minimal version of your production environment that is running on AWS. This allows for replication from on-premise servers to AWS, and in the event of a disaster the AWS environment spins up more capacity (elasticity/automatically) and DNS is switch from on-premise to AWS. It is important to keep up to date AMI and instance configurations if following pilot light protocol. 

**Warm Standby**: Has a larger foot print than a pilot light setup, and would most likely be running business critical applications in "standby". This type of configuration could also be used as a test area for applications. 

**Multi-Site Solution**: Essentially clones your "production" environment, which can either be in the cloud or on premise. Has an active-active configuration which means instances size and capacity are all running in full standby and can easily convert at the flip of a switch. Methods like this could also be used to "load balance" using latency based routing or Route 53 failover in the event of an issue. 

**Services Examples**:
 - Elastic Load Balancer and Auto Scaling
 - Amazon EC2 VM Import Connector
 - AMI's with up to date configurations
 - Replication from on-premise database servers to RDS
 - Automate the increasing of resources in the event of a disaster
 - Use AWS Import/Export to copy large amounts of data to speed up replication times (also used for off site archiving)
 - Route 53 DNS Failover/Latency Based Routing Solutions
 - Storage Gateway (Gateway-cached volumes/Gateway-stored volumes) 
