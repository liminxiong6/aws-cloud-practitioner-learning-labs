# Leveraging the AWS Global Infrastructure Lab

## Route 53
1. Go to Route 53 -> Domains : Registered domains -> Register Domain "bbeerbear.com" -> Check -> Add to cart -> Continue
2. Go to Hosted zones (have 2 records)
3. Create a EC2 instance from Ireland (Allow HTTP traffic, disable SSH traffic) -> Add User Data Script ("Hello Wolrd from Ireland")
4. Create another EC2 instance from US(Oregon) -> Add User Data ("Hello Wolrd from US")
5. Go to Hosted zones -> Create record -> Record name: www.bbeerbear.com, Value: 34.344.23.234, Routing policy: Latency, Region: Ireland, Record ID: My Instance From Ireland
6. Add another Record -> Record name: www.bbeerbear.com, Value: 52.344.23.234, Routing policy: Latency, Region: Oregon, Record ID: My US Instance -> Create records
7. test the domain name: www.bbeerbear.com in browser
8. delete instances and hosted zone

## CloudFront
1. Go to S3 -> Create S3 bucket: name - "demo-cloudfront-beerbear-v3" -> Create
2. Add files (.jpg, index.html)
3. Go to CloudFront -> Create distribution: name - "DemoCloudFrontDistribution", Single website or app -> Origin type: S3, S3 Origin: "demo-cloudfront-beerbear-v3" -> Allow private S3 bucket access to CloudFront (CloudFront can access the bucket directly, privately) -> WAF: Do not enable security protections -> Create distribution
4. Check S3 bucket permissions -> Bucket policy updated
5. Use Distribution domain name to access the resources in S3 (or cached version)

## AWS Local Zones
1. Go to EC2 -> Choose Region: Europe (Ireland) eu-west-1 -> Account attributes : Zones (Only have availability zone)
2. Change Region to US East (N.Virginia) -> Have Local Zones: US East(Boston) : Want the Boston users have low-latency access -> Manage : Enabled -> Update zone group
3. Launch a instance -> Create a new subnet: name - "boston-subnet", availability zone - "us-east-1-bos-1a" -> IPv5 CIDR block - "172.31.96.0/20" -> So now extend the VPC to local zone (deploy EC2 instances close to the users)
