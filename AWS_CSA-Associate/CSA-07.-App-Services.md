# CloudWatch 
 - Used to monitor AWS services such as EC2.
 - It provides centralized logging and performance metrics for AWS resources.
 - For example, "CPU Utilization" of an EC2 instance, or network usage.
 - CloudWatch Alarms can be used as "triggers" in your AWS (i.e. to trigger an auto scaling event) 

# CloudTrail 
 - CloudTrail is an API logging service that logs all API calls made to AWS. It does not matter if the API calls originate from the CLI, SDK, or console. Logs can help address security concerns, and even allow you to log and view each action performed by a user on your AWS account. 

# Simple Notification Service (SNS)
 - SNS is an AWS service that allows you to automate the sending of notifications (i.e. email and text messages), based on events that occur in your AWS account.
 - SNS coordinates and manages the delivery of messages to specific endpoints.
 - SNS is integrated into many AWS services, so it is really easy to setup and use.
 - With CloudWatch and SNS, a full environment monitoring solution could be created that will notify administrators with alerts like: capacity issues, downtime, changes in the environment, and more! 
