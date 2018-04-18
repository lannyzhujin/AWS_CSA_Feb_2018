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

# OpsWorks
AWS OpsWorks is a configuration management service that helps you build and operate highly dynamic applications, and propagate changes instantly.

## OpsWorks Stacks
Define, group, provision, deploy, and operate your applications in AWS by using Chef in local mode.

## OpsWorks for Chef Automate
Create Chef servers that include Chef Automate premium features, and use the Chef DK or any Chef tooling to manage them.

## OpsWorks for Puppet Enterprise
Create Puppet servers that include Puppet Enterprise features. Inspect, deliver, update, monitor, and secure your infrastructure.

aws has a host of connected resources that can be created, managed, backed up, terminated and restored. you can do that manually, via the console, api or cli tools. you can also leverage additional tools to automate much (or all) of that process. cloudformation, opsworks and elastic beanstalk are three of those such tools.

essentially, they fill similar roles: perform actions on specific connected resources within an account or accounts. the high level difference is that their functions offer various levels of abstration above the AWS platform.

 - Elastic Beanstalk is the high level offering. it is the simplest way to deploy an application on aws. if you're looking for a no-frills, automagic, as-fully-managed-as-you-can-get-in-aws experience, this is it.

 - OpsWorks is the middle tier. operating as a full featured orchestration tool (thus the tight relationship with chef), opsworks combines straightforward deployment, configuration and management with the flexibility to handle complex implementations.

 - Cloudformation is the nuts and bolts, low level utility. when you want granular control over everything in your environment, cfn is the choice. cfn can handle pretty much anything - from tiny footprint, one instance web server deployments to netflix - with a templatized, code driven approach. if you're doing serious work with aws, you're probably using cloudformation.

it's worth noting that these services can be combined. a useful set up would be to use cfn to establish the networking topology of your aws presence and opsworks to handle specific application deployment processes within the created network space.

you can find more discussion about deployment methods here: https://d0.awsstatic.com/whitepapers/overview-of-deployment-options-on-aws.pdf 

[[1]](https://acloud.guru/forums/aws-certified-solutions-architect-professional/discussion/-K8uBbNMc2Jcc-mHRYLp/cloudformation-v-opsworks-v-elastic-beanstalk-use-cases)

## Q: How is AWS CloudFormation different from AWS Elastic Beanstalk?

These services are designed to complement each other. ***AWS Elastic Beanstalk*** provides an environment to easily deploy and run applications in the cloud. It is integrated with developer tools and provides a one-stop experience for you to manage the lifecycle of your applications. ***AWS CloudFormation*** is a convenient provisioning mechanism for a broad range of AWS resources. It supports the infrastructure needs of many different types of applications such as existing enterprise applications, legacy applications, applications built using a variety of AWS resources and container-based solutions (including those built using AWS Elastic Beanstalk).

AWS CloudFormation supports Elastic Beanstalk application environments as one of the AWS resource types. This allows you, for example, to create and manage an AWS Elastic Beanstalk–hosted application along with an RDS database to store the application data. In addition to RDS instances, any other supported AWS resource can be added to the group as well.

[[Home]](https://github.com/lannyzhujin/AWS_CSA_Feb_2018/blob/master/AWS_CSA-Associate/Home.md)