# NBA Game Day Notifications / Sports Alerts System

## **Project Overview**
This project fetches NBA game data for the current date (adjusted to Central Time), formats the data into readable messages, and publishes updates to an SNS topic for distribution (e.g., to emails or SMS). It leverages **Amazon SNS**, **AWS Lambda and Python**, **Amazon EvenBridge** and **NBA APIs** to provide sports fans with up-to-date game information. The project demonstrates cloud computing principles and efficient notification mechanisms.

---

## **Features**
- Fetches live NBA game scores using an external API.
- Sends formatted score updates to subscribers via SMS/Email using Amazon SNS.
- Scheduled automation utilizing cron jobs for regular updates using Amazon EventBridge.
- Designed and implemented with security in mind, following the principle of least privilege for IAM roles.

## **Prerequisites**
- Free account with subscription and API Key at [sportsdata.io](https://sportsdata.io/)
- Personal AWS account with basic understanding of AWS and Python

---

## **Technical Architecture**
<img width="957" alt="Screenshot 2025-01-09 at 9 30 53 PM" src="https://github.com/user-attachments/assets/6c8c85c1-7a38-48cb-9efb-054dcbaf2db6" />


---


## **Technologies**
- **Cloud Provider**: AWS
- **Core Services**: SNS, Lambda, EventBridge
- **External API**: NBA Game API (SportsData.io)
- **Programming Language**: Python 3.x
- **IAM Security**:
  - Least privilege policies for Lambda, SNS, and EventBridge.

---

## **Project Structure**
```bash
game-day-notifications/
├── src/
│   ├── gd_notifications.py          # Main Lambda function code
├── policies/
│   ├── gb_sns_policy.json           # SNS publishing permissions
│   ├── gd_eventbridge_policy.json   # EventBridge to Lambda permissions
│   └── gd_lambda_policy.json        # Lambda execution role permissions
├── .gitignore
└── README.md                        # Project documentation
```

## **Setup Instructions**

### **Clone the Repository**
```bash
git clone https://github.com/ifeanyiro9/game-day-notifications.git
cd game-day-notifications
```

### **Create an SNS Topic**
1. Open the AWS Management Console.
2. Navigate to the SNS service.
3. Click Create Topic and select Standard as the topic type.
4. Name the topic (e.g., gd_topic) and note the ARN.
5. Click Create Topic.

![Screenshot 2025-01-09 at 9 35 15 PM](https://github.com/user-attachments/assets/79c6a04b-c263-4d68-85df-55be76a44b38)

   

### **Add Subscriptions to the SNS Topic**
1. After creating the topic, click on the topic name from the list.
2. Navigate to the Subscriptions tab and click Create subscription.
3. Select a Protocol:
- For Email:
  - Choose Email.
  - Enter a valid email address.
- For SMS (phone number):
  - Choose SMS.
  - Enter a valid phone number in international format (e.g., +1234567890).
  - I chose Email.
 
  <img width="735" alt="Screenshot 2025-01-09 at 9 27 47 PM" src="https://github.com/user-attachments/assets/c6cb6bb6-b870-4fbb-8e12-0d7bb99eae19" />


4. Click Create Subscription.
5. If you added an Email subscription:
- Check the inbox of the provided email address.
- Confirm the subscription by clicking the confirmation link in the email.
6. For SMS, the subscription will be immediately active after creation.

![Screenshot 2025-01-09 at 9 37 00 PM](https://github.com/user-attachments/assets/da29fad4-01e0-4ef2-a8bf-be8ce8b161d2)


### **Create the SNS Publish Policy**
1. Open the IAM service in the AWS Management Console.
2. Navigate to Policies → Create Policy.
3. Click JSON and paste the JSON policy from gd_sns_policy.json file
4. Replace REGION and ACCOUNT_ID with your AWS region and account ID.
5. Click Next: Tags (you can skip adding tags).
6. Click Next: Review.
7. Enter a name for the policy (e.g., gd_sns_policy).
8. Review and click Create Policy.

![Screenshot 2025-01-09 at 9 40 35 PM](https://github.com/user-attachments/assets/e990d500-27c5-4957-9278-ad6e2afa4ecf)


### **Create an IAM Role for Lambda**
1. Open the IAM service in the AWS Management Console.
2. Click Roles → Create Role.
3. Select AWS Service and choose Lambda.
4. Attach the following policies:
- SNS Publish Policy (gd_sns_policy) (created in the previous step).
- Lambda Basic Execution Role (AWSLambdaBasicExecutionRole) (an AWS managed policy).
5. Click Next: Tags (you can skip adding tags).
6. Click Next: Review.
7. Enter a name for the role (e.g., gd_role).
8. Review and click Create Role.
9. Copy and save the ARN of the role for use in the Lambda function.

![Screenshot 2025-01-09 at 9 53 55 PM](https://github.com/user-attachments/assets/3bd74b95-3a10-4b3b-aecf-06fd504779a9)


### **Deploy the Lambda Function**
1. Open the AWS Management Console and navigate to the Lambda service.
2. Click Create Function.
3. Select Author from Scratch.
4. Enter a function name (e.g., gd_notifications).
5. Choose Python 3.x as the runtime.
6. Assign the IAM role created earlier (gd_role) to the function.
7. Under the Function Code section:
- Copy the content of the src/gd_notifications.py file from the repository.
- Paste it into the inline code editor.
8. Under the Environment Variables section, add the following:
- NBA_API_KEY: your NBA API key.
- SNS_TOPIC_ARN: the ARN of the SNS topic created earlier.
9. Click Create Function.


![Screenshot 2025-01-09 at 9 57 35 PM](https://github.com/user-attachments/assets/01418341-26e0-4013-86f6-3c72d5bd6d61)
This script Fetches NBA game data for the current date (adjusted to Central Time), processes and transforms the data into user-friendly messages, and publishes the updates to an Amazon SNS topic for distribution via email or SMS.


### **Set Up Automation with Eventbridge**
1. Navigate to the Eventbridge service in the AWS Management Console.
2. Go to Rules → Create Rule.
3. Select Event Source: Schedule.
4. Set the cron schedule for when you want updates (e.g., hourly).
5. Under Targets, select the Lambda function (gd_notifications) and save the rule.

![Screenshot 2025-01-09 at 10 03 39 PM](https://github.com/user-attachments/assets/45f678ed-8ea1-45d4-aa87-39caa0b3761c)



### **Test the System**
1. Open the Lambda function in the AWS Management Console.
2. Create a test event to simulate execution.
3. Run the function and check CloudWatch Logs for errors.
4. Verify that SMS notifications are sent to the subscribed users.

<img width="373" alt="Screenshot 2025-01-09 at 9 01 54 PM" src="https://github.com/user-attachments/assets/7f295a16-45df-466b-b8d7-dbf0797c8c43" />



### **What We Learned**
1. Designing and depolying a notification system via AWS SNS and Lambda.
2. Securing AWS services with least privilege IAM policies.
3. Automating scheduled workflows via cron jobs using EventBridge.
4. Integrating external APIs into cloud-based workflows.

### **Challenges**
1. Lambda function initially faiiled to run due to time out
2. Navigated to the Lambda function 'gd_notifications' in the AWS Console
3. Select the 'Configuration' tab
4. Click on 'General configuration' in the left sidebar
5. Click the 'Edit' button
6. Increased the 'Timeout' value from 3 seconds to 30 seconds, to allow more time to execute my function due to complexity.
7. Click 'Save' to apply the changes
8. Resolved



### **Future Enhancements**
1. Add NFL score alerts for extended functionality.
2. Store user preferences (teams, game types) in DynamoDB for personalized alerts.
3. Implement a web UI
