# High Available & Fault Tolerant VPC Networking

![](https://github.com/lannyzhujin/AWS_CSA_Feb_2018/blob/master/AWS_CSA-Associate/img/High-Available-Fault-Tolerant-VPC.PNG)

## Elastic Load Balancer (ELB) Essentials:  
 - **Load balancing** (as a concept) is a common method used for distributing incoming traffic among servers.
 - An Elastic Load Balancer is an EC2 service that automates the process of distributing incoming traffic (evenly) to all the instances that are assocaited with the ELB.
 - An elastic load balancer can load balance traffic to multiple EC2 instances located across multiple availability zones.
     - This allows for highly availability and fault tolerant architecture.
 - Elastic load balancing should be paired with **Auto Scaling** to enhance high availability and fault tolerance, AND allow for automated scalability and elasticity.
 - An ELB has it's own DNS record set that allows for direct access from the open internet access. 
 - Connection draining feature
 - [Comparision of ELB products](https://amazonaws-china.com/elasticloadbalancing/details/#compare)

### Classic ELB:  
![](https://github.com/lannyzhujin/AWS_CSA_Feb_2018/blob/master/AWS_CSA-Associate/img/Classic-ELB.PNG)
 - A "classic" elastic load balancer is designed for simple balancing of traffic to multiple EC2 instances.
 - There are no granular routing "rules" - all instances get routed to evenly, and no special routing request can be made based on specific content request from the user.
 - Classic load balancing is best used when all instances (that are being served traffic) contain the same data. 

### Application Elastic Load Balancer: 
![](https://github.com/lannyzhujin/AWS_CSA_Feb_2018/blob/master/AWS_CSA-Associate/img/App-ELB.PNG)
 - Lay 7 load balancer
 - An "Application" elastic load balancer is designed for complex balancing of traffic to multiple EC2 instances using Content-based "rules".
 - Content-based rules (setup on the listener) can be configured using:
     - Host-based rules: Route traffic based on the host field of the HTTP header
     - Path-based rules: Route traffic based on the ULR path of the HTTP header
     - This Allows you to structure your application as smaller services, and even monitor/auto-scale based on traffic to specific "target groups".
 - Target Group - Each target group is used to route requests to one or more registered targets.
 - An Application ELB also supports ECS Containers, HTTPS, HTTP/2, WebSockets, Access Logs, Sticky Sessions, and AWS WAF (Web Application Firewall). 

### Network ELB
![](https://github.com/lannyzhujin/AWS_CSA_Feb_2018/blob/master/AWS_CSA-Associate/img/Network-ELB.PNG)
 - Lay 4 load balancer
 - Ability to handle volatile workloads and scale to millions of requests per second.
 - Support for static IP addresses for the load balancer. You can also assign one Elastic IP address per subnet enabled for the load balancer.
 - Support for registering targets by IP address, including targets outside the VPC for the load balancer.
 - Support for routing requests to multiple applications on a single EC2 instance. You can register each instance or IP address with the same target group using multiple ports.
 - Support for containerized applications. Amazon Elastic Container Service (Amazon ECS) can select an unused port when scheduling a task and register the task with a target group using this port. This enables you to make efficient use of your clusters.
 - Support for monitoring the health of each service independently, as health checks are defined at the target group level and many Amazon CloudWatch metrics are reported at the target group level. Attaching a target group to an Auto Scaling group enables you to scale each service dynamically based on demand.

### Other important ELB facts:
 - When used within a VPC, and ELB can act as an **internal** load balancer and load balance to internal EC2 instances on private subnets (as often done with multi-tier applications).
 - ELBs will automatically stop serving traffic to an instance that becomes unhealthy (via health checks).
 - An ELB can help reduce compute power on an EC2 instance by allowing for an SSL certificate to be applied directly to the elastic load balancer. 

## Auto Scaling: 
 - Auto Scaling is a service (and method) provided by AWS that automates the process of increasing or decreasing the number of provisioned on-demand instances available for your application. • Auto scaling will increase or decrease the amount of instances based on chosen Cloudwatch metrics. 
 - For example: If your application's demand increases un-expectantly, auto scaling can automatically scale up (add instance) to meet the demand and terminate instances when the demand decreases. • This is known as "elasticity" in the AWS environment. 

### Auto Scaling has two main components: 
 - **Launch Configuration:**
     - The EC2 "template" used when the auto scaling group needs to provision an additional instance (i.e. AMI, instance type, user-data, storage, security groups, etc) 
 - **Auto Scaling Group:**
     - All the rules and settings that govern if/when an EC2 instance is automatically provisioned or terminated.
         - Number of MIN & MAX allows instances
         - VPC & AZs to launch instances into
         - If provisioned instances should receive traffic from a ELB
         - Scaling policies (cloudwatch metrics thresholds that trigger scaling)
         - SNS notifications (to keep you informed when scaling occurs)
 
**NOTE: For architecture to be considered highly available and fault tolerant - it MUST have and ELB serving traffic to and ASG with a MIN of two instances located in separate availability zones.**

# Bastion Host/NAT Networking
![](https://github.com/lannyzhujin/AWS_CSA_Feb_2018/blob/master/AWS_CSA-Associate/img/Bastion_NAT_Networking.PNG)
## Bastion Host: 
 - A Bastion Host is an EC2 instance that lives in a public subnet, and is used as a "gateway" for traffic that is destined for instances that live in private subnets.
 - This means that we can use a bastion host as a "portal" to access EC2 instances that are located in a private subnet.
 - A bastion host is considered the "critical strong point" of the network - as all traffic must pass through it first.
 - A bastion host should have increased and extremely tight security (usually with extra 3rd party security and monitoring software installed).
 - A bastion host can be used as an access point to "ssh" into an internal network (to access private resources) without a VPN (virtual private network). 

"A system identified by the firewall administrator as a critical strong point in the network's security. Generally, bastion hosts will have some degree of extra attention paid to their security, may undergo regular audits, and may have modified software"- Marcus J. Ranum 

## NAT Gateway:  
 - A NAT Gateway is designed to provide EC2 instances that live in a private subnet with a route to the internet (so they can download software packages and updates).
 - A NAT Gateway will prevent any hosts located outside of the VPC from initiating a connection with instances that are associated with it.
 - A NAT Gateway will only allow incoming traffic through if a request for it originated from an instance in a private subnet.
 - A NAT Gateway is needed because instances launched into private subnets can't communicate with the open internet.
 - Placing instances in a private subnet creates a higher level of security, but also creates the limitation of the instances not being able to download software and software updates. 

**A NAT Gateway MUST:**
 - Be created in a public subnet.
 - Be part of the private subnets route table. 

### NAT Instance:
 - A NAT Instance is identical to a NAT gateway in its purpose.
 - However, it is executed differently by configuring an actual EC2 instance to do the same job.
 - A NAT Instance is starting to become more of a legacy feature in AWS.
 - However, questions about them may still appear on the exam. 

# Trouble Shooting

## EC2 Troubleshooting:  
### Connectivity issues to an EC2 instance
 - Correct ports on the security group are may not be open. 
### Cannot attach and EBS volume to an EC2 instance
 - EBS volumes must live in the same availability zone as the EC2 instance they are to be attached to.
 - You can create a snapshot from the volume and launch the volume in the correct availability zone. 
### Cannot launch additional instances
 - You have probably reached the EC2 limit and need to contact AWS to increase limit. 
### Unable to download package updates
 - The EC2 instance may not have a public/Elastic IP address, and/or does not belong to a public subnet. 
### Applications seeming to slow down on T2 micro instances
 - T2 micro instances utilize CPU credits (for "burstable" processing), so chances are your application is using too much processing power and needs a larger instance or different instance type. 
### AMI unavailable in other regions
 - AMI's are only available in the regions that they are created.
 - An AMI can be copied to another region but will receive a new AMI id.
### "Capacity error" when attempting to launch an instance in a placement group
 - Start and stop all the instances in the placement group (AWS tries to locate them as close as possible). 

## VPC Troubleshooting:  
### New EC2 instances are not automatically being assigned a public IP address
 - Modify the Auto-Assign Public IP setting on the subnet. 
### NAT Gateway is configured but instances inside a private subnet still cannot download packages
 - Need to add 0.0.0.0/0 route to the NAT gateway on the route table for private subnets. 
### Traffic is not making it to the instances even though security group rules are correct
 - Check the Network Access Control Lists to ensure the proper ports from the proper sources are open. (also check you IGW and route table settings) 
### Error when attempting to attach multiple internet gateways to a VPC
 - Only one Internet gateway can be attached to a VPC at any given time. 
### Error when attempting to attach multiple Virtual Private gateways to a VPC
 - Only one Virtual Private Gateway can be attached to a VPC at any given time.
### VPC Security group (for EC2 instances) does not have enough rules for the required application
 - Assign the EC2 instance to multiple security groups. 
### Cannot SSH/communicate with resources inside of a private subnet
 - Either you have not setup a VPN, or you have not connected to an EC2 instance (Bastion host) within the VPC to launch a connection from. 
### Successful site-to-site VPN connection but unable to access extended resources
 - Need to add on-premise routes to the Virtual Private Gateway route table. 
### Failure to create a VPC peering connection between two VPC's in different regions
 - Peering connections can only be created between two VPC's in the same region. 

## ELB Troubleshooting:  
### Load balancing is not occurring between instances in multiple availability zones
 - Make sure "Enable Cross-Zone load balancing" has been selected. 
### Instances are healthy but are not registering as healthy with the ELB
 - Check configuration for the "health check" to make sure you have selected the proper ping protocol, ping port, and ping path. 
### The ELB is configured to listen on port 80, but traffic is not making it to the instances that belong to the ELB
 - You may have mistaken the "Listeners" for the security group. Listeners are not the same as the security group rules, port 80 still needs to be open on the security group that the ELB is using. 
### Access logs on web servers show IP address of the ELB not the source traffic
 - Enable Access Logs to Amazon S3 (found under "attributes") 
### Unable to add instances from a specific subnet to the ELB
 - Most likely the subnet that the instance lives in has not been added to the ELBs configuration. 

## Auto Scaling Troubleshooting:  
### An Auto Scaled instance continues to start and stop (or create/terminate) in short intervals
 - The scale-up and scale-down thresholds may be too close to each other. Either raise the scale-up threshold or lower the scale-down threshold. 
### Auto Scaling does not occur even though scaling policies are configured correctly
 - The "max" number of instances set in the auto scaling group may have been reached. 

Please note: that there are more troubleshooting considerations that are outside the scope of the AWS CSA. However, they are covered in the AWS SysOps Certification and training course at the Linux Academy. 

[[Home]](https://github.com/lannyzhujin/AWS_CSA_Feb_2018/blob/master/AWS_CSA-Associate/Home.md)
