## CloudFront
- Create a bucket -> name: demo-cloudfront-myname-v4 -> Create
- Upload beach.jpg, coffee.jpg, index.html
- Go to CloudFront -> Create a CloudFront distribution -> Free plan -> Distribution name: DemoNewCloudFront -> Origin type: Amazon S3, select S3 origin, Enable Allow private S3 bucket access to CloudFront -> Create
- S3 bucket -> Permission tab -> Bucket policy (is allowing access to bucket from CloudFront distribution)
- Access CloudFront distribution domain name + /index.html, so even the bucket is private but we can use cloudfront to acess bucket object -> and since the objects are cached, so loading is almost instantaneous

## CloudFront - Caching & Caching Invalidations
- DemoNewCloudFront -> Behaviors tab -> Edit Default behaviors -> Cache key and origin request policy -> Create Cache policy -> name: DemoCachePolicy, Cache key settings (Headers, Query strings, Cookies...) -> Cancel create cache policy -> Cache key and origin request policy -> Create origin request policy(extra header/query string pass to origin request) -> Name: DemoOriginRequestPolicy, Origin request settings: Include the following headers... -> Cancel
- Or we can: DemoNewCloudFront -> Behaviors tab -> Create behavior -> Path pattern: /images/*, Origin and origin groups: demo-cloudfront-myname-v4.s3...(s3 bucket origin) -> 
- Change the index.html file <h1> to "I REALLY love coffee EVERY MORNING" -> Upload this new index.html -> Access CloudFront distribution domain name + /index.html, still "I REALLY love coffee" (cache previous version for one day)
- DemoNewCloudFront -> Invalidations tab -> Create -> Object paths: /*
- Refresh DemoNewCloudFront domain name + /index.html -> Update to "I REALLY love coffee EVERY MORNING"

## CloudFront - Geo Restriction
- Need to change from Free plan to Pay-as-you-go -> Security tab -> CloudFront geographic restrictions -> Edit -> Allow list/Block list

## CloudFront signed URL - Key groups
- CloudFront console -> Key Management -> Public keys -> Google: search "generate rsa key online 2048 bit", key size: 2048 bit -> Create public key -> Name: DemoKey, Key: upload the public key
- CloudFront console -> Key Management -> Key groups -> Create -> Name: DemoKeyGroup, Public Key: Choose the newly created one (allow users/EC2 instance to create signed URL
- Old way of adding keys into CloudFront: Log in as Root Account -> My Account -> My Security Credentials -> CloudFront key pairs -> Create New Key Pair (not recommended, not secure)
