## Amazon RDS
- Aurora and RDS Console -> Create db -> Select Fully configuration, Engine options: MySQL, Templates: Free tier (Single AZ), DB instance identifier: database-1, Credentials management: self managed (no additional cost), Select Password authentication, Storage: [Allocated storage: 20GiB, Additional configuration: Enable storage autoscaling & Maximum storage threshold: 1000], Select "Don't connect to an EC2 compute resource", Select "Default VPC", Enable Public (can access DB with a public IP), create new VPC security group: [name: demo-rds, AZ: no preference] -> Create DB
- Download sqlectron from google (SQL client used to connect to DB)
- Modify VPC security group : Enable anywhere IPv4 access to 3306
- Copy the Endpoint of DB -> In SQLectron, click Add button [Server Name: RDSDemo, Database Type: MySQL, Server Address: Endpoint, port: 3306, User: admin, Password: ..., Initial Database: mydb] -> Save and then Connect -> In mydb, `CREATE TABLE mytable (name VARCHAR(20), first_name VARCHAR(20))`, `INSERT INTO 'mytable' ('name', 'first_name') VALUES ("tommy", "john")`
- In AWS console -> db -> Actions: Create read replica -> Multi-AZ deployment: No -> Cancel
- db -> Monitoring
- cleanup: modify db (disable deletion protection and apply immediately) -> delete db (remove create final snapshot)

## Aurora
- Aurora and RDS Console -> Create db -> Select Standard create, Engine options: Aurora (MySQL), Templates: Production, Settings: [DB cluster identifier: database-2, Master username: admin(default), Self Managed password], Select Aurora Standard, Instance configuration: [enable "include previous generation classes", select Burstable classes (db.t3.medium)], Availability & durability: Create an Aurora Replica or Reader node in a different AZ, Select "Don't connect to an EC2 compute resource", Select "Default VPC", Enable Public (can access DB with a public IP), create new VPC security group: [name: demo-database-aurora], disable enhancde monitoring, Additional configuration: [initial db name: mydb, enable deletion protection] -> Create DB
- database-2 has one writer instance and one reader instance, and they are in diff AZ (Connectivity & security tab: one reader endpoint and one writer endpoint)
- Actions (we have Add reader (to scale read capacity) or Create cross-Region read replica or restore to point in time) -> Select Add replica auto scaling -> Add Auto Scaling policy : [Policy nanme: ReadReplicasScalingPolicy, Target metric: Average CPU utilization of Aurora Replicas,Target value: 60%, Cluster capacity details: minimum-1, max-15] -> Cancel
- cleanup: delete the read instance -> delete write instance -> delete the whole database-2 cluster

## ElasticCache
- Amazon ElasticCache -> Get started -> Engine: Redis OSS, Deployment option: Node-based cluster, Creation method: Cluster cache, Cluster mode: disabled (only 1 shard), Cluster Info: DemoCluster, Location: AWS Cloud, disabled Multi-AZ, enable Auto-failover, Cluster settings: [Node type: cache.t2.micro, Number of replicas: 0(save cost)], Subnet group settings: [Name: my-first-subnet-group], Security: [If Enable Encryption in transit, can sleect user group access control list], disable automatic backups -> Create
- can use the cache's primary endpoint or reader endpoint to write some codes
- cleanup: select the cluster -> Actions: Delete (No backup)
