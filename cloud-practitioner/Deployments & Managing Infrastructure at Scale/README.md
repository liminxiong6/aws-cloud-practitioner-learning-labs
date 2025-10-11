# Deployments & Managing Infrastructure at Scale Lab

## CloudFormation
1. CloudFormation -> Create Stack: Region choose 'us-east-1'
2. Choose an existing template -> Upload a template file (0-just-ec2.yaml) -> View in Application Composer
3. Provide a stack name "DemoCloudFormation"
4. Add Tag: Name - CFDemo
5. Submit
6. Go to Instances -> Look at the Tags
7. Go to Stacks -> Update (direct update) -> Replace existing template (1-ec2-with-sg-eip.yaml) -> Parameters: SecruityGroupDescription - "Demo Description" -> Submit
8. Go to Instances -> Networking(EIP attached)
9. Go to Stacks -> Delete stack

## Beanstalk
1. Beanstalk -> Create application -> Web server environment -> Application name: MyApplication -> Environment information: name - MyApplication-dev
2. Platform -> Platform type: Managed platform, Platform: Node.js
3. Application code -> Sample application -> Presets: Single instance
4. Configure service access -> Service role: Create and use new service role
5. Open another window, Go to IAM console -> create role: AWS service, Use case: EC2 -> Permissions policies: type "beanstalk", add AWSElasticBeanstalkWebTier, AWSElasticBeanstalkWorkerTier, AWSElasticBeanstalkMulticontainerDocker -> Role name: aws-elasticbeanstalk-ec2-role -> Create role
6. Beanstalk -> EC2 instance profile: aws-elasticbeanstalk-ec2-role -> Skip to review -> Submit
7. Events(come from Cloudformation: template, resources, view in application composer)
8. Beanstalk -> Enviroments -> MyApplication-dev -> use domain name to access the web
9. Beanstalk -> Application: MyApplication ( Create new envrionment (can create another env for the app)) -> Delete application
