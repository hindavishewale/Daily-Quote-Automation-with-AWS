# Daily-Quote-Automation-with-AWS
The Daily Quote Project is an automated cloud-based application that delivers motivational quotes to users every day through SMS and Email. The project leverages multiple AWS services to manage data, automation, and monitoring. Each component contributes to creating a seamless workflow without manual intervention.
Service	Purpose	Configuration / Details
DynamoDB	Stores all motivational quotes.	Table: Quotes
Partition Key: id (String)
Lambda	Fetches random quote, stores in S3, and publishes to SNS.	Function: DailyQuoteFunction
Runtime: Python 3.12
Role: LambdaQuoteRole with DynamoDB, S3, SNS, CloudWatch access
S3	Stores daily selected quote in JSON format for record-keeping.	Bucket: quote-of-the-day-hindavi
Object: quote-of-the-day.json
SNS	Sends quotes to subscribers via SMS and Email.	Topic: DailyQuoteTopic
Subscribers: SMS & Email
EventBridge	Automates daily execution of the Lambda function.	Rule: DailyQuoteRule
Schedule: rate(1 day) or cron expression
CloudWatch	Monitors executions, logs Lambda and SNS activities, tracks errors.	Log Group: /aws/lambda/DailyQuoteFunctio
