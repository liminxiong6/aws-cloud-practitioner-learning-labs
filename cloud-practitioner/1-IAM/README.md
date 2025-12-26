# IAM (Identity and Access Management) Lab

**Steps:**
IAM user and groups (IAM is Global service)
1. Log into AWS Console using root account
2. Go to IAM → Users -> Create User -> Create Group
   <img width="1147" height="697" alt="image" src="https://github.com/user-attachments/assets/60b1099e-c607-478e-9705-255452f2118f" />
   <img width="812" height="458" alt="image" src="https://github.com/user-attachments/assets/2b19f898-aed7-435a-aeb9-735167545afe" />
   <img width="1174" height="197" alt="image" src="https://github.com/user-attachments/assets/a6a9da77-120b-4978-868d-f9e432550a91" />
3. Check User Permissions policies
   <img width="1582" height="316" alt="image" src="https://github.com/user-attachments/assets/6b215ec3-56ae-45e4-9371-cbb0ed58e7bf" />
4. Dashboard -> Account Alias -> Create
5. Turn on multi-session support (Can login 2 accounts in the same brower)
6. Sign-in console -> IAM user

IAM policies
1. Groups -> remove user from the group
2. Users -> Permissions -> Add permission
   <img width="1599" height="493" alt="image" src="https://github.com/user-attachments/assets/fc780c40-480b-418b-a7b2-dab8b545ba8e" />
3. Groups -> add user back to the group
4. Policies -> Search AdministratorAccess (allows all the services: 384 of 384) -> Search IAMReadOnlyAccess -> Permissions (1 of 384) -> JSON (Show how the policy is defined)
5. Policies -> Create policy -> Visual Editor
   <img width="1279" height="374" alt="image" src="https://github.com/user-attachments/assets/e145f99c-6d8c-4e5d-b7e0-f66ef819f82f" />

IAM Password Policy & MFA (Multi Factor Authentication)
1. IAM -> Account settings -> Edit Password policy -> Custom
2. Security credentials -> Assign MFA device
   <img width="361" height="189" alt="image" src="https://github.com/user-attachments/assets/f4c26ad1-a479-4c4e-b18d-7a34502604c8" />

AWS CLI
1. Windows: AWS MSI Installation, Mac: AWS pkg installation
2. Create Access Key: User -> Security credentials -> Create access key -> CLI
3. Command: `aws configure` -> `aws iam list-users`

IAM Roles for AWS services
1. Roles -> Create role -> Add permissions
   <img width="1124" height="562" alt="image" src="https://github.com/user-attachments/assets/13552234-83b2-40d4-b1ae-d78b9163b4e2" />

IAM Security Tools
1. Credentials report -> Download
2. Users -> Last Accessed(Access Advisor)

