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

##  AWS CLI profile (Manage multiple AWS Accounts)
- `aws configure --profile my-other-aws-account` now the AWS Access Key ID is none, then we configure another profile
- `cat credentials` we have 2 credentials (default + my-other-aws-account)
- `aws s3 ls` execute this function against the default profile, to execute against other profile `aws s3 ls --profile my-other-aws-account`

## AWS CLI with MFA
- IAM -> Users -> my-user -> Security credentials tab -> Manage Assigned MFA device -> Type: Virtual MFA device -> Scan QR code -> have a arn-...
- in Console -> `aws sts get-session-token help`
- `aws sts set-session-token --serial-number arn:aws:iam:.... --token-code 82313`  -> Get access key ID, Secret Acess Key
- `aws configure --profile mfa` -> copy paste the access key ID, Secret Acess Key
- every time do an API call will use the temporary aws-session-token
- `aws s3 ls --profile mfa`

## AWS Signature v4 Signing (Sigv4)
- S3 -> bucket -> coffee.jpg -> Open button -> Copy the URL -> Query String has X-AMz-Algo..., X-Ams-Signature, ...

