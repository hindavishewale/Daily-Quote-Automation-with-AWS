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

## 📌 Architecture  
```mermaid
flowchart TD
    EB[EventBridge (Schedule Rule)] --> L[Lambda Function]
    L --> D[(DynamoDB - Quotes Table)]
    L --> S3[(S3 - Daily Quote Storage)]
    L --> SNS[(SNS - Notifications)]
    SNS --> SMS[SMS Subscribers]
    SNS --> Email[Email Subscribers]
    L --> CW[(CloudWatch Logs)]

📊 Monitoring

CloudWatch Logs capture Lambda start/end time, SNS responses, and errors.

Can be extended with CloudWatch Alarms to send alerts on failures.

DynamoDB Quotes Table
<img width="1904" height="924" alt="image" src="https://github.com/user-attachments/assets/a70d70c1-9a11-4f0c-bda1-7ebbb68f45ca" />

CloudWatch Logs
<img width="1917" height="940" alt="image" src="https://github.com/user-attachments/assets/d0e72e78-f897-4eee-a160-edbc811c8daa" />

S3 bucket
<img width="1918" height="871" alt="image" src="https://github.com/user-attachments/assets/57c3fd04-b52b-4a7b-9628-1f1fd30a4fc0" />

SNS
<img width="1900" height="871" alt="image" src="https://github.com/user-attachments/assets/57790256-8d91-4535-b485-12d6ef3f8e93" />

Lambda
<img width="1909" height="845" alt="image" src="https://github.com/user-attachments/assets/f507624f-773e-4345-ac84-e080eeedec7d" />

Event Bridge
<img width="1913" height="871" alt="image" src="https://github.com/user-attachments/assets/6bd4e2f0-adbf-4348-bd98-8374130abbfe" />


