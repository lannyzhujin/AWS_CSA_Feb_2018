# CloudFormation
![](https://github.com/lannyzhujin/AWS_CSA_Feb_2018/blob/master/AWS_CSA-Associate/img/CloudformationEssentials.PNG)
## Cloud formation Essentials:  
 - CloudFormation is the pure definition of infrastructure as code:
     - You can "convert" your application's architecture into a JSON formatted template (so your architecture is literally code).
     - You can then use that JSON template to deploy out updated or replicated copies of that architecture to multiple regions. 

Benefits
 - Saves time - you don't have to manually create duplicate architecture in additional regions.
 - Since your infrastructure is now code, you can version control your infrastructure. Allowing for rollbacks to previous versions of your infrastructure if a new version has issues.
 - Allows for backups of your infrastructure.
 - Great solution for disaster recovery. 

# Elastic Beanstalk
![](https://github.com/lannyzhujin/AWS_CSA_Feb_2018/blob/master/AWS_CSA-Associate/img/ElasticBeanStalk.PNG)
## Elastic BeanStalk Essentials:  
 - Elastic Beanstalk is designed to make it easy to deploy less complex applications.
 - This helps reduce the management required for building and deploying applications.
 - Elastic Beanstalk is used to deploy out easy, single-tier applications that take advantage of core services such as:
     - EC2
     - Auto Scaling
     - ELB
     - RDS
     - SQS
     - CloudFront 
	 
 - Why/when to use Elastic Beanstalk:
     - In order to quickly provision an AWS environment that require little to no management.
     - The application fits within the parameters of the Beanstalk service.
     - Can deploy from repositories or from uploaded code files.
     - Easily update applications by uploading new code files or requesting a pull from a repository. 
	 
 - Supported Platforms:
     - Docker
     - Java
     - Windows .NET
     - Node.js
     - PHP
     - Python
     - Ruby 

## Q: How is AWS CloudFormation different from AWS Elastic Beanstalk?

These services are designed to complement each other. ***AWS Elastic Beanstalk*** provides an environment to easily deploy and run applications in the cloud. It is integrated with developer tools and provides a one-stop experience for you to manage the lifecycle of your applications. ***AWS CloudFormation*** is a convenient provisioning mechanism for a broad range of AWS resources. It supports the infrastructure needs of many different types of applications such as existing enterprise applications, legacy applications, applications built using a variety of AWS resources and container-based solutions (including those built using AWS Elastic Beanstalk).

AWS CloudFormation supports Elastic Beanstalk application environments as one of the AWS resource types. This allows you, for example, to create and manage an AWS Elastic Beanstalk–hosted application along with an RDS database to store the application data. In addition to RDS instances, any other supported AWS resource can be added to the group as well.