AWS Global Infrastructure</br>
Regions = agroup of AWS resource, comprised of multiple of AZ</br>
AZ Availabiliy Zones</br>
------
VPC Virtual Private Cloud

A default VPC including:
- IGW internet gateway
- A route table
- A network access control list
- Subnets to provision AWS resource in (e.g. EC2 instance)
![](img/VPC_Basic.PNG)

------

 - IGW<br>
 - RouteTables<br>
You cannot delete RT if it has dependencies<br>
![](img/RTs.PNG)

 - NACL Network Access Control List<br>
 (1) Rules are evaluated from lowest to highest based on "rule #"<br>
 (2) The first rule found that applies to the traffic type is immediatly applied, regardless of any rules that come after it (have a higher "rule #").<br>
 (3) The 'default' NACL allows all traffic to the default subnets.<br>
 (4) Any new NACLs you create DENY all traffic by default.<br>
 (5) A subnet can only be associated with ONE NACL as a lime.<br>
 (6) An NACL allows or denies traffic from entering a subnet. Once inside the subnet, other AWS resources (i.e. EC2 instances) may have an additional layer of security (security groups).<br>

 ![](img/NACL.PNG)
