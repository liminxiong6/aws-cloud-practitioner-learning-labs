## Route 53 - Registering a domain
- Route 53 -> Registered domains -> Register domains -> Check availability: example.com -> Seleceted -> Proceed to checkout (Auto-renew)

## Route 53 - Creating our first records
- Hosted zones -> Create record -> Record name: test.example.com, Record type: A, 11.22.33.44 (EC2 instance) -> Create
- CloudShell -> `sudo yum install -y bind-utils` -> `nslookup test.example.com`, responding address: 11.22.33.44 -> `dig test.example.com`, ANSWER SECTION: 269(TTL) type A 11.22.33.44

## Route 53 - EC2 Setup
- EC2 -> us-east-1，Launch instance -> Select Amazon Linux 2, t2.micro, without key pair, create security group (allow ssh and http), advanced user data
  ```
  #!/bin/bash
  yum update -y
  yum install -y httpd
  systemctl start httpd
  systemctl enable httpd
  # updated script to make it work with Amazon Linux 2023
  CHECK_IMDSV1_ENABLED=$(curl -s -o /dev/null -w "%{http_code}" http://169.254.169.254/latest/meta-data/)
  if [[ "$CHECK_IMDSV1_ENABLED" -eq 200 ]]
  then
      EC2_AVAIL_ZONE="$(curl -s http://169.254.169.254/latest/meta-data/placement/availability-zone)"
  else
      EC2_AVAIL_ZONE="$(TOKEN=`curl -s -X PUT "http://169.254.169.254/latest/api/token" -H "X-aws-ec2-metadata-token-ttl-seconds: 21600"` && curl -s -H "X-aws-ec2-metadata-token: $TOKEN" http://169.254.169.254/latest/meta-data/placement/availability-zone)"
  fi
  echo "<h1>Hello world from $(hostname -f) in AZ $EC2_AVAIL_ZONE </h1>" > /var/www/html/index.html
  ```
- EC2 -> Singapore, Launch instance -> Select Amazon Linux 2, t2.micro, without key pair, create security group (allow ssh and http), advanced user data
- Launch another instance with region in Frankfurt
- Region Frankfurt -> Load Balancers -> ALB -> name: DemoRoute53ALB, Network Mappings: all AZs(eu-central-1a, eu-central-2b, eu-central-3c), Security group (created before, allow ssh, http), Listener HTTP:80 Forward to
- Create a new target group -> target type: Instances, name: demo-tg-route-53 -> Register 1 instance (same region as ALB)
- Back to load balancer -> select target group (demo-tg-route-53) -> Create load balancer
- Access the instance ipv4 address (3.70.14.253) to make sure it's working
- Acess the DNS of the ALB to make sure it's working

## Route 53 - TTL
- Hosted zones : example.com -> Records -> Create record -> record name: demo.example.com, Record type: A, Value: 3.70.14.253(eu-central-1), TTL: 120 seconds -> Create
- Access "demo.example.com"
- In CloudShell -> run `dig demo.example.com`, ANSWER SECTION: 98 A 3.79.14.253 -> In 98 seconds left, will not request DNS server, just access the ipv4 address in the client cache
- Edit the record: demo.example.com, value: 54.251.92.166 (ap-southeast-1) -> Save
- Even the record is updated, in cloudshell run `dig demo.example.com`, ANSWER SECTION: 66 A 3.79.14.253 -> In browser, still access to the eu-central-1 ipv4 address -> cuz the record got cached for 2 mins

## Route 53 - CNAME vs Alias
- Hosted zones -> records -> Create record -> record name: myapp.example.com, Record type: CNAME, Value: ALB DNS name
- Access myapp.example.com through browser (this way is not suitable for AWS native, better use Alias type)
- Hosted zones -> records -> Create record -> record name: myalias.example.com, Record type: A, Route traffic to: Alias to Application and Classic Load Balancer, Choose Region: eu-central-1, choose ALB, Create
- Alias record is free to query

## Routing Policy - Simple
- Hosted zones -> records -> Create record -> record name: simple.example.com, record type: A, value: 54.251.92.166 (ap-southeast-1) + 54.172.8.44(us-east-1), TTL: 20, Routing policy: Simple routing
- In CloudShell -> run `dig simple.example.com`, ANSWER SECTION: 20 A 54.251.92.166 20 A 54.172.8.44

## Routing Policy - Weighted
- Hosted zones -> records -> Create record -> record name: weighted.example.com, record type: A, value: 54.251.92.166 (ap-southeast-1), TTL: 3, Routing policy: Weighted, Weight: 10, Record ID: SOUTHEAST
- Add another record -> record name: weighted.example.com, record type: A, value: 54.172.8.44(us-east-1), TTL: 3, Routing policy: Weighted, Weight: 70, Record ID: US EAST
- Add another record -> record name: weighted.example.com, record type: A, value: 3.70.14.253(eu-central-1), TTL: 3, Routing policy: Weighted, Weight: 20, Record ID: EU -> Create Record
- Access "weighted.example.com" in the browser
- In CloudShell -> run `dig weighted.example.com`, ANSWER SECTION: 20 A 54.251.92.166

- ## Routing Policy - Latency
- Hosted zones -> records -> Create record -> record name: latency.example.com, record type: A, value: 54.251.92.166 (ap-southeast-1), TTL: 300, Routing policy: Latency, Region: ap-southeast-1, Record ID: ap-southeast-1
- Add another record -> record name: weighted.example.com, record type: A, value: 54.172.8.44(us-east-1), TTL: 300, Routing policy: Latency, Region: us-east-1, Record ID: us-east-1
- Add another record -> record name: weighted.example.com, record type: A, value: 3.70.14.253(eu-central-1), TTL: 300, Routing policy: Latency, Region: eu-central-1, Record ID: eu-central-1
- Access "weighted.example.com" in the browser, response "Hello word from eu-central-1" (if close to eu)
- In CloudShell -> run `dig latency.example.com`, ANSWER SECTION: 20 A 3.70.14.253
- if use VPN set to Canada, close to US, response "Hello word from us-east-1" 
