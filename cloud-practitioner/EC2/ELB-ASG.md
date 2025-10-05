# ELB(Elastic Load Balancing) & ASG(Auto Scaling Groups) Lab

**Steps:**
ALB (Application Load Balancer)
1. create 2 instances -> user data
   <img width="431" height="137" alt="image" src="https://github.com/user-attachments/assets/cd3c3b53-a6df-4a5f-b76f-200bc848640c" />
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
2. Load Balancer -> Create load balancer (ALB) -> Network mapping: Select all AZs -> Security groups: create a new security group
   <img width="1110" height="580" alt="image" src="https://github.com/user-attachments/assets/fd9ea045-2ccc-40ee-aa6c-98eb35db05f0" />
3. Remove default security group and choose the new created one
4. Listener and routing -> Forward to target groups -> create target groups: register targets (2 instances) -> include as pending below -> created and use this target group
   <img width="682" height="459" alt="image" src="https://github.com/user-attachments/assets/682cebb7-620b-4ee8-95ef-d2e58ec8a080" />
5. Create load balancer -> use the DNS name of ALB to access the website



ASG
1. Auto Scaling Groups -> create auto scaling group -> create a launch template (tell ASG how to create EC2 instances): Quickstart Amazon Linux, security group, user data... -> select the template created
2. Next -> Network: select different AZs -> Load balancing: Attach to an existing load balancer -> Choose existing load balancer target groups (Attach newly created instances to this target group) -> Health checks: Enable Turn on Elastic Load Balancing health checks 
   <img width="797" height="421" alt="image" src="https://github.com/user-attachments/assets/ba9675d7-adc2-4d42-bde1-1d8377feb261" />
3. Next -> Group size configure -> Create ASG
   <img width="601" height="436" alt="image" src="https://github.com/user-attachments/assets/c8ccd0f9-c257-45e1-a195-d52366e25b54" />
4. check the newly create ASG Activity and Instance management: have 2 new instances created
5. check target group Details: have 2 targets
6. target groups -> Edit Health checks
   <img width="619" height="406" alt="image" src="https://github.com/user-attachments/assets/1417f24d-21c0-4c05-a17f-fe440945e2a1" />



Cleanup
1. Auto Scaling groups -> select and delete ASG
2. Load Balancers -> select and delete ALB
3. target groups no need to delete (free): it will be empty

