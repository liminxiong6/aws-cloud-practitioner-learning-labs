# 🪣 S3 Bucket Lab

**Steps:**
Create a bucket
1. Log into AWS Console
2. Go to S3 → Create Bucket -> Name: `test-bucket`
3. Objects -> Upload a photo object or create a folder

Bucket Policy (Make objects public access to all visitors)
1. Go to S3 bucket -> Permissions 
2. Disable "Block all public access"
3. Bucket Policy -> Policy Generator
   <img width="1370" height="729" alt="image" src="https://github.com/user-attachments/assets/e1409781-b1e1-4f05-92ce-01ef4d8fa649" />
4. Copy Policy -> Paste in Bucket Policy and Save

S3 Websites:
1. Properties -> Enable Static website hosting -> Index document: index.html -> Save
2. Objects -> Upload index.html
3. Properties -> Static website hosting -> open the Bucket website hosting url

S3 Versioning
1. Properties -> Enable Bucket Versioning
2. Change index.html file and upload it again
3. Objects -> Show versions (Version ID) -> Select and Delete index.html (new version)
4. disable `Show versions` and Delete photo object
5. enable `Show versions` and Delete photo object(type: Delete Marker)

S3 Replication
1. Create origin bucket (Region: ca-central-1) -> enable versioning (Replication only works if versioning is enabled) -> create bucket -> upload a file
2. Create replica bucket (diff Region) -> enable versioning -> create bucket
3. In origin bucket -> Management -> Replication rules -> Create replication rule -> Save -> Do not replicate existing objects
   <img width="1530" height="733" alt="image" src="https://github.com/user-attachments/assets/cf245c0e-e9f2-432d-b8ed-14d180a7dbf0" />
4. Upload the file in origin bucket (In replica bucket, the version ID of the file is replicated)

S3 Storage Classes
1. Create a bucket -> Upload a file -> Properties -> Storage class: Standard-IA -> Upload
2. bucket -> Management -> create lifecycle rule
   <img width="1512" height="661" alt="image" src="https://github.com/user-attachments/assets/a3553fd9-1e44-4bc4-a55f-9b68fe59b3cc" />

AWS Snow Family
1. Create new job -> Import into Amazon S3 -> Select S3 bucket 
   <img width="1287" height="559" alt="image" src="https://github.com/user-attachments/assets/fe1379c8-4c88-4605-b8e3-9e59fce13300" />
2. Choose service access type -> Create service role -> Shipping address]
3. Receive snowball device and load data onto it and send it back to AWS

