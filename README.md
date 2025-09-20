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

📊 Monitoring
CloudWatch Logs capture Lambda start/end time, SNS responses, and errors.
This can be extended with CloudWatch Alarms to send alerts on failures.
📸 Screenshots
1. DynamoDB Quotes Table
<img width="1904" height="924" alt="DynamoDB Table" src="https://github.com/user-attachments/assets/a70d70c1-9a11-4f0c-bda1-7ebbb68f45ca" /> 👉 This table stores all the motivational quotes. Each item has an `id` as the partition key and the quote text. Lambda fetches a random quote from this table daily.
2. CloudWatch Logs
<img width="1917" height="940" alt="CloudWatch Logs" src="https://github.com/user-attachments/assets/d0e72e78-f897-4eee-a160-edbc811c8daa" /> 👉 CloudWatch records all executions of the **DailyQuoteFunction** Lambda. It logs start/end times, SNS publish responses, and any errors that occur during execution.
3. S3 Bucket
<img width="1918" height="871" alt="S3 Bucket" src="https://github.com/user-attachments/assets/57c3fd04-b52b-4a7b-9628-1f1fd30a4fc0" /> 👉 The daily selected quote is stored in **S3** as `quote-of-the-day.json`. This keeps a historical record of all delivered quotes and allows integration with future web apps.
4. SNS (Simple Notification Service)
<img width="1900" height="871" alt="SNS" src="https://github.com/user-attachments/assets/57790256-8d91-4535-b485-12d6ef3f8e93" /> 👉 SNS delivers the daily quote to all subscribers via SMS and Email. Each subscriber must confirm their subscription before receiving notifications.
5. Lambda Function
<img width="1909" height="845" alt="Lambda" src="https://github.com/user-attachments/assets/f507624f-773e-4345-ac84-e080eeedec7d" /> 👉 The `DailyQuoteFunction` Lambda is the **core logic** of the project. It fetches a random quote from DynamoDB, saves it to S3, and publishes it to SNS. It runs automatically once per day.
6. EventBridge Rule
<img width="1913" height="871" alt="EventBridge" src="https://github.com/user-attachments/assets/6bd4e2f0-adbf-4348-bd98-8374130abbfe" /> 👉 EventBridge schedules the Lambda function to run daily. The rule `DailyQuoteRule` is configured to trigger the Lambda once every 24 hours (or at a specific cron time). ```
