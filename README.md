# Daily-Quote-Automation-with-AWS
The Daily Quote Project is an automated cloud-based application that delivers motivational quotes to users every day through SMS and Email. The project leverages multiple AWS services to manage data, automation, and monitoring. Each component contributes to creating a seamless workflow without manual intervention.

## 🚀 AWS Services Used

| Service         | Purpose                                                                 | Configuration / Details |
|-----------------|-------------------------------------------------------------------------|-------------------------|
| **DynamoDB**    | Stores all motivational quotes.                                         | Table: `Quotes` <br> Partition Key: `id (String)` |
| **Lambda**      | Fetches random quote, stores in S3, and publishes to SNS.              | Function: `DailyQuoteFunction` <br> Runtime: Python 3.12 <br> Role: `LambdaQuoteRole` (DynamoDB, S3, SNS, CloudWatch access) |
| **S3**          | Stores daily selected quote in JSON format for record-keeping.         | Bucket: `quote-of-the-day-hindavi` <br> Object: `quote-of-the-day.json` |
| **SNS**         | Sends quotes to subscribers via SMS and Email.                         | Topic: `DailyQuoteTopic` <br> Subscribers: SMS & Email |
| **EventBridge** | Automates daily execution of the Lambda function.                      | Rule: `DailyQuoteRule` <br> Schedule: `rate(1 day)` / `cron(0 9 * * ? *)` |
| **CloudWatch**  | Monitors executions, logs Lambda and SNS activities, tracks errors.    | Log Group: `/aws/lambda/DailyQuoteFunction` |

## 🏗️ Architecture  

```mermaid
flowchart TD
    EB[EventBridge (Schedule Rule)] --> L[Lambda Function]
    L --> D[(DynamoDB - Quotes Table)]
    L --> S3[(S3 - Daily Quote Storage)]
    L --> SNS[(SNS - Notifications)]
    SNS --> SMS[SMS Subscribers]
    SNS --> Email[Email Subscribers]
    L --> CW[(CloudWatch Logs)]
