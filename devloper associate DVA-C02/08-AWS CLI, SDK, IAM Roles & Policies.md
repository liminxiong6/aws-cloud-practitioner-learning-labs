## AWS EC2 Instance Metadata
- EC2 -> Luanch an instance -> Name: DemoEC2, Select Amazon Linux 2023 t2.micro, Proceed without key pair, Create security group: Allow SSH from Anywhere -> Launch
- Connect EC2 instance using EC2 instance Connect  
- Google search imds v1 -> Instance metadata -> Access instance metadata -> copy  `curl http://169.254.169.254/latest/meta-data/` and run in the command -> Failed, cuz Amazon Linux 2023 only IMDSV2, but works with Aamazon Linux 2
- so use IMDSv2, generate a token first:
  ```
  TOKEN=`curl -X PUT "http://169.254.169.254/latest/api/token" -H "X-aws-ec2-metadata-token-ttl-seconds: 21600"`
  ```
- check the generated token:
  ```
  echo $TOKEN
  ```
- then use the token to generate top-level metadata items:
  ```
  curl -H "X-aws-ec2-metadata-token: $TOKEN" http://169.254.169.254/latest/meta-data/
  ```
- To get the hostname of the instance
  ```
  curl -H "X-aws-ec2-metadata-token: $TOKEN" http://169.254.169.254/latest/meta-data/hostname
  ```
  To get local ipv4
  ```
  curl -H "X-aws-ec2-metadata-token: $TOKEN" http://169.254.169.254/latest/meta-data/hostname
  ```
- To get identity credentials
  ```
  curl -H "X-aws-ec2-metadata-token: $TOKEN" http://169.254.169.254/latest/meta-data/identity-credentials/ec2/info
  ```
  To get security credentials of ec2, 404 not found cuz we don't have IAM role attached to our instances
  ```
  curl -H "X-aws-ec2-metadata-token: $TOKEN" http://169.254.169.254/latest/meta-data/identity-credentials/ec2/security-credentials
  ```
- EC2 Console -> Click on the instance -> Actions -> Security -> Modify IAM role -> Choose rny role(AmazonEC2RoleForSSM), then run
  ```
  curl -H "X-aws-ec2-metadata-token: $TOKEN" http://169.254.169.254/latest/meta-data/identity-credentials/ec2/security-credentials
  ```
  ```
  curl -H "X-aws-ec2-metadata-token: $TOKEN" http://169.254.169.254/latest/meta-data/identity-credentials/ec2/security-credentials/ec2-instance
  ```
- Cleanup: Terminate EC2 instance
