# EC2

![](https://github.com/lannyzhujin/AWS_CSA_Feb_2018/blob/master/AWS_CSA-Associate/img/EC2.PNG)

## EC2 Essentials:  
 - Amazon EC2 provides scalable virtual servers in the cloud.
 - An EC2 virtual server is known as an "instance" and can be made up of different instance types and sizes.
 - The virtual servers can run different operating systems, but most commonly run a flavor of Linux or Windows. 

### Instance Configuration:
 - EC2 instances are designed to mimic traditional on-premise servers, but with the ability to be commissioned and decommissioned on-demand for easy scalability and elasticity.
 - EC2 instances are primarily comprised of the follow components:
 - Amazon Machine Image (AMI): The operating system (and other settings).
 - Instance Type: The hardware (compute power, ram, network bandwidth, etc).
 - Network interface (public, private, or elastic IP addresses).
 - Storage: The instances "hard drive" (including two options).
     - Elastic Block Store (EBS) - which is "network persistent storage"
     - Instance Store - which is "ephemeral storage" 

### Other Important EC2 Facts:
 - A security group must be assigned to an instance during the creation process.
 - Each instance must be placed into an existing VPC, availability zone and subnet.
 - Automated (bootstrapping) custom launch commands can be passed into the instance during launch via "user-data" scripts.
 - "Tags" can be used to help name and organize provisioned instances.
 - Encrypted key-pairs are used to manage login authentication.
 - There are limits on the amount of instances you can have running in a region at any particular time. 

## EC2 Purchasing Options:  

### On-Demand: 
 - On-demand purchasing allows you to choose any instance type you like and provision/terminate it at any time (on-demand).
     - Is the **most expensive** purchasing option. 
     - Is the **most flexible** purchasing option. 
     - You are only charged when the instance is **running**:
         - **Per second pricing**: Amazon Linux and Ubuntu AMIs. (60 second billing minimum)
         - **Hourly pricing**: Windows, RHEL, and any other AMIs where per second pricing is not indicated. 
     - You can provision/terminate an on-demand instance at anytime. 

### Reserved: 
 - Reserved purchasing allows you to purchase an instance for a **set time period** of one (1) or three (3) years.
     - This allows for a **significant price discount** over using on-demand.
     - You can select to pay upfront, partial upfront, no upfront.
     - Once you buy a reserved instance, you own it for the selected time period and are **responsible for the entire price** - regardless of how often you use it. 

### Spot: 
 - Spot pricing is a way for you to "**bid**" on an instance type, and only pay for and use that instance when the spot price is **equal to or below** your "bid" price.
     - This option allows Amazon to sell the use of **unused instances**, for short amounts of time, at a **substantial discount**.
     - **Spot prices fluctuate** based on supply and demand in the spot marketplace.
     - You are **charged by the minute or hour** (same AMI conditions as on-demand instances).
     - When you have an active bid, an instance is **provisioned for you when the spot price is equal to or less than you bid price**.
     - A provisioned instances **automatically terminate when the spot price is greater than your bid price**.
     - Bid on unused EC2 instances for "non production applications". 

## EC2 Instance Type: 
 
 - Instances Types describe the "hardware" components that an EC2 instance will run on:
     - Compute power (processor/vCPU)
     - Memory (ram)
     - Storage Options/optimization (hard drive)
     - Network Performance (bandwidth) 

 - As an architect, it's important to use the proper instance type to handle your application's workload. 

 - There is a collection of preconfigured instance types that are grouped into families and types that you can choose from: 

TYPE | FAMILY |	Notes
-----|--------|-------
 t2 | General Purpose | "Burstable" performance instances
 m3 | General Purpose | Nice Balance
c3/c4 | Compute Optimized  | For high traffic front end fleets, web servers
d2 | Storage Optimized | For large-scale data warehouse or parallel file systems
 i2 | Storage Optimized | For large-scale data warehouse or parallel file systems
 g2 | GPU Optimized | For machine learning, high performance databases, rendering 
 p2 | GPU Optimized | For machine learning, high performance databases, rendering
r3/r4 |Memory Optimized | For databases, memcached, large deployments of enterprise applications 
xl | Memory Optimized | For databases, memcached, large deployments of enterprise applications

# EC2 Storage

Storage Options:
 - Amazon Elastic Block Store
 - Amazon EC2 Instance Store
 - Amazon Elastic File System (Amazon EFS)
 - Amazon Simple Storage Service (Amazon S3)

## EBS
### EC2 Elastic Block Store (EBS) Basics:  

 - EBS volumes are **persistent**, meaning that they can live beyond the life of the EC2 instance they are attached to.
 - EBS backed volumes are **network attached storage**, meaning they can be attached/detached to or from various EC2 instances.
 - However, they can only be attached to ONE EC2 instance at a time.
 - EBS volumes have the benefit of being backed up into a snapshot - which can later be restored into a new EBS volume. 

### EBS Performance:
 - EBS volumes measure input/output operations in IOPS:
     - IOPS are input/output operations per second
     - AWS measures IOPS in **256KB chunks** (or smaller)
     - Operations that are greater than 256KB are separated into individual 256KB chunks
     - For example, A 512KB operation would count as 2 IOPS
 - The type of EBS volume you specify greatly influences the I/O performance (IOPS ) your device will receive.
 - It is important as architects to understand if your application requires more (or less) I/O when selecting an EBS volume.
 - Even volumes with **"provisioned IOPS"** may not produce the performance you expect. If this is the case, an EBS optimized instance type is required, which prioritizes EBS traffic. 

### Initializing EBS Volumes:
 - New EBS volumes no longer need to be "pre-warmed".
 - **New** volumes will receive their maximum performance at the moment they are created.
 - Volumes created from an EBS snapshot must be **initialized**.
 - Initializing occurs the first time a storage block on the volume is read - and the performance impact can be impacted by up to 50%.
 - You can avoid this impact in production environments by manually reading all the blocks. 

### EBS Volume Types
EC2 Elastic Block Store Volumes: 

**General Purpose SSD (gp2):**
 - Use for dev/test environments and smaller DB instances
 - Performance of 3 IOPS/GiB of storage size (burstable with baseline performance)
 - Volume size of 1GiB to 16TiB
 - Considerations when using T2 instances with SSD root volumes (burstable vs. baseline performance) 

**Provisioned IOPS SSD (io1):**
 - Used for mission critical applications that require sustained IOPS performance
 - Large database workloads
 - Volume size of 4GiB to 16TiB
 - Performs at provisioned level and can provision up to 20,000 IOPS per volume 

**Throughput Optimized HDD (st1):**
 - 500 GiB - 16 TiB
 - Low cost HDD volume designed for frequently accessed, throughput-intensive workloads

**Cold HDD (sc1) :**
 - 500 GiB - 16 TiB
 - Lowest cost HDD volume designed for less frequently accessed workloads

### Previous Generation 
**EBS Magnetic:**
 - Low storage cost
 - Used for workloads where performance is not important or data is infrequently accessed
 - Volume size of Min 1GiB Max 1 TiB 

### EBS Snapshots:  
 - Snapshots are point-in-time backups of EBS volumes that are stored in S3. 

 - Snapshot properties:
    - Snapshots are incremental in nature.
    - A snapshot only stores the changes since the most recent snapshot, thus reducing costs (by only having to pay for storage for the "incremental changes" between snapshots).
    - However, if the "original" snapshot is deleted, all data is still available in all the other snapshots.
    - Even though snapshot storage only charges you for the amount of incremental data in each snapshot all prior data is still there.
    - Snapshots can be used to create fully restored EBS volumes 

 - A few other important snapshot notes:
    - Frequent snapshots of your data increases data durability - so highly recommended.
    - When a snapshot is being taken against the EBS volume, it can degrade performance so snapshots should occur during non-production or non-peak load hours. 

## EC2 Instance Store Volumes: 
 - Instance-store volumes are virtual devices whose underlying hardware is physically attached to the host computer that is running the instance. 
 - Instance store volumes are considered ephemeral data, meaning the data on the volumes only exists for the duration of the life of the instance.
 - Once the instance is "stopped" or "shutdown" the data is erased.
 - The instance can be rebooted and still maintain its ephemeral data. 
 - Viewing the Instance Block Device Mapping for Instance Store Volumes:
 
    When you view the block device mapping for your instance, you can see only the EBS volumes, not the instance store volumes. You can use instance metadata to query the complete block device mapping. The base URI for all requests for instance metadata is http://169.254.169.254/latest/.
    
## Elastic File System (EFS): 
 - EFS is a storage option for EC2 that allows for a scalable storage option.
 - EFS storage capacity is elastic.
 - The storage capacity will increase and decrease as you add or remove files.
 - Applications running on an EC2 instance using EFS will always have the storage they need, without having to provision and attach larger storage devices.
 - EFS is fully-managed (no maintenance required)
 - Supports the Network File System version 4.0 and 4.1 (NFSv4) protocols when mounting.
 - Best performance when using an EC2 AMI with Linux kernel 4.0 or newer. 

### Benefits of EFS:
 - The EFS file system can be accessed by one (or more) EC2 instances at the same time.
 - Shared file access across all your EC2 instances.
 - Application that span multiple EC2 instances can access the same data.
 - EFS file systems can be mounted to on-premises servers (when connected to your VPC via AWS Direct Connect).
 - This allows you to migrate data from on-prem servers to EFS and/or use it as a backup solution.
 - EFS can scale to petabytes in size, while maintaining low-latency and high levels of throughput.
 - You pay only for the amount of storage you are using. 

### Security:
 - Control file system access through POSIX permissions.
 - VPC for network access control, and IAM for API access control.
 - Encrypt data at rest using AWS Key Management Service (KMS). 

### When to use:
 - Big Data and analytics
 - Media processing workflows
 - Web Serving & Content Management 

# EC2 Network
## IP address

### Private IP Address:
 - All EC2 instances are automatically created with a PRIVATE IP address.
 - The private IP address is used for internal (inside the VPC) communication between instances. 

### Public IP Address:
 - When creating an EC2 instance, you have the option to enable (or auto-assign) a public IP address.
 - A public IP address is required if you want the EC2 instance to have direct communication with resources across the open internet.
     - i.e. If you want to directly SSH into the instance or have it directly serve web traffic.
 - Auto-assigning is based on the setting for the selected subnet that you are provisioning the instance in. 

### Elastic IP Address (EIP):
 - An EIP is a static IPv4 address designed for dynamic cloud computing.
 - An EIP is a public IPv4 address.
 - With an EIP you can attach a public IP address to an EC2 instance that was created with only a private IP address OR....
 - You can mask the failure of an instance or software by rapidly remapping the address to another instance in your account (i.e. detaching the EIP from one instance and attaching it to another).
 - Attaching an EIP to an instance will replace it's default public IP address for as long as it is attached. 

## ENI (Elastic Network Interfaces)
 - Hot attach - running instance
 - Warn attach - stopped instance
 - Cold attach - lauching instance

## Enhanced Networking on Linux
 > Enhanced networking uses single root I/O virtualization (SR-IOV) to provide high-performance networking capabilities on supported instance types.

### Enhanced Networking Types
 - Intel 82599 Virtual Function (VF) interface
     - Up to 10 Gbps
     - C3, C4, D2, I2, M4 (excluding m4.16xlarge), and R3 instances
 - Elastic Network Adapter (ENA)
     - Up to 25 Gbps 
     - C5, F1, G3, H1, I3, m4.16xlarge, M5, P2, P3, R4, and X1 instances 
     
# AMI
## Amazon Machine Images (AMIs):  
 - A preconfigured package (template) required to launch and EC2 instance; includes an:
    - operating system 
    - Software packages
    - Other required settings (root storage type & yirtualization type) 

 - Amazon Machine Images are used with Auto Scaling to quickly launch new servers on demand, and to quickly recover from disaster. 
 - You can create your own AMIs, choose from a list of free AWS/community provided AMIs, or choose one from the marketplace. 

## AMIs come in three main categories: 
 - Community AMIs: 
     - Free to use 
     - Generally with these AMIs you are just selecting the OS you want 
 - AWS Marketplace AM Is:
     - Pay to use
     - Generally comes packaged with additional, licensed software 
 - My AMIs:
     - AMIs that you create yourself 

# Virtualization
## Virtualization: 

 - Virtualization is the process of creating a "virtual" version of something rather than the actual version of that thing.
 - In AWS EC2, virtualization refers to using a "portion" of a server's computing power and storage to setup and run an operating system.
 - Virtualization for EC2 is run using the Xen Hypervisor software.
 - The maintenance of the physical AWS server and the Xen Hypervisor is handled by AWS. 

## Linux AMI Virtualization Types: 

 - HVM AM Is (Hardware Virtual Machine):
     - This virtualization type provides the ability to run an operating system directly on top of a virtual machine without any modification, as if it were run on the bare-metal hardware.
     - The Amazon EC2 host system emulates some or all of the underlying hardware that is presented to the guest.
     - Unlike PV guests, HVM guests can take advantage of hardware extensions that provide fast access to the underlying hardware on the host system. 

- PV AM Is (Paravirtual):
     - Guests can run on host hardware that does not have explicit support for virtualization, but they cannot take advantage of special hardware extensions such as enhanced networking or GPU processing.
     - Historically, PV guests had better performance than HVM guests in many cases, but because of enhancements in HVM virtualization and the availability of PV drivers for HVM AMIs, this is no longer true. 
     
## Bootstrapping & User-Data/Meta-Data:  
### Bootstrapping:
 - Refers to a self-starting process or set of commands without external input.
 - With EC2, we can bootstrap the instance (during the creation process) with custom commands (such as installing software packages, running updates, and configuring other various settings) 

### User-Data:
 - A step/section during the EC2 instance creation process where you can include your own custom commands via a script (i.e. a bash script) 
 - Here is an example of a bash script that will automate the process of updating the yum package installer, install Apache Web Server, and start the Apache service. 

``` 
 #!/bin/bash
 yum update -y
 yum install -y httpd
 service httpd start
```

### Viewing User-Data & Instance Meta-Data: 
 - When logged into an EC2 instance, you can view the instance user-data used during creation, or meta-data by executing one of the the following commands:
     - **curl http://169.254.169.254/latest/user-data (displays bootstrapping commands)**
     - **curl http://169.254.169.254/latest/meta-data (displays AMI, instance type, etc)**
# EC2 Placement Groups:  
 - A placement group is a cluster of instances within the same availability zone.
 - Used for applications that require an extremely low latency network between them.
 - AWS attempts to place all the instances as close as physically possible in the data center to reduce latency.
 - Instances within a placement group have a low-latency, 10 Gbps network connection between them.
 - Instances that are in the placement group need to have enhanced networking in order to maximize placement groups. 

## Troubleshooting Placement Groups:
 - If an instance in a placement group is stopped, once it is started again it will continue to be a member of the placement group.
 - It is suggested to launch all the required instances within a placement group in a single request, and that the same instance type is used for all instances within the placement group.
 - It is possible, if more instances are added at a later time to the placement group OR if a placement group instance is stopped and started again, to receive an "insufficient capacity error".
 - Resolve the capacity error by stopping all instances in the member group and attempting to start them again. 

## Important facts about Placement Groups:
 - Instances not originally launched/created in the placement group cannot be moved into the placement group.
 - Placement groups cannot be merged together.
 - A placement group cannot span multiple availability zones.
 - Placement group names must be unique within your own AWS account.
 - Placement groups can be "connected".
 - Instances must have 10 gigabit network speeds in order to take advantage of placement groups (proper instance type). 


# Others
 ### Shared responsibility model
 - AWS is responsible for DDOS protection, port scanning protection, and ingress network filtering. You are responsible for managing security groups, applying an SSL certificate to an ELB, and the installation of custom firewall software.
 

