## Application Load Balancer(ALB, layer 7)
- Launch 2 instances: Name-My First Instance, Amazon Linux 2, t2.micro, without key pair, existing sg(allows HTTP and SSH traffic in), 8 GiB gp2, add user data
  ```sh
    #!/bin/bash
  # Use this for your user data (script from top to bottom)
  # install httpd (Linux 2 version)
  yum update -y
  yum install -y httpd
  systemctl start httpd
  systemctl enable httpd
  echo "<h1>Hello World from $(hostname -f)</h1>" > /var/www/html/index.html
  ```
- Create load balencer -> Select ALB -> name: DemoALB, Network mapping(which AZ to deploy load balancer): select all AZs, security groups: create new(name: demo-sg-load-balancer, description: Allow HTTP into ALB), Listeners and routing(HTTP:80): create target group
- Create target group -> target type: Instances, name: demo-tg-alb, protocol version: HTTP1 -> Register targets: select 2 instances on port 80 -> Include as pending below -> Create target
- load balencer creation -> select newly created target group(demo-tg-alb) -> Create
- Select the load balencer's DNS, access through it, keep refreshing -> redirecting between 2 instance
- If stop one instance -> in target group, the instance's health is "unused"
- Check the instances' security group -> In HTTP type inbound rules, change it to only allow traffic from load balancer -> Not allow to access the instance directly but only through DNS
- Modify ALB Listener rules -> Go to DemoALB -> Listeners: HTTP:80 -> Listener rules: Default -> Add rule -> Name: DemoRule -> Add condition -> Path is /error -> Action types: Return fixed response, Response code: 404, Content type: text/plain, Response body: Not found, custom error!
- Access URL(ALB's DNS + /error)

## Network Load Balancer(layer 4)
-  Create load balencer -> Select NLB -> name: DemoNLB, N Network mapping: select all AZs (each AZ has a fixed ipv4 address assigned by AWS)
-  Create new security group -> name: demo-sg-nlb, description: Demo SG for NLB, Inbound rules: Allow HTTP from anywhere (0.0.0.0/0)
-  load balencer creation -> select newly created target group(demo-sg-nlb) -> Create
-  Listeners and routing -> Listener TCP:80 -> Create target group -> target type: Instances, name: demo-tg-nlb, protocol: [TCP:80 -> Health check: protocol:HTTP, path:/ ], Advanced health check settings: [Healthy threshold: 2, Timeout: 2 seconds, Interval: 5 seconds] -> Register targets: select 2 instances on port 80 -> Include as pending below -> Create target
-  load balencer creation -> select newly created target group(demo-tg-nlb) -> Create
-  Failed to access the NLB URL -> Why? The instances are unhealthy, their security groups inbound rules only allow SSH and HTTP traffic from the ALB
-  To fix: Add inbound rule in EC2 instancces's security group to allow HTTP traffic from NLB (can add inbound rule's description: Allow traffic from NLB)
-  Try to access the NLB URL again -> Succeed
-  Cleanup: Delete NLB & demo-tg-nlb(optional to delete target group & nlb's security group)

## Elastic Load Balancer - Sticky Sessions
- Create ALB with a Target group -> Select the target group -> Actions: Edit attributes -> Target selection configuration: Turn on stickiness, Stickiness type: Load balancer generated cookie (if choosing Application-based cookie, need to specify the cookie name) -> Save changes
- If access the ALB URL and keep refreshing the page, will show the access is to the same instance
- Check the Get request -> Cookies: Response Cookies AWSALB (expires tomorrow)
- Can go to the target group and disable the stickiness back

## Elastic Load Balancer - Cross Zone Load Balancing
- Have a NLB -> Attributes (Cross-zone load balancing is off by default) -> Edit -> Can enable this but will charge
- Have a ALB -> Attributes (Cross-zone load balancing is on by default, is always on in ALB!) -> Go to target group -> Attributes -> Edit: Turn Off the cross-zone load balancing

## Elastic Load Balancer - SSL Certificates
- Have a ALB(DemoALB) -> Add one listener -> Protocol: HTTPS, Port: 443, Default actions: Forward， Forward to: demo-tg-alb, Security policy: leave it as default, Default SSL/TLS certificate: Import To ACM
- Have a NLB(DemoNLB) -> Add one listener -> Protocol: TLS, Port: 443, Default actions: Forward， Forward to: demo-tg-nlb, Security policy: leave it as default, Default SSL/TLS certificate: Import To ACM

## Auto Scaling Groups
- Go to EC2 - Auto Sacling Groups -> Create -> name: DemoASG -> create a launch template -> name: MyDemoTemplate, description: Template, AMI:  Amazon Linux 2 + t2.micro, Key pair: EC2 Tutorial, security group: existing launch-wizard-1, EBS Root volume: 8Gib gp2, add user data -> Create this template and select it in ASG creation -> Next
- Choose instance launch options ->  Network settings: VPC use default and select all AZs to launch instances, Availability Zone distribution-Balanced best effort -> Next
- Integrate with other services -> Load balancing-Attach to an existing load balancer(target group: demo-to-alb) -> Health checks: Turn on ELB heath checks -> Next
- Configure group size and scaling: desired-1, min-1, max-1, automatic scaling-no scaling policies, instance maintenance policy: no policy, Additional capacity settings-default -> Next
- Create ASG
- Click on ASG created -> Activity tab -> Activity history: launching a new EC2 instance
- Instance management tab -> new instance running
- Go to target groups -> new instance is registered in to that alb
- Edit ASG group details: desired-2, min-1, max-2 -> Activity tab -> Activity history: launching a new EC2 instance -> Go to target groups -> new instance is registered

## Auto Scaling Groups - Scaling Policies Hands On
- Go to DemoASG -> Automatic scaling tab
- Scheduled actions -> Create -> Specific start time/date
- Predictive scaling policies (more ML driven) -> Create -> Turn on scaling (scale based on forecast)
- Dynamic scaling policies -> type: Simple scaling, Take the action: add 10 precent of group -> change type to Step scaling -> change type to Target tracking scaling (Average CPU utilization 40)
- Edit group details -> max capacity: 3
- Go to Monitoring tab -> EC2 section
- Go to EC2 instance to skyrocket the CPU utilization to 100% -> use EC2 instance connect to connect that instance -> Google search "install stress amazon linux 2" -> Find the command `sudo yum install stress -y` -> `stress -c 4`
- Go to Monitoring tab -> EC2 section -> High CPU utilzation
- Go to Activity tab -> Check the scaling action
- Go to instance management tab -> Have 2 instances
- Go to CloudWatch -> Alarms (2): AlarmLow(scale in) + AlarmHigh(scale out)
- Try stop that stress commond -> trigger AlarmLow -> ASG scales in

## Auto Scaling - 
