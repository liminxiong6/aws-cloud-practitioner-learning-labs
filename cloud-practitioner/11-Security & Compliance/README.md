## Encrytion with KMS & CloudHSM
1. Go to EC2 -> Create Volume -> Encrypt this volume
   <img width="1050" height="160" alt="image" src="https://github.com/user-attachments/assets/08e58e17-670f-4c71-a9ea-7b02e2269f6a" />
2. Go to KMS -> AWS Managed Keys -> aws/ebs
3. Cloudtrail/Glacier S3 : encrytion is enabled by default
4. Create customer managed keys -> Symmetric -> key material origin : KMS/External(own key)/CloudHSM -> choose KMS -> Alias: demokey (create cost 1$) -> Key rotation
5. Delete Volume -> Disable key

## Secrets Manager
1. Go to Secrets Manager -> store a new secret -> Other type of secrets
2. Secret key/value: Password - MYSECRETPASSWORD -> Secret name: myproductionapplicationpassword -> Enable automatic rotation -> rotation interval: 30 days
3. Create rotation funciton

## Artifact (Not a service)
1. Go to AWS Artifact -> View reports -> select one and download it
2. Go to Agreements -> select one and download it

## AWS Config (Not free) 
1. Enable "Record all resources supported in this region", "include global resources" -> Create a bucket -> Create AWS Config service-linked role
2. AWS Config rules: restricted-ssh, rds-instance-public-access-check, s3-bucket-logging-enabled... (check the compliance of resources within our account) -> for now only enable "restricted-ssh"
3. Dashboard -> Resource inventory (see all my resources in AWS) -> Noncompliant resources
4. Go to Resources -> Find noncompliant security groups -> click -> ResourceTimeline (Configuration timeline, compliance timeline)
5. Go to EC2 console -> Security Groups -> Filter security groups -> Edit Inbound rules -> Delete SSH for 0.0.0.0/0 rule
6. Wait for some time and check the ResourceTimeline, or faster: AWS Config -> Rules -> relaunch/re-evaluate restricted-ssh rule
7. Compliance timeline -> Changes (Noncompliant -> compliant), Configuration timeline -> Changes -> From ... to ..., CloudTrail Events: root user -> RevokeSecurityGroupIngress

## AWS Security Hub
1. Go to Security Hub

## IAM Access Analyzer
1. Go to Access Analyzer -> Resource analysis - External access -> it will create a service-linked role to allow the analyzer to interact with resources on our behalf
2. Findings
  <img width="1070" height="272" alt="image" src="https://github.com/user-attachments/assets/62af2b1c-fead-4534-a55f-6862776cad10" />
3. Intended or not intended -> If try to fix it, choose not intended -> delete bucket policy
4. We can create archive rule to always enable this type of public access

5. 
