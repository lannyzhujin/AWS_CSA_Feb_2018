# S3 Essentials:  
![](https://github.com/lannyzhujin/AWS_CSA_Feb_2018/blob/master/AWS_CSA-Associate/img/S3.PNG)
 - As AWS' main storage service, S3 can serve many purposes when designing highly available, fault tolerant, and secure application architecture. Including:
     - Bulk (basically unlimited) static object storage
     - Various storage classes to optimize cost vs. needed object availability/durability
     - Object versioning
     - Access restrictions via S3 bucket policies/permissions 
     - Object management via lifecycle policies
     - Hosting static files & websites
     - Origin for CloudFront CDN
     - File shares and backup/archiving for hybrid networks (via AWS Storage Gateway) 

## Important S3 Facts: 
 - Objects stay within an AWS region and are synced across all AZ's for extremely high availability and durability.
 - You should always create an S3 bucket in a region that makes sense to it's purpose:
     - Serving content to customers
     - Sharing Data with EC2 

## S3 Read Consistency Rules:
 - ALL regions now support read-after-write consistency for PUTS of new objects into S3.
     - Object can be immediately available after "putting" an object in S3
 - All regions use eventual consistency for PUTS overwriting existing objects and DELETES of objects. 

# S3 Buckets:  
 - Buckets are the main storage container of S3, and contain a grouping of information and have sub name spaces that are similar to folders (but yet are called folders).
 - Tags can be used to organize buckets (i.e. tag based on application the bucket belongs to). 
 - **Each bucket must have a unique name across ALL of AWS.**
 - **Bucket Limitations:**
     - Only 100 buckets can be created in an AWS account at a time.
     - Bucket ownership cannot be transferred once a bucket is created. 

# S3 Objects: 
 - Objects are static files that contain metadata information:
     - Set of name-key pairs
     - Contain information specified by the user, and AWS information such as storage type 
 - Each object must be assigned a storage type, which determines the object's availability, durability, and cost. 
 - By default, all objects are private. 
 - Objects can:
     - Be as small as 0 bytes and as large as 5 TB.
     - Have multiple versions (if versioning is enabled)
     - Be made publicly available via a URL
     - Automatically switch to a different storage class or deleted (via lifecycle policies)
     - Encrypted
     - Organized into "sub-name" spaces called folders 

## Object Encryption:
 - SSE (Server Side Encryption):
     - S3 can encrypt the object before saving it on the partitions in the data centers and decrypt it when it is downloaded
     - AES-256 
 - Or you can use your own encryption keys:
     - Considered client side encryption where you encrypt the data before upload 
 - SSL terminated endpoints for the API 

# S3 Folders:  
 - For simplicity, S3 supports the concept of "folders".
 - This is done only as a means of grouping objects.
 - Amazon S3 does this by using key-name prefixes for objects. 

**Amazon S3 has a flat structure, there is no hierarchy like you would see in a typical file system.**

# S3 Permissions:  
 - All buckets and objects are private by default - only the resource owner has access.
 - The resource owner can grant access to the resource (bucket/objects) through S3 "resource based policies" OR access can be granted through a traditional IAM user policy.
 - Resource based policies (for S3) are: 
      - **Bucket policies**
          - Are policies that are attached only to the S3 bucket (not an IAM user).
          - The permissions in the policy are applied to all objects in the bucket.
          - The policy specifies what actions are allowed or denied for a particular user of that bucket - such as:
              - Granting access to an anonymous User.
              - Who (a "principal") can execute certain actions like PUT or DELETE.
              - Restricting access based off of IP address (generally used for CDN management). 

      - **S3 access control lists**
          - Grant access to users in other AWS accounts or to the public.
          - Both buckets and objects has ACLs.
          - Object ACLs allow us to share an S3 object with the public via a URL link. 

# S3 Storage Classes:  
 - A storage class represents the "classification" assigned to each Object in S3. Current Storage Class types include:
    - Standard
    - Reduced Redundancy Storage (RRS)
    - Infrequent Access (53-IA)
    - Glacier 
 
 - Each storage class has varying attributes that dictate things like:
    - Storage cost
    - Object availability
    - Object durability
    - Frequency of access (to the object) 

## Standard:
 - Designed for general, all-purpose storage.
 - Is the default storage option.
 - 99.999999999% object durability ("eleven nines").
 - 99.99% object availability.
 - Is the most expensive storage class. 

## Reduced Redundancy Storage (RRS):
 - Designed for non-critical, reproducible objects.
 - 99.99% object durability.
 - 99.99% object availability.
 - Is less expensive than the standard storage class. 

## Infrequent Access (S3-IA):
 - Designed for objects that you do not frequently access, but must be immediately available when accessed.
 - 99.999999999% object durability.
 - 99.90% object availability.
 - Is less expensive than the standard/RRS storage classes. 

## Glacier:
 - Designed for long-term archival storage (not to be used for backups). 
 - May take several hours for objects stored in Glacier to be retrieved. 
 - 99.999999999% object durability
 - Is the cheapest S3 storage class (very low cost) 

### Glacier:  
 - Amazon Glacier is an archival storage type.
 - Used for data that is NOT accessed frequently.
 - "Check out" and "check in jobs" can take several hours, meaning how long it can take for the data to be changed and/or retrieved.
 - Integrates with Amazon S3 lifecycle policies for easy archiving.
 - Very inexpensive and cost effective archival storage solution.
 - Glacier should NOT be used as a backup solution. 

NOTE: Glacier now offers three levels of data retrieval (pricing varies):
 - Expedited: 1-5 minutes 
 - Standard: 3-5 hours
 - Bulk: 5-12 hours 

# S3 Versioning:  
 - S3 versioning is a feature to manage and store all old/new/deleted versions of an object. 

 - By default, versioning is disabled on all buckets/objects.
 - Once versioning is enabled, you can only "suspend" versioning. It cannot be fully disabled.
 - Suspending versioning only prevents new versions from being created. All objects with existing versions will maintain their older versions.
 - Versioning can only be set on the bucket level and applies to ALL objects in the bucket. 

 - Lifecycle policies can be applied to specific versions of an object.
 - Versioning and lifecycle policies can both be enabled on a bucket at the same time.
 - Versioning can be used with lifecycle policies to create a great archiving and backup solution in S3. 

# Lifecycle Policies:  
An object lifecycle policy is a set of rules that automate the migration of an object's storage class to a different storage class (or deletion), based on specified time intervals. 
 - By default, lifecycle policies are disabled on a bucket/object.
 - Are customizable to met your company's data retention policies.
 - Great for automating the management of object storage and to be more cost efficient.
 - Can be used with versioning to create a great archiving and backup solution in S3. 

Example:
 (1) I have a work file that I am going to access every day for the next 30 days.
 (2) After 30 days, I may only need to access that file once a week for the 60 next days.
 (3) After which (90 days total) I will probably never access the file again but want to keep it just in case. 

![](https://github.com/lannyzhujin/AWS_CSA_Feb_2018/blob/master/AWS_CSA-Associate/img/S3LifePolicy.PNG)

# S3 Event Notifications:  
 - S3 events notifications allow you to setup automated communication between S3 and other AWS services when a selected event occurs in an S3 bucket. 
 - Common event notification triggers include: 
    - RRSObjectLost (Used for automating the recreation of lost RRS objects)
    - ObjectCreated (for all or the following specific APIs called)
    - Put
    - Post
    - Copy
    - CornpleteMultiPartUpload 
 - Events notification can be sent to the following AWS services:
    - SNS
    - Lambda
    - SQS Queue  

# S3 Static Web Hosting:  
 - Amazon S3 provides an option for a low-cost, highly reliable web hosting service for static 
websites (content that does not change frequently). 
 - When enabled, static web hosting will provide you with an unique endpoint (url) that you can point to any properly formatted file stored in an S3 bucket. Supported formats include:
    - HTML
    - CSS
    - JavaScript 
 - Amazon Route 53 can also map human-readable domain names to static web hosting buckets, which are ideal for DNS failover solutions. 

Cross-Origin Resource Sharing (CORS):
 - CORS is a method of allowing a web application located in one domain to access and use resources in another domain. 
 - This allows web applications running JavaScript or HTML5 to access resources in an S3 bucket without using a proxy server.
 - For AWS, this (commonly) means that a web applications hosted in one S3 bucket can access resources in another S3 bucket. 

---
# S3-Storage-Transit-Service
![](https://github.com/lannyzhujin/AWS_CSA_Feb_2018/blob/master/AWS_CSA-Associate/img/S3-Storage-Transit-Service.PNG)
## Single Operation Upload:  
 - A single operation upload is "traditional" upload where you upload the file in one part.
 - A single operation upload can upload a file up to 5GB in size, however any file over 100MB should use multipart upload. 

## Multipart Upload:  
 - Multipart upload allows you to upload a single object as a set of parts.
 - Allows for uploading parts of a file concurrently
 - Allows for stopping/resuming file uploads
 - If transmission of any part fails, you can retransmit that part without affecting other parts.
 - After all parts of your object are uploaded, Amazon S3 assembles these parts and creates the object.
 - Required for objects 5GB and large, and highly suggested for use when objects are 100MB and larger.
 - Can be used to upload a file up to 5TB in size. 

## AWS Import/Export:  
 - AWS Import/Export gives the ability to take on-premise data and physically snail mail it to AWS (using a device that you own).
 - AWS will import the data to either S3, EBS, or Glacier within one business day of the physical device arriving at AWS. 
 
 - Benefits:
     - Off-site backup policy
     - Quickly migrate LARGE amounts of data to the cloud (up to 16TB per job)
     - Disaster recovery (AWS will even take s3 data and ship it back to you) 

## Snowball:  
 - Snowball is a petabyte-scale data transport solution.
 - Snowball uses an AWS provided secure transfer appliance.
 - Quickly move large amounts of data into and out of the AWS cloud. 

## Storage Gateway:  
 - Connects local data center software appliances to cloud based storage such as Amazon S3. 

**Gateway-Cached Volumes**
 - Create storage volumes and mount them as iSCSI devices on the on-premise servers.
 - The gateway will store the data written to this volume in Amazon S3 and will cache frequently access data on-premise in the storage device. 

**Gateway-Stored Volumes**
 - Store all the data locally (on-premise) in storage volumes.
 - Gateway will periodically take snapshots of the data as incremental backups and stores them on Amazon S3. 

