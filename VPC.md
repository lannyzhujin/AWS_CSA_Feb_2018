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
 Rules are evaluated based on "rule #" from lowest to highest
 ![](img/NACL.PNG)
