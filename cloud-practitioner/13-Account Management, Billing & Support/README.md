## Organizations 
1. Create two new aws account (aws-course-master, aws-course-child)
2. Go to aws-course-master account -> Create AWS Organizations -> Add an AWS account -> Invite an existing AWS account (aws-course-child@gmail.com) -> Send invitation
3. Go to aws-course-child account -> invitations -> Accept
4. Go back to aws-course-master account -> Organization has new account -> Root -> create new OU : OU name - Dev -> create new OU : OU name - Test -> create new OU : OU name - Prod
5. Go to Prod OU -> Create new OU : OU name - HR -> Create new OU : OU name - Finance
6. Select aws-course-child account -> Move to Finance OU
7. Go to Policies -> Enable SCP (Service control policies : Restrict what the account can do) -> Create policy : name - DenyAccessS3 -> Edit statement: search S3 -> All actions (s3:*), Sid: DenyS3, Resources: ["*"]
8. Go to Finance OU policies -> Attach new policy (DenyAccessS3) -> aws-course-child also inherit that policy
9. Go to aws-course-child account -> check if access to S3 is denied

## AWS Control Tower(costly, not recommend to do this hands on)
1. Go to AWS Control Tower -> Set up landing zone -> OU (security OU + Sandbox  OU(your other accounts)) -> Management account (current account) -> Create log archive account -> Enable cloudtrail -> Enable KMS Encryption
2. Go to AWS Organizations -> Have 3 Accounts within organization ( Management account + Log archive account + Audit account) -> Don't manage the accounts through Orgnization but Control Tower
3. Control Tower -> Dashboard : Recommended actions -> Add or register organizational units / Configure your account factory
2. AWS Control  -> OUs / Accounts / Account factory (Enroll account into Control Tower) / Users and access / Guardrails

## Savings Plan
1. Services: Search saving plans -> Under AWS Cost Explorere, choose Savings Plan -> Purchase a Savings Plan -> Compute Savings Plans, Term: 3-year， Hourly commitment: 500， Partial Upfront

## Estimating Costs in the Cloud - Pricing Calculator
1. Google search: AWS Pricing Calculator -> Find Service: EC2 -> Configure -> Description: EC2 Workload, EC instances: 4 vCPUs + 16 GiB -> Choose t4g.xlarge EC2 instance -> 4 instances -> EBS: Storage amount: 30 GB -> Save and view summary
2. Add Service -> Elastic Load Balancing -> Configure -> 5 GB per hour processed bytes (EC2 instances and IP addresses as targets) -> 5 new connections per second

## Tracking Costs in the Cloud - Billing Dashboard, Cost Allocation Tag, Reports
1. click top right account name to show the panel -> Choose Billing and Cost Management -> Cost breakdown
2. To explore the bill deeper: use Cost Allocation Tags -> Services: Resource Groups & Tag Editor -> Open both Resource Group window and Tag Editior window
3. In Tag Editor -> Resource type: Security Group -> Select all search results -> Manage tags of selected resources -> Add Tag: Department - IT -> Review and apply changes
4. Create Resource Group -> Tag based, Tag: Departmnet - IT -> Group name: IT-Resources
5. Billing and Cost Management -> Cost Allocation Tags
6. Cost and Usage Analysis -> Data Exports (export cost and usage reports) -> Create Standard data export
7. Cost and Usage Analysis -> Cost Explorer -> Save to report library -> Access it through Cost Explorer Saved Reports

## Monitoring Costs in the Cloud - Billing Alarms & AWS Budgets 
1. Services: AWS Budgets -> Create budget -> Customize (advanced) -> Cost budget -> Budget name - DemoBudget, Period - Monthly, Budgeted amount - 10, Scope - Filter specific AWS cost dimensions
2. Filter -> Dimension: Service, Values: EC2 & Key Management Service -> Apply filter
3. Add an alert threshold -> 80 % of actual budgeted amount -> Email recipients: xxx@xxx.com
4. Add another alert threshold -> 80% of forecasted amount -> Email recipients: xxx@xxx.com
5. Can check the Budget preview on the right hand side (or view in AWS Cost Explorer)
