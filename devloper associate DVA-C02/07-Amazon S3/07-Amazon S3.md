## S3
- S3 -> Buckets -> Create bucket -> bucket type: General purpose, bucket name: myname-demo-s3-v5 -> ACLs disabled -> Block all public access -> Disable bucket versioning -> Create
- Click on the newly created bucket -> Upload coffee.jpg -> Click on open button, but Object URL get access denied
- bucket -> Create folder -> name: images -> upload beach.jpg in this folder

## S3 Security: Bucket Policy
- bucket -> Permission tab -> Edit Block public access to disable it -> Bucket policy -> Edit -> Policy generator -> Type: S3 bucket policy, Effect: Allow, Principal: *(allow anyone), AWS Service: Amazon S3, Actions: GetObject, Amazon Resource Name (ARN): Copy and paste the Bucket ARN and then add "/*" -> Generate Policy and then paste the json into the Edit bucket Policy -> Save changes
- Access the coffee.jps by object URL

## S3 Website
- bucket -> upload index.html -> Properties tab -> Enable Static website hosting, index document: index.html -> Save
- In Properties tab -> have a bucket website endpoint in the static website hosting

## S3 Versioning
- bucket -> Properties tab -> Enable Bucket Versioning
- Edit html file: I Really love coffee -> Uplod it
- bucket Objects tab -> Show versions -> other objects' Version ID: null, index.html has 2 versions
- Click on the index.html (has version ID one) and delete it
- If refresh the html page, change back to "I love coffee"
- Disable show versions -> Delete coffee.jpg -> adds delete marker
- Enable show versions -> coffee.jpg still there (type: delete marker)

## S3 Replication
- Create bucket -> name: s3-myname-bucket-origin-v2, AWS Region: eu-west-1, Enable Bucket Versioning -> Create
- Create bucket -> name: s3-myname-bucket-replica-v2, AWS Region: us-east-1, Enable Bucket Versioning -> Create
- In origin bucket, upload beach.jpg
- origin bucket -> Management tab -> Create replication rule -> rule name: DemoReplicationRule, Status: Enabled, rule scole: APply to all objects in the bucket -> Destination: s3-myname-bucket-replica-v2, IAM role: create new role -> Save -> No, do not replicate existing objects
- Upload a new file in the origin bucket -> added also in the target bucket -> Version ID are the same in each bucket
- Edit replication rule -> Enable Delete marker replication-> Delete coffee.jpg in the origin bucket -> target bucket also get the delete marker
- but if delete a specific verion in origin bucket -> will not be replicated to the replica bucket

## S3 Storage Classes
- Create bucket -> name: s3-storage-classes-demos-2022, AWS Region: eu-west-1 -> Create
- Objects tab -> Upload -> Add files: coffee.jpg, Properties: [Storage class: Standard-IA]
- Click on the uploaded file -> Edit storage class -> One Zone-IA -> Save changes
- Go to bucket -> Management tab -> Create lifecycle rule -> name: DemoRule, Select apply to all objects in the bucket, Lifecycle rule actions: Move current versions of objects between storage classes, Transition: [Standard-IA after 30 days, Intelligent-Tiering after 60 days, Glacier Flaxible Retrival after 180 days] -> Cancel
