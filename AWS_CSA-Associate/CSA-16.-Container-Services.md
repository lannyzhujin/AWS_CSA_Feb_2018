# Container Services
![](https://github.com/lannyzhujin/AWS_CSA_Feb_2018/blob/master/AWS_CSA-Associate/img/ECS.PNG)
## EC2 Container Service (ECS) Essentials: 
 - ECS is a container management service that supports Docker.
 - It allows you to easily create and manage a fleet of Docker containers on a cluster of EC2 instances. 

 Why use ECS/Containers?
 - Create distributed applications and Microservices:
     - Create application architecture comprised of independent tasks or processes (micro services).
	 - For example, you can have separate containers for various components of your application
    	 - Webserver
		 - Application server
		 - Message queue
		 - Backend Servers
	 - This allows you to start, stop, manage, monitor, and scale each container independently. 

- Batch and ETL Jobs:
     - Package batch and ETL jobs into containers and deploy them into an shared EC2 cluster(s).
	 - Run different versions of the same job or multiple jobs on the same cluster.
	 - Share cluster capacity with other processes and or grow job dynamically on-demand to improve resource utilization.

 - Continuous Integration and Deployment:
     - By using Dockers Image versioning, you can use containers for continuous integration and deployment.
	 - Build processes can pull, build, and create a Docker Image that can be deployed into your containers.
	 - This allows you to avoid an application from working in a developer environment and not working in a production environment because the Docker daemon is the same across all environments. 

### ECS Agent: 
 - The ECS Agent runs on each EC2 instance in the ECS cluster.
 - It communicates information about the instances to ECS, including:
     - Running tasks
	 - Resource Utilization
 - The ECS Agent is also responsible for starting/stopping tasks (when told to by ECS). 

### ECS Task: 
 - An ECS Task is the actual representation of the Task Definition on an EC2 instance inside of your container cluster.
 - The ECS Agent will start/stop these tasks based on instruction/schedule. 

### ECS Task Definition: 
 - A JSON formatted text file that contains the "blueprint" for your application, including:
     - Which container/docker image to use.
	 - The repository (container registry) the image is located in.
	 - Which ports should be open on the container instance.
	 - What data volumes should be used with the containers. 

[[Home]](https://github.com/lannyzhujin/AWS_CSA_Feb_2018/blob/master/AWS_CSA-Associate/Home.md)