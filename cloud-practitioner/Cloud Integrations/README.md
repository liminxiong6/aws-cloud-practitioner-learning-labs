# Cloud Integrations Lab

## SQS (simple queue service)
1. Go to SQA -> Create queue: Standard queue, name - "demo-sqs" -> Create
2. Send and receive messages -> Send message : "Hello world!" -> View details -> Receive messages (1 Messages available)
3. Send message "another Hello!"
4. Click on poll for messages (2 messages received) -> click on the message -> Body : "Hello world!"
5. Select 2 messages -> Delete
6. Queues -> Delete queue

## SNS (simple notification service)
1. Go to SNS -> Create topic: demo-sns -> Create 
2. Create subscription -> Protocol: Email, Endpoint: xxx@mailinator.com -> Create subscription -> Check the Mailinator mail (new email) -> Create
3. Publish message -> Subject: Demo Subject Line, Message body: Hello world! -> Publish -> Check the Mailinator mail (new email)
4. Delete topic
