## S3 Encryption
- Create a bucket -> bucket name: demo-encryption-myname-v2, Enable bucket versioning, Default encryption: SSE-S3 -> Create
- Upload a file (coffee.jpg) -> Open the file -> Server-side encryption settings is SSE-S3 currently -> Edit -> Server-side encryption: [Encryption settings: Override bucket settings for default encryption, Encryption type: SSE-KMS, Choose from your AWS KMS keys: arn:aws:kms:..(default key for S3 service, free)] -> Save changes
- coffee.jpg has a new version (SSE-KMS)
- Upload a new file in the bucket (beach.jpg) -> Properties: [Server-side encryption: Specify an encryption key, Encryption settings: Override bucket settings for default encryption, Encryption type: SSE-S3]
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
