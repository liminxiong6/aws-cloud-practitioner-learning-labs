# EC2 Instance Storage Lab

**Steps:**

EBS Volume (Elastic Block Store: Persist data even the instance is termindated, only mounted to 1 instance at a time, bound to a specific AZ)
1. Volums -> Create volume
   <img width="850" height="622" alt="image" src="https://github.com/user-attachments/assets/dd651ed9-88e1-4182-8ebe-d48484d84fe0" />
2. Select created volume -> Actions -> Attach volume
   <img width="642" height="420" alt="image" src="https://github.com/user-attachments/assets/08aa01e8-eac2-478b-a2c5-52d63c9014e5" />
3. Instances -> select instance -> Storage -> Block devices (2 volumes: Root volume 'delete on termination')

EBS Snapshot/backup(can copy snapshot accross region)
1. Recommend to detach the volume before backup
2. Volumes -> select a volume -> Actions: Create snapshot
3. Snapshots -> new snapshot in the list -> right click: Copy snapshot -> Choose another region
4. Snapshots -> select snapshot -> Actions: Create volume from snapshot -> Can choose a different AZ
5. Snapshots -> Recycle bin -> Create retention rule
  <img width="1008" height="443" alt="image" src="https://github.com/user-attachments/assets/9b70f7e5-03dc-448c-9412-ebd8a419e065" />
6. Snapshots -> select snapshot -> Actions: Archive snapshot
6. Snashots -> delete snapshot -> Recycle Bin -> recover snapshot


AMI (Amazon Machine Image: Built for a specific region, can copy accross the region)
1. Create EC2 instance (Amazon Linux) -> User data (install apache server) 
  <img width="717" height="271" alt="image" src="https://github.com/user-attachments/assets/2e57134b-cf9b-4d75-9094-846ef08e6e8e" />
2. right click instance -> Image and templates -> Create image
3. Images -> AMIs 
   <img width="1660" height="166" alt="image" src="https://github.com/user-attachments/assets/4930e1f0-ff62-452a-be72-964668572411" />
4. Instances -> Launch a instance -> select custom AMI -> user data (add the web server page)
   <img width="986" height="442" alt="image" src="https://github.com/user-attachments/assets/726fa540-8581-49a4-b2d7-975b28178835" />
   <img width="677" height="230" alt="image" src="https://github.com/user-attachments/assets/26c917fa-2a4f-4d8c-84cb-64f02ced762e" />

Cleanup
1. EC2 Dashboard -> Instances: terminate -> Volumes: delete -> AMIs: deregister
