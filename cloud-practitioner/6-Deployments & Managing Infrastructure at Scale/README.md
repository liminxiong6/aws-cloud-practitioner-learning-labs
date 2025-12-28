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
9. CloudFormation -> Template -> View in Infrastructure Composer
10. Go to Stacks -> Delete stack

## Beanstalk
1. Beanstalk -> Create application -> Web server environment -> Application name: MyApplication -> Environment information: name - MyApplication-dev
2. Platform -> Platform type: Managed platform, Platform: Node.js
3. Application code -> Sample application -> Presets: Single instance
4. Configure service access -> Service role: Create and use new service role: Elastic BeanStalk - Compute -> refresh EC2 instance profile : aws-elasticbeanstalk-ec2-role -> Skip to Review -> Create
5. Check Events(come from Cloudformation Events, Template -> View in Insfrastructure Composer)
6. EC2 console -> 1 instance running (has EIP)
7. Beanstalk -> Enviroments -> MyApplication-dev -> use domain name to access the web
8. Beanstalk -> Application: MyApplication ( Create new envrionment (can create another env for the app)) -> Delete application

## SSM Session Manager (no need to open port 22)
1.  create a instance: ami t2.micro, disable SSH traffic
2.  Open another window, IAM -> create role: entity type-aws service + use case-ec2 role for AWS System Manager -> Permission policies: AmazonSSMManagedInstanceCore -> Role name: DemoEC2RoleForSSM -> Create
3.  IAM instance profile -> choose the newly created role -> Launch instance
4.  Go to ssm -> Fleet Manager (all the instances registered with SSM will appear here)
5.  Ready to run a secure shell against it -> Go to Session Manager -> Start session(secure shell)
6.  `ping google.com` -> `hostname`
7. terminate instance

## SSM Parameter Store
1. Go to ssm -> Parameter Store -> Create parameter -> Name: demo-parameter, Tier: Standard, Type: String, Data type: text, Value: My configuration parameter -> Create
2. Parameter Store -> demo-parameter
3. delete parameter
