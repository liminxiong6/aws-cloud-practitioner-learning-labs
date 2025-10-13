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
   
