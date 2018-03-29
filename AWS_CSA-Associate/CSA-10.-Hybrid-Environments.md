# Route 53 Essentials:  
![](https://github.com/lannyzhujin/AWS_CSA_Feb_2018/blob/master/AWS_CSA-Associate/img/RT53.PNG)
 - Route 53 is a domain management service (DNS hosting solution) provided by AWS.
 - Key features include:
     - Domain Registration
         - Register domain names, such as orionpapers.com
     - Domain Name System (DNS) service
         - Translates friendly domains names like www.orionpapers.com into IP addresses like 192.0.2.1.
         - Amazon Route 53 responds to DNS queries using a global network of authoritative DNS servers, which reduces latency. 
     - Health checking
         - Amazon Route 53 sends automated requests over the Internet to your application to verify that it's reachable, available, and functional. 
 - Route 53 can manage external DNS for domain routing - routing request for www.orionpapers.com to the proper AWS resources such as a CloudFront distribution, ELB, EC2 instance, or RDS server.
 - Route 53 is commonly used with an ELB to direct traffic from the domain to the ELB (and thus have traffic evenly distributed among servers running your application).
 - Route 53 can also be used to manage internal DNS for custom internal hostnames within a VPC as long as the VPC is configured for it.
 - Latency, GEO, basic, and failover routing policies allow for region-to-region fault tolerant architecture design.
 - You can easily configure for failover to S3 (if website bucket hosting is enabled) or CloudFront. 

## Route 53 Hosted Zones:  
 - A Hosted Zones stores DNS records for your domain.
 - Basically, it contains all the rules (record sets) that tells Route 53 what do to with DNS request. 
 
 - There are both public and private hosted zones: 
     - A **public hosted zone** is a container that holds information about how you want to route traffic on the Internet for a domain, such as linuxacademy.com, and its subdomains. 
	  - A **private hosted zone** is a container that holds information about how you want to route traffic for a domain and its subdomains within one or more Amazon Virtual Private Clouds. 
	  
 - After you create a hosted zone for your domain, such as orionpapers.com, you create resource record sets to tell the Domain Name System (DNS) how you want traffic to be routed for that domain.
 - Hosted zones come pre-populated with NS (name server) and SOA (start of authority) record sets. 

## Route 53 Record Sets: 
 - Record sets are instructions that actually match domain names to IP addresses.
 - Record sets are comprised of various options, including:
     - Record type
     - Standard/alias
     - Routing policy
     - Evaluate target health
	 
### Common record types include: 
 - A: Used to point a domain to an IPv4 IP address.
 - AAAA: Used to point a domain to an IPv6 IP address.
 - CNAME: Used to point a host/name to another host/name.
 - MX: Used to route email (mail exchange). 
 
### Alias Record Sets:
 - Instead of an IP address (standard record sets), an alias record set contains a pointer to an AWS specific resource, such as:
     - An elastic load balancer
     - CloudFront distribution
     - Elastic Beanstalk environment
     - Amazon S3 bucket that is configured as a static website

### Routing Policy:
 - Simple: Route all traffic to one endpoint
 - Weighted: Route traffic to multiple endpoints (manual load balancing)
 - Latency: Route traffic to an endpoint based on the users latency to various endpoints
 - Failover: Route traffic to a "secondary" endpoint if the "primary" is unavailable.
 - Geolocation: Route traffic to an endpoint based on the geographical location of the user. 

### Evaluate Health Check:
 - Can monitor the health of your application and trigger an action. 

## S3 for DNS Failover:  
 - By using a failover routing policy in a Route 53 DNS record set, an S3 bucket can be used as a failover endpoint.
 - This can provide an extremely reliable backup solution if your primary endpoint fails.
 - And even though S3 should only be used for static web hosting, it gives you the opportunity to provide your users with some type of information until the primary endpoint is working again.
 - An S3 bucket can also be used as a primary endpoint, if you just want to host a simple static site. 

**Note: For a DNS record to use an S3 bucket as an endpoint, the bucket name MUST be the same as the domain name.**

## CloudFront Essentials: 
![](https://github.com/lannyzhujin/AWS_CSA_Feb_2018/blob/master/AWS_CSA-Associate/img/CloudFront.PNG)
 - CloudFront is a global CDN which delivers content from an "origin" location (the source of the content) to an "edge" location (AWS CDN data center).
 - A edge location allows the caching of static objects from the origin location.
 - An origin can be an:
     - S3 bucket
	 - Elastic Load Balancer that distributes requests among origin EC2 instances.
 - CloudFront can integrate with Route 53 for "alternate" CNAMEs.
     - This allows you to create a URL such as http://cdn.mydomain.com that works with your distribution. 

### CloudFront Benefits:
 - Users experience lower latency and content load time.
 - Reduces load on your applications resources (origin services) - thus reducing cost. 
 
### Updating Cached Files:
 - Caching is done based off the object name.
 - In order to serve a new version of an object, either create a new object with a new name or create an "invalidation" on the CloudFront distribution based off the object name.
 - "Invalidations" have a cost, so if you have to invalidate a large CloudFront distribution then perhaps you should just create a new distribution and move DNS names.
 - Cached objects can also be set with a specific expiration time/date, or set to not cache at all. 

### Signed URLs:
 - Signed URLs allow access to "private content" by creating a temporary, one-time-use URL based off of the number of seconds you want it to be accessible. 
 - Signed with a X.509 certificate. 

## CloudFront Origin:  
 - An "origin" location is the source of the content (static objects).
 - An origin can be an:
     - S3 bucket
	 - Elastic Load Balancer that distributes requests among origin EC2 instances.

## Edge Location:  
- An Edge Location is an AWS datacenter which does not contain AWS services.
 - Instead, it is used to deliver content to parts of the world. 
 
- An example would be CloudFront, which is a CDN:
     - Cached items such as a PDF file can be cached on an edge location which reduces the amount of "space/time/latency" required for a request from that part of the world. 
 
## CloudFront Performance Considerations: 
 - CloudFront performance can be affected by:
     - File size and type of file.
     - Having to remake the request from the Edge location to the origin. 
	     - Downloading the object from the origin takes time.
		 - As well as writing it to cache and responding to the end user request
		 - The more requests that have to go to the origin, the higher the load is on your source. Which can also cause latency and load performance issues. 
     - The end location that the user's request goes to is dependent upon a "DNS check" to determine the closest EDGE location. So slow DNS issues can cause performance issues. 
     - Query strings (request to the origin to serve a specific object) reduce cache "hits": 
	     - htto://cdn.linuxacademy.comPauerythis=auerythat
		 - It reduces performance because query strings are often unique so it reduces the cache hits and also requires extra "work" in order to forward to the origin location. 
 - CloudFront Performance can be increased by: 
     - Longer cache periods increases performance (less frequent request to the source). 

# VPN Essentials: 
![](https://github.com/lannyzhujin/AWS_CSA_Feb_2018/blob/master/AWS_CSA-Associate/img/VPN.PNG)
 - A virtual private network enables the ability to extend a subnet from one geographic location to another geographic location on two separate networks.
 - Extending the subnets allows the network at location "A" to communicate internally with all resources at location "B". 
 - This is essentially "extending" the on-premise network to the cloud, or the cloud to the on-premise network. 
 
 - For AWS, this allows us to communicate with all resources (like an EC2 instance) internally without the need for public IP addresses and an internet gateway.
 - It also provides an additional level of security by ensuring that traffic sent using the VPN is encrypted. 
 
 - The VPN connection has two parallel routes (IPsec tunnels), which is for redundancy.
 - Only one Virtual Private Gateway can be attached to a VPC (just like only one IGW can be attached to a VPC).
 - A VPC can have both a VPG and an IGW attached at the same time. 

 ## Virtual Private Gateway (VPG):  
 - A virtual private gateway acts as the "connector" on the VPC (AWS) side of the VPN connection.
 - The VPG is connected to the VPC. 

## Customer Gateways:  
 - A customer gateway is a physical device or software application at the on-premise location that acts as the "connector" to the VPN connection. 
 - In your AWS account, the customer gateway component is where you configure the public IP (internet routable static IP) address of the physical device or software application at the on-premise location. 
 
Note: Both a VPG and a Customer Gateway are required to establish a VPN connection.  

## VPN Connection:  
 - The VPN connection is the actual link between the virtual private gateway and the cusotmer gateway.
 - This connection is setup and managed in AWS.

 - Each connection uses two IPsec tunnels for redundancy. 
 
# Router: 
 - AWS has dispensed with the concept of having users physically setup and manage a "router".
 - However, it is important understand that route tables are actually part of a "router" assigned to your VPC. 

 - When setting up a VPN, the route table (for the subnet you wish to extend) must include routes for the on-premise network that are used by the VPN, and point them to the Virtual Private Gateway. 

# Direct Connect Essentials:  
![](https://github.com/lannyzhujin/AWS_CSA_Feb_2018/blob/master/AWS_CSA-Associate/img/DirectConnect.PNG)
 - AWS Direct Connect is a service that provides a dedicated network connection between your network and one of the AWS Direct Connect locations.
 - This is done through an authorized Direct Connect Provider (i.e. Verizon or other ISPs)
 - Does not require hosting any router/hardware at the Direct Connect Partner location, only requires a Direct Connect location and a participating backbone provider.
 - An AWS Direct Connect location provides access to the AWS region it is associated with.
 - It does not provide access to other AWS regions. 
![](https://github.com/lannyzhujin/AWS_CSA_Feb_2018/blob/master/AWS_CSA-Associate/img/DirectConnect1.PNG)
### Direct Connect Benefits:
 - Reduce network costs:
     - Reduce bandwidth commitment to corporate ISP over public internet.
	 - Data transferred over direct connect is billed at a lower rate by Amazon (data in/out). 
	 
- Increase network consistency:
     - Dedicated private connections reduce latency (over sending the traffic via public routing). 
	 
- Dedicated private network connection to on-premise:
     - Connect the direct connect connection to a VGW in your VPC for a dedicated private connection from on-premise to VPC.
     - Use Multiple VIF (Virtual Interfaces) to connect to multiple VPCs. 

## Cross-network Connection (Cross Connect):  
 - The physical connection between your network and the Direct Connect authorized partner, which then handles the routes and connections to AWS networks. 

##  Private Virtual Interface:  
 - A Private Virtual Interfaces allows you to interface with an AWS (VPC).
     - With automatic route discovery using BGP
     - Requires a public or private ASN number
 - Can only communicate with internal IP addresses inside of EC2.
 - Cannot access public IP addresses, as Direct Connect is NOT an internet provider.
 - This is a dedicated private connection which works like a VPN.
 - For best practice, use two Direct Connect connections for active-active or active-failover availability.
 - You can also use VPN as a backup to direct connect connections.
 - You can create multiple private virtual interfaces to multiple VPC's at the same time. 

## Public Virtual Interface:  
- A Public Virtual Interface allows you use a Direct Connect conneciton to connect to public AWS endpoints:
     - Any AWS service (for exmample, DynamoDB and Amazon S3).
 - Requires public CIDR block range.
 - And even though we are accessing public endpoints, the connection maintains consistent traffic consistency as it is sent over your dedicated network. 

# Storage Gateway Essentials:  
![](https://github.com/lannyzhujin/AWS_CSA_Feb_2018/blob/master/AWS_CSA-Associate/img/StorageGW.PNG)
 - Storage Gateway connects local data center software appliances to cloud based storage such as Amazon S3.
 - It does this though the Storage Gateway virtual appliance, which connects directly to your local infrastructure as a file server, a local disk volume, or as a virtual tape library (VTL).
 - It can maintain frequently accessed data on-premises (providing low-latency performance) while storing all other data in:
    - S3
    - EBS
    - Glacier
 - Storage Gateway also integrates your data with:
    - AWS encryption
    - Identity management
    - Monitoring 

**Gateway-Cached Volumes** (Store on S3)
 - Create storage volumes and mount them as iSCSI devices on the on-premise servers.
 - The gateway will store the data written to this volume in Amazon S3 and will cache frequently access data on-premise in the storage device. 

**Gateway-Stored Volumes** (Store on server)
 - Store all the data locally (on-premise) in storage volumes.
 - Gateway will periodically take snapshots of the data as incremental backups and stores them on Amazon S3. 

# VPC Peering Essentials: 
![](https://github.com/lannyzhujin/AWS_CSA_Feb_2018/blob/master/AWS_CSA-Associate/img/VPCPeering.PNG)
 - VPC peering is used to extend your private network from one VPC, or one subnet, or specifically one instance, to another VPC.
 - This is for sharing internal resources, via private IP addresses.
 - VPC peering can occur between two VPCs that are in the same region, or two VPCs that are in different regions (called inter-region VPC peering).
 - You can configure VPC peering between two VPCs in different accounts (inter-region VPC peering is also possible across accounts).
 - To peer VPCs, they must have separate (non-overlapping) CIDR block ranges.
 - Transitive connections are **NOT** allowed.
 - You can configure the peering to connect the entire VPC, or just specific subnets. 

