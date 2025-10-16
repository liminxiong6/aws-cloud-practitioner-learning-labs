# Cloud Monitoring

## CloudWatch Metrics & CloudWatch Alarms
1. Create a instance
2. Go to CloudWatch -> All metrics -> SQA -> Queue Metrics -> demo-sqs
3. All metrics -> EC2 -> Per-Instance Metrics -> CPU utilization
4. Alarms -> All alarms -> Create alarm -> EC2 -> CPUUtilization -> Select metric -> Whenever CPUUtilization is Greater than 80 -> Alarm state trigger : In alarm -> Create new SNS topic : Email - bbbear@example.com -> Next
5. Alarm name: DemoAlarm -> Create alarm
6. Go to EC2 instances -> Alarm status + -> Create an alarm -> Alarm notification choose the topic created before -> Alarm action : Recover -> Group samples by "Average", Type of data to sample "Status check failed: system"
7. Check Alarm status (have 2 alarms)
8. Create third alarm -> Alarm action : Reboot -> Group samples by "Average", Type of data to sample "CPU Utilization", Percent "95"
9. Go to CloudWatch -> All alarms(3 alarms)
10. Change region to us-east-1 (the only region having billing alarm)
11. Go to Billing -> Create alarm -> Greater than 8 USD -> In alarm -> Create new topic "Default_CloudWatch_Alarms_Topic_billing" -> bbbear@example.com -> Create topic -> Alarm name: DemoBillingAlarm -> Create alarm
12. Delete Billing alarm -> Go back to origin Region

## CloudWatch Logs 
1. Go to CloudWatch -> Log groups (demo-lambda, lambda fn automatically create logs into CloudWatch logs)
2. choose that log group -> click on log stream -> Log lines
   <img width="1058" height="401" alt="image" src="https://github.com/user-attachments/assets/3b78d177-bfbc-4571-97b6-740ab46a099e" />
3. Go to lambda fn -> Code source -> add `print("An extra log line!")` -> Deploy -> Test
4. log group -> new log stream
   <img width="984" height="384" alt="image" src="https://github.com/user-attachments/assets/5d232901-e949-4c5a-b627-9d7e4cb4e9e6" />
5. Go to lambda fn -> Deploy -> Test
   <img width="697" height="187" alt="image" src="https://github.com/user-attachments/assets/c0e9fb85-5617-46a1-8305-5f7297011c3c" />
6. new log stream
   <img width="1069" height="396" alt="image" src="https://github.com/user-attachments/assets/c23b0c7a-db8e-4e0d-a016-e115fcb0996f" />



