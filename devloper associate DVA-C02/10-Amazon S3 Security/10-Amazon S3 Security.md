## S3 Encryption
- Create a bucket -> bucket name: demo-encryption-myname-v2, Enable bucket versioning, Default encryption: SSE-S3 -> Create
- Upload a file (coffee.jpg) -> Open the file -> Server-side encryption settings is SSE-S3 currently -> Edit -> Server-side encryption: [Encryption settings: Override bucket settings for default encryption, Encryption type: SSE-KMS, Choose from your AWS KMS keys: arn:aws:kms:..(default key for S3 service, free)] -> Save changes
- coffee.jpg has a new version (SSE-KMS)
- Upload a new file in the bucket (beach.jpg) -> Properties: [Server-side encryption: Specify an encryption key, Encryption settings: Override bucket settings for default encryption, Encryption type: SSE-S3]aws s3api put-bucket-versioning --bucket mfa-demo-stephane --versioning-configuration Status=Enabled,MFADelete=Enabled --mfa "arn-of-mfa-device mfa-code" --profile root-mfa-delete-demo
- bucket -> Properties tab -> Edit Default encryption -> Encryption type: SSE-KMS, Bucket Key: Enable (reduce cost by less KMS API calls)

## S3 CORS
- Go to myname-demo-s3 bucket (with beach.jpg, coffee.jpg, index.html) -> upload index.html, extra-page.html
- Properties tab -> open Endpoint in the web browser -> Same origin works
- Create another bucket -> name: demo-other-origin-myname, AWS Region: ca-central-1, Unblock all public access -> Create
- Properties tab -> Enable static website hosting, Index document: index.html
- To make this bucket object public -> Permissions tab ->  copy the bucket policy before and paste it -> update the bucket arn -> Save changes
- Upload extra-page.html in this bucket -> Can access the Object URL in the browser
- In Origin bucket: remove extra-page.html -> change the index.html file to point to the extra-page.html object URL in the other-origin bucket， instead of "extra-page.html" -> reupload the index.html file
- Refresh the index.html webpage, in Chrome developer tools -> Cross-Origin Request block
- Go to demo-other-origin-myname bucket -> Permissions tab -> Edit CORS -> Save

## S3 MFA Delete
- Login with root account -> Create a bucket -> bucket name: demo-myname-mfa-delete-2020, enable bucket versioning -> create
- bucket -> Properties tab -> Edit Bucket versioning -> MFA delete is currently disabled (can't enable it with UI, but AWS CLI can)
- make sure already setup MFA of root account -> Login with root account -> My Security Credentials under the acccount dropdown panel (IAM) -> has MFA -> create new access key for CLI to use this root account (not recommended, only for the MFA Delete Configuration)
- In CLI, create a profile `aws configure --profile root-mfa-delete-demo`, check if this profile works `aws s3 ls --profile root-mfa-delete-demo`
- enable MFA Delete `aws s3api put-bucket-versioning --bucket demo-myname-mfa-delete-2020 --versioning-configuration Status=Enabled,MFADelete=Enabled --mfa "arn-of-mfa-device(arn:aws:...) mfa-code(712385)" --profile root-mfa-delete-demo`
- Check if MFA Delete is enable -> in bucket, upload coffee.jpg then delete it -> Objects and enable List versions -> Delete previous version
- `aws s3api put-bucket-versioning --bucket demo-myname-mfa-delete-2020 --versioning-configuration Status=Enabled,MFADelete=Disabled --mfa "arn-of-mfa-device(arn:aws:...) mfa-code(712385)" --profile root-mfa-delete-demo`
- Delete root access key in the IAM console !!!

## S3 Access logs
- Create bucket -> bucket name: s3-access-logs-myname-v3 -> Create
- Go to demo-myname-v3-event-notifications bucket -> Properties -> Server access logging -> Edit -> Enable, Destination: s3-access-logs-myname-v3, Log object key format: [DestinationPrefix][YYYY]... -> Save changes
- In demo-myname-v3-event-notifications bucket, upload a file beach.jpg -> generate activity and logged to the logging bucket
- In s3-access-logs-myname-v3 -> has new log files

## S3 Pre-signed URLs
- Go to demo-myname-v3-event-notifications bucket -> coffee.jpg object URL can't work cuz the bucket is not public -> but click on the Open button, can access it (Pre-signed URL)
- To generate pre-signed URL -> Object actions button -> Share with a presigned URL -> Time interval until the presigned URL expires: 5 minutes
