# SNS
## SNS Essential
![](https://github.com/lannyzhujin/AWS_CSA_Feb_2018/blob/master/AWS_CSA-Associate/img/SNS.PNG)

 - SNS coordinates and manages the sending and delivery of messages to specific endpoints.
 - We are able to use SNS to receive notifications when events occur in our AWS Environment.
 - SNS is integrated into many AWS services, so it is very easy to setup notifications based on events that occur in those services.
 - With CloudWatch and SNS, a full-environment monitoring solution can be created that notifies administrators of alerts, capacity issues, downtime, changes in the environment, and more!
 - This service can also be used for publishing iOS/Android app notifications, and creating automation based off of notifications. 

SNS Components:

Publisher -> Topic -> Subscription

 - Topic:
     - The group of subscriptions that you send a message to.
 - Subscription:
     - An endpoint that a message is sent.
     - Available endpoints include:
         - HTTP
         - FITTPS
         - Email
         - Email-JSON
         - SQS
         - Application, Mobile APP notifications (I0S/Android/Amazon/Microsoft)
         - Lambda
         - SMS (cellular text message)
 - Publisher:
     - The "entity" that triggers the sending of a message
     - Examples include:
         - Human
         - S3 Event
         - Cloudwatch Alarm 

# SQS
![](https://github.com/lannyzhujin/AWS_CSA_Feb_2018/blob/master/AWS_CSA-Associate/img/SQS.PNG)
## SQS Essentials: 
 - SQS provides the ability to have hosted/highly available queues that can be be used for messages being sent between servers.
 - This allows for the creation of **distributed/decoupled** application components.
 - **SQS is used to create decoupled application environments.**
 - Messages between servers are retrieved through **polling**. 
 
### Two types of polling
 - Long Polling (1-20 seconds):
     - Allows the SQS service to wait until a message is available in a queue before sending a response, and will return all messages from all SQS services.
	 - Long polling reduces API requests (over using short polling).
 - Short Polling:
     - SQS samples a subset of servers and returns messages from just those servers.
	 - Will not return all possible messages in a poll.
	 - Increases API requests (over long polling), which increases costs. 

### Other important SQS facts
 - Each message can contain up to 256KB of text (in any format).
 - Amazon SQS offer two different types of queues:
     - **Standard Queue**: Guarantees delivery of each message at least once BUT DOES NOT guarantee the order (best effort) in which they are delivered to the queue.
	 - **First-in-first-out (FIFO) Queue**: Designed for applications where the order of operations and events is critical, or where duplicates can't be tolerated.
 - SQS is also highly available and redundant. 
 
### SQS Workflow
 - Generally a "worker" instance will "poll" a queue to retrieve waiting messages for processing.
 - Auto Scaling can be applied based off of queue size so that if a component of your application has an increase in demand, the number of worker instances can increase. 

### SQS Message:  
 - A set of instructions that will be relayed to the "worker" instances via the SNS Queue.
 - Can be up to 256KB of text (in any format).
 - Each message is guaranteed to be delivered at least once:
     - Order is not guaranteed
     - Duplicates can occur 
### SOS Oueue:  
 - An queue stores messages (for up to 14 days), that can be retrieved through "polling".
 - Queues allows components of your application to work independently of each other (decoupled environments). 

### Decoupled Architecture:  
- Tightly Coupled System:
   - A system architecture of components that are not just linked together but are also dependent upon each other.
   - If one component fails all components fail. 
- Loosely Coupled/Decoupled Systems:
   - Multiple components that can process information without being connected.
   - Components are not connected - if one fails the rest of the system can continue processing (fault tolerant/highly available). 
- AWS Services that are used for distributed/decoupled system architectures:
   - SWF (Simple Work Flow Service)
   - SQS (Simple Queue Service) 
![](https://github.com/lannyzhujin/AWS_CSA_Feb_2018/blob/master/AWS_CSA-Associate/img/DecoupledArch.PNG)

# SWF
![](https://github.com/lannyzhujin/AWS_CSA_Feb_2018/blob/master/AWS_CSA-Associate/img/SWF.PNG)
## Simple Workflow Essentials:  
 - SWF is a fully-managed "work flow" service provided by AWS.
 - A SWF **workflow** allows an architect/developer to implement distributed, asynchronous applications as a work flow.
 - A **workflow** coordinates and manages the execution of activities that can be run asynchronously across multiple computing devices. 
 - SWF has consistent execution.
 - Guarantees the order in which tasks are executed.
 - There are no duplicate tasks. 
 - The SWF service is primarily an API which an application can integrate it's workflow service into. This allows the service to be used by non-AWS services, such as an on-premise data center.
 - A workflow execution can last up to 1 year.
 
### Components of SWF
 - **Workflow**: A sequence of steps required to perform a specific task. 
     - A workflow is also commonly referred to as a **decider**.
 - **Activities**: A single step (or unit of work) in the workflow.
 - **Tasks**: What interacts with the "workers" that are part of a workflow.
     - **Activity task** — Tells the worker to perform a function.
     - **Decision task** — Tells the decider the state of the workflow execution, which allows the decider to determine the next activity to be performed.
 - **Worker**: Responsible for receiving a task and taking action on it.
     - Can be any type of component such as an **EC2 instance**, or even a person**. 

# API Gateway
![](https://github.com/lannyzhujin/AWS_CSA_Feb_2018/blob/master/AWS_CSA-Associate/img/APIGateWay.PNG)

## API Gateway Essentials: 
 - API Gateway is a fully-managed service that allows you to create and manage your own APIs for your application.
 - API Gateway acts as a "front door" for you application, allowing access to data/logic/functionality from your back-end services. 
### API Gateway Main Features: 
 - Build RESTful APIs with:
     - Resources
     - Methods (i.e. GET, POST, PUT) 
     - Settings
 - Deploy APIs to a "Stage" (different envs: i.e. dev, beta, production)
     - Each stage can have it's own throttling, caching metering, and logging.
 - Create a new API version by cloning an existing one.
     - You can create and work on multiple versions of an API (API version control).
 - Roll back to previous API deployments
     - A history of API deployments are kept.
 - Custom domain names
     - Custom domain names can point to an API or Stage.
 - Create and manage API keys for access AND meter usage of the API keys through Amazon CloudWatch Logs
 - Set throttling rules based on the number of request per second (for each HTTP method)
     - Request over the limit throttled (HTTP 429 response)
 - Security using Signature v.4 to sign and authorize API calls
     - Temporary credentials generated through Amazon Cognito and Security Token Service (STS) 

### Benefits of API Gateway:
 - Ability to cache API responses
 - DDoS protection via Cloud Front
 - SDK generation for iOS, Android, and JavaScript
 - Supports Swagger (a very popular framework of API dev tools)
 - Request/response data transformation (i.e. JSON IN to XML OUT) 

### API Gateway: CloudFront 
- API Gateway benefits from using CloudFront infrastructure:
    - Built in Distributed Denial of Service (DDoS) attack protection and mitigation.
    - All CloudFront Edge Locations become entry points for your API into your back-end.
- Summary: Benefits are reduced latency and improved projection. 

### API Gateway: CloudWatch  
- CloudWatch can be used to monitor API Gateway activity and usage.
- Monitoring can be done on the API or Stage level. 
- Throttling rules are monitored by CloudWatch 
- Monitoring metrics include such statistics as: 
    - Caching
    - Latency
    - Detected errors
- Method-level metrics can be monitored. 
- You can create CloudWatch alarms based on these metrics. 

