# Sales Analysis Report - AWS Lambda Project

## Overview
This project demonstrates the creation and configuration of AWS Lambda functions to generate and send daily sales analysis reports. It involves integrating AWS services such as IAM, SNS, CloudWatch Events, and Lambda. The project includes:

1. **Extracting data from a database**.
2. **Sending reports to users via email**.
3. **Automating report generation on a schedule using CloudWatch Events**.

---

## Features
- **IAM Role Management**: Assigning permissions to Lambda functions.
- **Lambda Layer Creation**: Satisfying external library dependencies.
- **Automated Scheduling**: Using CloudWatch Events for scheduled report generation.
- **Testing and Validation**: Unit testing the Lambda function and validating outputs.
- **Troubleshooting**: Leveraging CloudWatch logs for debugging.

---

## Setup Instructions

### Task 1: Prepare the Environment
1. **Navigate to the project files**:
   ```bash
   cd activity-files
   ls
   ```
2. Ensure the `salesAnalysisReport-v2.zip` file is present.

### Task 2: Retrieve IAM Role ARN
1. Open the IAM management console.
2. Search for the `salesAnalysisReportRole` role and copy its ARN.
3. Save the ARN in a text editor for later use.

### Task 3: Create the Lambda Function
1. Run the following command to create the Lambda function:
   ```bash
   aws lambda create-function \
   --function-name salesAnalysisReport \
   --runtime python3.9 \
   --zip-file fileb://salesAnalysisReport-v2.zip \
   --handler salesAnalysisReport.lambda_handler \
   --region <region> \
   --role <salesAnalysisReportRoleARN>
   ```
   Replace `<region>` and `<salesAnalysisReportRoleARN>` with appropriate values.
2. Verify the function creation via the returned JSON object.

### Task 4: Configure Environment Variables
1. Open the Lambda management console.
2. Navigate to **Functions > salesAnalysisReport**.
3. Under the **Configuration** tab, add the following environment variable:
   - **Key**: `topicARN`
   - **Value**: ARN of the `salesAnalysisReportTopic` SNS topic.
4. Save the changes.

### Task 5: Test the Lambda Function
1. Open the **Test** tab in the Lambda management console.
2. Create a new test event with the following details:
   - **Event name**: `SARTestEvent`
   - **Template**: `hello-world`
3. Run the test and verify the output:
   ```json
   {
       "statusCode": 200,
       "body": "\"Sale Analysis Report sent.\""
   }
   ```
4. Check your email inbox for the sales report.

### Task 6: Add a Trigger
1. In the Lambda **Function overview** panel, choose **Add trigger**.
2. Configure the trigger with the following:
   - **Trigger configuration**: `EventBridge (CloudWatch Events)`
   - **Rule name**: `salesAnalysisReportDailyTrigger`
   - **Rule type**: `Schedule expression`
   - **Schedule expression**: Example: `cron(0 20 ? * MON-SAT *)` (UTC time).
3. Save the trigger.
4. Verify the automated execution via email reports.

---

## Project Workflow

1. **IAM Role Assignment**: Assign permissions for Lambda to access SNS and other AWS resources.
2. **Lambda Function Creation**: Develop the `salesAnalysisReport` function.
3. **Environment Variable Configuration**: Define the SNS topic ARN.
4. **Testing**: Validate the function using test events.
5. **Trigger Addition**: Schedule the function with CloudWatch Events.
6. **Validation**: Check email for daily sales reports.

---

## Cron Expression Guide
- **General Syntax**: `cron(Minutes Hours Day-of-month Month Day-of-week Year)`
- **Production Schedule**: For daily execution Monday through Saturday at 8 PM UTC, use:
  ```
  cron(0 20 ? * MON-SAT *)
  ```

---

## Expected Output
### Lambda Execution Result:
```json
{
    "statusCode": 200,
    "body": "\"Sale Analysis Report sent.\""
}
```
### Email Report:
- Subject: `Daily Sales Analysis Report`
- Content: Detailed report of café sales for the day.

---

## Troubleshooting
- **Timeout Issues**: Increase the timeout in **Configuration > General Configuration**.
- **Error Logs**: Check CloudWatch logs for detailed error messages.

---

## Conclusion
Congratulations! You have successfully:
1. Created and tested Lambda functions.
2. Configured environment variables and permissions.
3. Automated daily tasks using CloudWatch Events.
4. Sent email notifications with SNS.

---

## End Lab
1. Choose **End Lab** at the top of the page.
2. Confirm by selecting **Yes**.
3. Wait for resources to terminate before closing the session.

---

## Resources
- [AWS Lambda Documentation](https://docs.aws.amazon.com/lambda/index.html)
- [CloudWatch Events](https://docs.aws.amazon.com/eventbridge/latest/userguide/what-is-cloudwatch-events.html)
- [IAM Roles](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_roles.html)
please refer screenshots for execution part 
