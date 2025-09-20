# 🌟 Daily Quote Automation with AWS  

The **Daily Quote Project** is an automated cloud-based application that delivers **motivational quotes** to users every day through **SMS and Email**.  
This project leverages multiple **AWS services** to handle storage, automation, notifications, and monitoring — all without manual intervention.  

---

## 🚀 AWS Services Used  

| Service         | Purpose                                                                 | Configuration / Details |
|-----------------|-------------------------------------------------------------------------|-------------------------|
| **📂 DynamoDB**    | Stores all motivational quotes.                                         | Table: `Quotes` <br> Partition Key: `id (String)` |
| **⚡ Lambda**      | Fetches random quote, stores in S3, and publishes to SNS.              | Function: `DailyQuoteFunction` <br> Runtime: Python 3.12 <br> Role: `LambdaQuoteRole` (DynamoDB, S3, SNS, CloudWatch access) |
| **🗄️ S3**          | Stores daily selected quote in JSON format for record-keeping.         | Bucket: `quote-of-the-day-hindavi` <br> Object: `quote-of-the-day.json` |
| **📢 SNS**         | Sends quotes to subscribers via SMS and Email.                         | Topic: `DailyQuoteTopic` <br> Subscribers: SMS & Email |
| **⏰ EventBridge** | Automates daily execution of the Lambda function.                      | Rule: `DailyQuoteRule` <br> Schedule: `rate(1 day)` / `cron(0 9 * * ? *)` |
| **📊 CloudWatch**  | Monitors executions, logs Lambda and SNS activities, tracks errors.    | Log Group: `/aws/lambda/DailyQuoteFunction` |

---

## 🖼️ Architecture Components  

### 1️⃣ DynamoDB Quotes Table  
👉 The **DynamoDB Quotes Table** stores all the motivational quotes.  
- Each item has an **`id`** attribute (partition key) and **quote text**.  
- The **Lambda function** fetches a random quote daily.  
✔ Ensures that every day a different quote is delivered.  

---

### 2️⃣ CloudWatch Logs  
👉 **CloudWatch Logs** track all executions of the `DailyQuoteFunction`.  
They include:  
- ⏱ Start and End times  
- 📩 SNS publish responses  
- ⚠️ Error messages / exceptions  

✔ Helps with monitoring and debugging.  

---

### 3️⃣ S3 Bucket  
👉 The **S3 bucket** stores the selected quote as **`quote-of-the-day.json`**.  
- 🗂 Maintains a **historical record** of delivered quotes  
- 🌐 Provides easy integration for future web apps  

---

### 4️⃣ SNS (Simple Notification Service)  
👉 **SNS** delivers the daily quote to all subscribers.  
- Supports **SMS and Email notifications**  
- Requires subscription confirmation ✅  
✔ Ensures reliable, scalable message delivery.  

---

### 5️⃣ Lambda Function  
👉 The **core logic** is in `DailyQuoteFunction`.  
Responsibilities:  
- 🎲 Fetch a random quote from DynamoDB  
- 📝 Save it to S3  
- 📢 Publish it via SNS  

✔ Runs automatically **once per day**.  

---

### 6️⃣ EventBridge Rule  
👉 The **EventBridge Rule** (`DailyQuoteRule`) triggers the Lambda.  
- ⏰ Runs every **24 hours** or at a custom cron time  
✔ Guarantees daily quote delivery **without manual work**.  

---

✨ With this setup, AWS services work together to create a **fully automated motivational quote delivery system** — efficient, scalable, and serverless.  
<img width="936" height="510" alt="image" src="https://github.com/user-attachments/assets/380b4082-df6f-4d04-8902-c2c0320b6723" />

