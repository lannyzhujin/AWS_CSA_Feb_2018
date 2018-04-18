# IAM

![](https://github.com/lannyzhujin/AWS_CSA_Feb_2018/blob/master/AWS_CSA-Associate/img/IAM.PNG)

## Users

 - **non-explicit deny rule**
 - **Best Practice** is to login and do daily work as IAM user - NOT as the root user
 - **Principle of Least Privilege**
 - Unique access credentials, DO NOT share
 - Credentials should NEVER be stored on EC2 instance
 - MFA can be on a per user bases

## Groups

 - Assign IAM policies to 1+ user at a time.

## Roles

 - IAM user in another account
 - Application code running on an EC2 instance that needs to perform actions on AWS resources
 - An AWS service that needs to act on resources in your account to provide its features
 - Users from a corporate directory who use identity federation with SAML
 - An EC2 instance can only have ONE role attached at a time.
 - You can assign a role to EC2 when creation or change it after creation.

## Policies

   Document that formally states one or more permissions

 - **Explicit Deny** always overrides an **Explicit Allow**.

      A "Deny all" policy quickly restrict ALL access

 - Pre-built policy
    - Administrator access - FULL access to ALL services
    - Power user access - Admin access except user/group management
    - Read only access

 - Policy generator

 - 1+ policy can be attached to a user / group 

 - Policy CANNOT be directly attached to AWS resource

## API Keys

IAM API Keys: 
 - API Access Keys are required to make programmatic calls to AWS from the: 
   - AWS Command Line Interface (CLI)
   - Tools for Windows PowerShell
   - AWS SDKs
   - Direct HTTP calls using the APIs for individual AWS services. 

For Example:
  -  API credentials are user by developers, working from an on-premise network, for CLI access. 

Important API Access Key Facts:
 - API Keys are only available ONE time, when a new user is created OR when you reissue a new set of keys.
 - AWS will NOT regenerate the same set of access keys.
 - In order for API credentials to work, they must be assocaited with a USER.
 - Roles does not have API credentials.
 - In the AWS console you can only see the Access Key ID - never the sSecret Key ID
 - If you require new API credentials - you must deactivate the current set and generate new one
 - NEVER create or store API keys on an EC2 instance. 

## Specify a password policy as well as manage MFA requirements on a per user basis

## STS Security Token Service 
Overview  
 - **Short term**, **temporary** credentials
 - Valid for minutes ~ hours, then expire
 - When request via STS **API call**, credentials are returned with:
    - Security Token
    - An Access Key ID
    - A Secret Access Key

Benefits:
 - No distributing or embedding long-term AWS security credentials in App
 - Grant access to AWS resource without an IAM identity
 - Credentials are temp, no need to rotate or revoke 

When to use STS
 - Identity Federation:
     - Enterprise identity federation
           STS support SAML (Security Assertion Markup Language) which allow Microsoft Active Directory
     - Web identity federation (3rd party identity providers, i.e. Facebook, Google, Amazon)
 - Roles for Cross-Account Access
 - Role for Amazon EC2 (and other AWS services)

## Limitations
### Default limits for IAM entities:

Resource	| Default Limit 
------ | ------
Customer managed policies in an AWS account	| 1500
Groups in an AWS account	| 300
Roles in an AWS account	| 1000
Users in an AWS account	 | 5000 (If you need to add a large number of users, consider using temporary security credentials.)
Virtual MFA devices (assigned or unassigned) in an AWS account | 	Equal to the user quota for the account
Instance profiles in an AWS account	| 1000
Server certificates stored in an AWS account |	20

### Limits for IAM entities:

Resource	 | Limit
------ | ------
Access keys assigned to an IAM user	| 2
Access keys assigned to the AWS account root user	| 2
Aliases for an AWS account	| 1
Groups an IAM user can be a member of	| 10
IAM users in a group	| Equal to the user quota for the account
Identity providers (IdPs) associated with an IAM SAML provider object	| 10
Keys per SAML provider	| 10
Login profiles for an IAM user	| 1
Managed policies attached to an IAM group	| 10
Managed policies attached to an IAM role	| 10
Managed policies attached to an IAM user	| 10
MFA devices in use by an IAM user	| 1
MFA devices in use by the AWS account root user	| 1
Roles in an instance profile	| 1
SAML providers in an AWS account	| 100
Signing certificates assigned to an IAM user| 	2
SSH public keys assigned to an IAM user	| 5
Versions of a managed policy that can be stored	| 5

### Key Management Service
 - AWS KMS currently supports only symmetric (private) key cryptography.

[[Home]](https://github.com/lannyzhujin/AWS_CSA_Feb_2018/blob/master/AWS_CSA-Associate/Home.md)
