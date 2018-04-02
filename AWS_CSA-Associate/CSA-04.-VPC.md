# VPC
![](https://github.com/lannyzhujin/AWS_CSA_Feb_2018/blob/master/AWS_CSA-Associate/img/PVC_netowrk.PNG)
## Essential
 > Amazon Virtual Private Cloud (Amazon VPC) enables you to launch AWS resources into a virtual network that you've defined. This virtual network closely resembles a traditional network that you'd operate in your own datacenter, with the benefits of using the scalable infrastructure of AWS.

 - A VPC is designed to resemble: 
     - Private on-premise data centers
     - Private corporate network 

 - Private network features available in AWS VPCs:
     - Private and Public subnets
     - Scalable architecture
     - Ability to extend corporate/on-premise network to the cloud as if it was part of your network (VPN) 

 - Important VPC Facts:
     - A VPC is housed within a chosen AWS region.
     - A VPC spans multiple availability zones within a region.
     - This allows you to provision redundant resource in separate availability zones while having them accessible on the same network (foundation of high availability and fault tolerant architecture).
     - AWS provides a DNS server for your VPC so each instance has a hostname. However, you can run your own DNS servers by changing the DHCP option set configuration within the VPC. 

### Benefits of a Virtual Private Cloud (VPC):  

 - Ability to launch instances into a subnet.
 - Ability to define custom CIDR (IP address range) inside each subnet.
 - Ability to configure routes between subnets via route tables. 
 - Ability to configure an internet gateway to provide a route to the internet for resources launched inside the VPC. 
 - Ability to create a layered network of resources. 
 - Ability to extend your on-premise network into the cloud with VPN/VPG and an IPsec VPN tunnel. 
 - Layered Security:
     - Instance level Security Groups (firewall on the instance level)
     - Subnet level network ACLs (firewall on the subnet level) 

### The Default VPC: 
 - The default VPC is the VPC that comes preconfigured in your AWS account when it is first created.
 - The default VPC has a different setup than a non-default VPCs.
 - The default VPC is meant to allow the user easy access to a VPC without having to configure it from scratch.
 - In the default VPC, all subnets have a route to the internet via route table and an attached IGW.
 - Each instance launched in the default VPC (by default) has a private and public IP address (defined on the subnet settings). 

### VPC Limits:  
 - 5 VPCs per region (more available upon request) 
 - 5 internet gateways per region (this is equal to your VPC limit because you can only have one 
internet gateway attached to a VPC at a time)
 - 50 customer gateways per region
 - 50 VPN connections per region
 - 200 route tables per region / 50 entries per route table
 - 5 elastic IP addresses
 - 500 security groups per VPC
 - 50 rules per security group
 - 5 security groups per network interface (security groups although generally referred to as being on the on the instance level are technically on the VPC level) 

# IGW

## Internet Gateway:  
 - Is a VPC component that allows communication between instances in your VPC and the Internet.
 - Is a horizontally scaled, redundant and highly available.
 - It imposes no availability risks or bandwidth constraints on your network traffic.
 - Provides NAT translation for instances that have a public IP addresses assigned (public IP to private IP). 

NOTE: Your "default" VPC already has an IGW attached. 

Internet Gateway rules and details you need to know:
 - Only 1 IGW can be attached to a VPC at a time.
 - An IGW cannot be detached from a VPC while there are active AWS resources in the VPC (such as an EC2 instance or RDS Database)
 - An IGW must be attached to a VPC if the resources inside the VPC need to connect to resources via the open internet. 

"To enable access to or from the internet for instances in a VPC subnet, you must attach an Internet gateway to your VPC, ensure that your subnet's route table points to the Internet Gateway, ensure that instances in your subnet have a public IP address or Elastic IP address, and ensure that your network access control and security group rules allow the relevant traffic to and from your instance"— AWS 

# Route Tables

"A route table contains a set of rules, called routes, that are used to determine where network traffic is directed." 

- Amazon web Services 
 - A route table's rules are comprised of two main components:
     - Destination: The CIDR block range of the target (where the data is routed to).
     - Target: A name identifier of where the data is being routed to. 
 - By default, all subnets traffic is allowed to each other available subnet within your VPC which is called the local route.
 - You cannot modify the local route 
 - Unlike an IGW, you can have multiple "active" route tables in a VPC
 - You cannot delete a route table if it has "dependancies" (associated subnets) 
Best practice is to leave the default route table and create a new route table when new routes are needed for specific subnets. 

NOTE: The "default" VPC already has a "main" route table. 

# Subnets
  
"When you create a VPC, it spans all of the Availability Zones in the region. After creating a VPC, you can add one or more subnets in each Availability Zone. Each subnet must reside entirely within one Availability Zone and cannot span zones." -Amazon Web Services 

 - Subnets MUST be associated with a route table.
 - A PUBLIC subnet HAS a route to the Internet.
     - It is associated with a route table that has an IGW attached.
 - A PRIVATE subnet does **NOT** have a route to the Internet.
     - It is associated with a route table that does NOT have an IGW attached. 
 - Instances launched into a private subnet can't communicate with the internet.
 - This creates a higher level of security, but it creates a limitation of an instance not being able to download software and/or updates.
 - This issue is solved by routing traffic through a NAT instance. 
 - By default all subnets traffic is allowed to each other available subnet within via the local target in the route table.
 - A subnet is located in one specific availability zone, and does not span AZs. 

NOTE: The "default" VPC already has subnets created and associated with a route table. 

# Network Access Control List (ACLs)
Network Access Control List (ACLs) Essentials:  
 - ACLs Operate at the network/subnet level.
 - They support allow AND deny rules for traffic traveling into or out of a subnet.
 - They are **stateless**: so return traffic must be allowed through an outbound rule. 
 - They process rules in number order when deciding whether to allow traffic.
 - Rules are evaluated in order, starting with the lowest rule number- for example:
     - If traffic is denied at a lower rule number and allowed at a higher rule number, the allow rule will be ignored and the traffic will be denied.
     - The last rule in every ACL is a "catch all" deny rule. • This means that unless a protocol/port is explicitly allowed, it will be denied. 
 - A network access control list (NACL) is an optional layer of security for your VPC that acts as a firewall for controlling traffic in and out of one or more subnets. 
 - Best practice to increment numbers by 10 so if you have to place in a rule in a certain order it does not create an issue 

NOTE: Your "default" VPC already has a NACL and it is associated with the default subnets. 

# Security Group

Security Groups:  
 - Security groups are very similar to NACLs in that they allow/deny traffic.
 - However, security groups are security for the instance level (as opposed to the subnet level with ALCs).
 - In addition, the way allow/deny "rules" work are different from ACLs:
     - Security groups support only allow rules. 
     - They are **stateful**: so return traffic requests are allowed regardless of rules.
     - All rules are evaluated before deciding to allow traffic. 
