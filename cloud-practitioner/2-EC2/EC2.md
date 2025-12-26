# EC2 (Elastic Compute Cloud) Lab

**Steps:**
EC2 instance
1. Go to EC2 → Instances -> Launch instances -> Amazon Linux 2 -> Create new key pair (.pem) -> Create security group (Enable Allow HTTP traffic) -> Advanced Details: User data
2. Use pulic IP address to load the website
3. Instance State -> Stop instance (Don't want to use)
4. Instance State -> Start instance (use again)

Security Groups
! Important to maintain one seperate security group for SSH access
- Security Groups (2 groups: defaut + created one) -> Inbound rules

SSH 
- MAC: `chmod 0400 EC2Tutorial.pem` to not accessible by other users, then `ssh -i EC2Tutorial.pem ec2-user@2.252.4.236`, `exit` or Ctrl + D to close connection
- Windows: Putty Key Generator -> Select EC2Tutorial.pem -> Save private key (.ppk) -> Putty —> Save -> SSH -> Auth -> Credentials: Upload Private key (.ppk) -> Sessions -> Save
  <img width="388" height="241" alt="image" src="https://github.com/user-attachments/assets/3a659b15-9a78-4834-96a4-551c107123bc" />
  <img width="550" height="378" alt="image" src="https://github.com/user-attachments/assets/d0aa08b3-681b-41be-bc46-eff4b735953a" />
- Windows 10: pem file -> Properties -> Security -> Advanced -> Owner -> Change -> Enter object name to select: (PC user name) -> Remove all inhertied permissions -> Add user name -> Check name -> Give full control permissions -> Powershell `ssh -i .\EC2Tutorial.pem ec2-user@2.252.4.236`
  <img width="487" height="168" alt="image" src="https://github.com/user-attachments/assets/0211e0e6-6716-4cf6-a759-ac72e3858fb9" />
- EC2 instance connect (Browser based): select instance -> Connect

EC2 Instance Roles
- Option 1 (not recommended: credentials leak): EC2 instance connect -> aws configure (AWS Access Key)
- Option 2 (recommended): IAM -> Roles -> DemoRoleForEC2 (IAMReadOnlyAccess), EC2 -> Instances: select instance -> Actions -> Security -> Modify IAM Role -> `aws iam list-users` succeed
