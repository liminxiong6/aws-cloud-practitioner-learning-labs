# VPC & Networking Lab

## VPC, Subnet, Internet Gateway & NAAT Gateways
1. Go to VPC Console -> Your VPCs -> Copy the VPC IPv4 CIDR
2. Go to CIDR xyz website -> paste the IPv4 CIDR and check details (Count)
3. Go back to Your VPCs -> CIDRs -> Edit CIDRs (we can add more IPv4 CIDRs)
4. Go to Subnets (default subnets with IPv4 CIDRs, each CIDR corresponding to a diff AZ)
5. Go to EC2 -> Lanuch instances (without key pair) -> Network settings: VPC(default), Subnet(choose any, instance will launch within that subnet)
6. Check the EC2 isntance -> Check the Private IPv4 address (corresponding to the same subnet, same AZ in VPC)
   <img width="1125" height="158" alt="image" src="https://github.com/user-attachments/assets/868ee63f-89dd-4b1b-8094-21621dc1d462" />

1. Go to VPC -> Internet gateways -> A existed internet gateway attach to the current VPC
2. Go to Subnets -> choose the subnet -> Route table -> click on the route table (rtb-XXX) -> Subnet associations (the route table associated with 3 subnets) -> Routes (any traffic go to 0.0.0.0/0, should go to the internet gateway)

   <img width="457" height="203" alt="image" src="https://github.com/user-attachments/assets/a40f1fc9-37f7-4de6-a590-77ff37aadba7" />
4. In advanced course : will create a private subnet and a NAT gateway associated with that private subnet
5. Terminate instance


## Security Groups (instance level) & Network Access Control List (NACL, subnet level)
1. Go to VPC Console -> Security groups -> click on one security group -> Inbound rules
2. Go to VPC Console -> Network ACLs -> select -> 3 subnets associated -> Subnet associations -> Inbound rules
3. Edit inbound rules -> Add new : 200, HTTPS(443), Source: 0.0.0.0/0, Allow -> Delete rule 100 -> Cancel changes

## VPC Flow logs & VPC Peering
1. Go to VPC Console -> Your VPCs -> Choose default VPC -> Flow logs -> Create flow log
2. Go to VPC Console -> Peering connections -> Create peering connection

## VPC Endpoints - Interface & Gateway (S3 & DynamoDB) 
1. Go to VPC  -> Endpoints -> Create Endpoint -> Type: AWS Services, Services: ec2 (interface endpoint)， select vpc, subnet
2. Change Services: s3 (Gateway or Interface endpoint) (Only S3 and DynamoDB also have a gateway endpoint and an interface endpoint)
