## Creating ECS Cluster
- Amazon ECS -> Create cluster -> name: DemoCluster, Infrastructure: [method of obtaining compute capacity: Fargate and Managed Instances, Instance profile: [create new -> AWS service, select "EC2 Role for ECS Managed Instances" -> use this newly created role], Infrastructure role: [create new -> AWS service, select "Infrastructure for ECS Managed Instances" -> use this newly created role], Instance selection: [Use custom, Add instance attribute -> Allowed instance types: t3.micro]]
- Changing "method of obtaining compute capacity" in Infrastructure, choosing "Fargate and Self-managed instances" -> Create a new Auto Scaling group, On-demand instances, EC2 instance type: t3.micro, EC2 instance role: Create default role, Min Desired Capacity: 0, Max Desired Capacity: 2, Root EBS volume size: 30 -> Create
- EC2 console -> Auto Scaling groups -> Infra-ECS-Cluster created
- Amazon ECS -> DemoCluster -> Infrastructure tab -> Capacity provider: [FARGATE (launch Fargate task onto our ECS cluster), FARGATE_SPOT (can luanch Fargate task in Spot mode, select Spot instances for EC2), Infra-ECS-Cluster (ASG Provider, Launch EC2 instances in this cluster directly through this ASG)] -> The current size of Infra-ECS-Cluster is 0
- To change that size -> go to the ASG -> Click Infra-ECS-Cluster ASG -> Edit Details: [Desired capacity: 1] -> Infra-ECS-Cluster ASG -> Instance management -> an EC2 instance is created -> This EC2 instance will register itself into the DemoCluster -> Will see in the Container instances of the DemoCluster
- Copy the ALB DNS and access it -> NGINX welcome page
- So based on the Capaciaty provider, when we create a ECS task, it can either be launched on a Fargate, FARGATE_SPOT, or Container instances(launched as part of the ASG)

## Creating ECS Service
- Amazon ECS -> Task definitions -> Create new task definition -> Task definition family: nginxdemos-hello(coming from "niginxdemos/hello" image on docker hub) -> Infrastructure requirements: [Launch type: enable AWS Fargate, OS: Linux/x86_64, Task size: [.5 vCPU, 1GB Memory]] -> Container - 1: [Name: nginxdemos-hello, Image URI: nginxdemos/hello(pull image from docker hub)] -> Storage: 21 GiB -> Create
- Amazon ECS -> Clusters -> DemoCluster -> Services -> Create -> Task definition family: nginxdemos-hello, Service name: nginxdemos-hello-service -> Environment: [Compute configuration: Capacity provider strategy, Capacity provider: FARGATE] -> Deployment configuration: [Replica, Desired tasks: 1, Turn on AZ rebalancing] -> Networking: [Create a new security group (Allow HTTP traffic from anywhere), turn on Public IP] -> Load balancing: [Use load balancing, type: ALB, Containter: nginxdemos-hello 80:80, Create a new ALB, name: DemoALBForECS, Listener: HTTP 80, Target groups: [name: nginxdemos-tg, HTTP 80] -> Create
- DemoCluster -> Services -> nginxdemos-hello -> Health and metrics -> 1 Desired Task, 1 running -> is linked to a target group (nginxdemos-tg) -> target group is linked to DemoALBForECS, and one IP address(container) is registered as a target
- DemoCluster -> Services -> nginxdemos-hello -> Tasks -> 1 container is running (1 task) -> Click on it
- DemoCluster -> Services -> nginxdemos-hello -> Events -> 1 task started, the task is registered in the target group
- To launch more tasks in the service: Update the service -> Desired tasks: 3 -> Update
- DemoCluster -> Services -> nginxdemos-hello -> Tasks -> 3 tasks (3 containers) -> ALB will distribute the load between all containers (refresh ALB DNS web page, ip address keeps changing)
- Cleanup: upddate the service -> Desired tasks: 0 (service still there, no containers running on ECS) -> EC2: ASG -> make sure Infra-ECS-Cluster asg desired capacity is 0 (no EC2 instance running on ECS)

## Amazon ECS Task Definition
- Amazon ECS -> Create new task definition -> Task definition family: wordpress -> Infrastructure requirements: [Launch type: AWS Fargate (Network mode only awsvpc) & Amazon EC2 instances] -> Container - 1 -> [Name: word press, Image URI: wordpress, Essential container: Yes (if the container fails and is killed, all the tasks is going to be killed), Environment variables: [Kay: SECRET_DB_PASSWORD, Value type: ValueFROM, Value: ARN of the secrets manager secret], or Add Environment variables from the file -> Storage -> Volume type: Bind mount, (Container mount points: [Container: wordpress(to mount the volume on), Volumes from]) -> Monitoring -> Enable Trace collection & Metric collection -> Create -> Create new revision to modify

## Clean up
- Amazon ECS -> Clusters -> DemoCluster -> Services -> nginxdemo-hello -> Update service -> Desired tasks: 0 -> Delete Service
- CloudFormation -> Stacks -> ECS-Console-VS-Service-nginxdemos-hello DELETE_IN_PROGRESS -> Resources (ECS Service, Listener, LoadBalancer, SecurityGroup, TargetGroup) -> After all these deleted -> DemoCluster -> Delete Service -> CloudFormation -> Stacks -> Infra-ECS-Cluster-DemoCluster (Resources: EC2 CapacityProvider)
- Task definitions (don't cost money, can leave there) -> Actions -> Deregister

## Amazon ECR 
- Amazon ECS -> Task definitions -> nginxdemo-hello:1 -> Containers -> nginxdemos/hello Image (from docker hub)
- To host that image onto our private ECR repository: Amazon ECR -> Repositories -> Create -> Private, name: demomyname
- demomyname -> Push commands (have docker installed on PC `docker --version`) -> login `aws ecr get-login-password --region eu-west-1 | docker login --username AWS ....`
- pull the image `docker pull nginxdemos/hello` -> rename the image `docker tag nginxdemos/hello:latest 787....dkr.ecr.eu-west-1.amazonaws.com/demomyname:latest` -> push to amazon ecr repository `docker push 787....dkr.ecr.eu-west-1.amazonaws.com/demomyname:latest` -> latest image created for image named demomyname
- setting task definition with this image(to pull from Amazon ECR instead of from docker hub)

## AWS CoPilot
- https://aws.github.io/copilot-cli/docs/overview/
- `brew install aws/tap/copilot-cli`
- `copilot --help`
- aws cli and docker desktop installed `aws --version`, `docker --version`
- `git clone https://github.com/aws-samples/aws-copilot-sample-service example` -> `cd example` -> `copilot init` -> Application name: example-app, choose load balanced web service, service name: front-end, ./Dockerfile, Environment's name: test
- "Your service is accessible at http://..."
- cleanup: `copilot app delete`
