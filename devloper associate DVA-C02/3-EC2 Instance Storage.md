Amazon EFS
1. Go to Amazon Elastic File System -> Create file system -> use default VPC -> customize 
2. File system type: Regional -> Lifecycle management: Transition into IA(30 days), Transition into Archive(90 days), Transition into Standard(On first access) -> Perfomance: Elastic
3. Network access: Mount targets(3 AZs)
4. Go to EC2 -> Create Security Group for efs: name - efs-demo, Description - EFS Demo SG
5. Go back to Network access: Mount targets(3 AZs) -> All AZs' Security groups - efs-demo -> Create EFS
6. EFS file system only pay for the storage you use
7. Mount EFS to EC2 instances: Launch EC2 instances -> name: Instance A, t2.micro, other default settings -> Select subnet same as EFS -> EBS Volumnes File systems: EFS -> Add shared file system -> Launch instance
8. Launch another one: name: Instance B, t2.micro -> Select subnet same as EFS & diff from Instance A ->  Select existing sg (same as Instance A created) -> EBS Volumnes File systems: EFS -> Add shared file system -> Launch instance
9. Go to EFS console -> Network tag: each AZ has multiple security groups (efs-demo created before, efs-sg-1 & efs-sg-2 are auto created by EC2 console and attached into EFS)
10. Go to EC2 -> Security group: efs-sg-2 -> Inbound rules: Allows NFs protocol on port 2049, the source is the security group attached to newly created EC2 instance A & B
11. To verify EFS is correctly mounted on Instance A and B, use EC2 instance connect to one instance, and run `ls /mnt/efs/fs1`, `sudo su`, `echo "hello world" > /mnt/efs/fs1/hello.txt`
12. Connnect to another instance with EC2 instance connect, and run `cat /mnt/efs/fs1/hello.txt`, so file is shared through diff AZ
13. Clean up: Deletec EFS and Terminate EC2 instances and Delete SGs(except for the default one).
