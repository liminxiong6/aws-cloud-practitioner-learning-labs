# Database & Analytics Lab

**Steps:**
RDS (Amazon Relational Database Service)
1. Auroro and RDS: Database -> Create database (MySQL Free tier) -> Full Configuration, MySQL, Sandbox -> Credentials Settings: Master password -> Storage: 20GiB -> Connectivity: public acess 'yes' (connect DB from pc) -> VPC Security Group: Create new
   <img width="1064" height="596" alt="image" src="https://github.com/user-attachments/assets/5ecc7d5d-be8e-46cc-8ec8-26042041d032" />
2. Check Connectvitiy & security : Endpoint, VPC security groups
3. Check Monitoring
4. Actions -> Take snapshot (allow to restore the DB to another one) -> use newly created snapshot to resotre the snapshot (create a bigger or copy DB with diff settings)

   <img width="651" height="418" alt="image" src="https://github.com/user-attachments/assets/28e2237f-7c31-4149-910a-9df07a91ae81" />
6. Copy snapshot -> choose a different region (restore DB to a diff region)
7. Share snapshot (share with others AWS accounts)
8. Delete snapshot and database


DynamoDB
1. Create table (without create database: already exists 'serverless')
   <img width="943" height="377" alt="image" src="https://github.com/user-attachments/assets/423a1bc2-97e9-4197-956a-68ad9b8f1ab3" />
2. Explore table items -> create item
   <img width="1201" height="407" alt="image" src="https://github.com/user-attachments/assets/647059bd-29e7-41be-a105-5c58afbb375c" />
   <img width="642" height="129" alt="image" src="https://github.com/user-attachments/assets/25a0f30f-7fa8-40b7-8b22-3d6c182fffe3" />
3. Delete table
 
