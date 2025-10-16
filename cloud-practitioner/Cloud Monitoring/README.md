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
