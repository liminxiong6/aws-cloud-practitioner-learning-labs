## S3 Lifecycle Rules
- S3 bucket (with a coffee.jpg file uploaded) -> Management -> Create lifecycle rule -> Lifecycle rule name: DemoRule, rule scope: Apply to all objects in the bucket, Lifecycle rule actions: Select "Move current versions of objects between storage classes" & "Move noncurrent versions..." & "Expire current versions of objects" & "Permanently delete noncurrent versions of objects"
- Transition current versions of objects between storage classes: Stardard-IA after 30 days, Intelligent-Tiering after 60 days, Glacier Instant Retrieval after 90 days, Glacier Flexible Retrieval after 180 days, Glacier Deep Achieve after 365 days -> Select "I acknowledge ..."
- Transition noncurrent versions of objects between storage classes: Glacier Flexible Retrieval after 90 days obejcts become noncurrent. -> I acknowledge...
- Expire current versions of objects: 700 Days after object creation
- Permanently delete noncurrent versions of objects: 700 Days after objects become noncurrent
- Create rule

## S3 Event Notifications
- S3 -> Create bucket -> bucket name: demo-myname-v3-event-notifications -> Create bucket
- Properties tab -> Anazon EventBridge -> On -> Save changes
- Properties tab -> Event Notifications -> Create -> Event name: DemoEventNotification, Event types: [Object creation: All object create events], Destination: [Select SQS queue, Choose from your SQS queues]
- Create a SQS queue -> Name: DemoS3Notification -> Create queue
- SQS queue created -> Access policy tab (to allow s3 bucket to write into the sqs queue!) -> Edit Access policy -> policy generator -> Type of Policy: SQS Queue Policy, Effect: Allow, Principal: *(Anyone), Actions: SendMessage, Amazon Resource Name: copy from the current access policy (arn:aws:sqs:...) -> Add Statement -> Generate Policy
- S3 bucket event notification creation -> Select the newly created SQS queue -> Create
- Go to SQS queue -> Send and receive messages -> Poll for messages -> a message was being sent by Amazon S3 to test connectivity -> Delete this message
- Go to bucket -> Upload coffee.jpg
- Go to SQS queue -> Send and receive messages -> Poll for messages -> Message: [eventName:ObjectCreated:Put, key:coffee.jpg]
