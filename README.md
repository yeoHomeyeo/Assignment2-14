# Assignment2-14
Serverless Architecture 2 : Service Messaging and Event Services


### Does SNS guarantee exactly once delivery to subscribers?
- No. SNS provides at-least-once delivery, meaning that messages may be delivered to subscribers more than once
- Subscribers must be designed to handle duplicate messages idempotently (i.e., processing the same message multiple times should not cause unintended side effects)
- Source: https://aws.amazon.com/sns/faqs/

### What is the purpose of the Dead-letter Queue (DLQ)? This is a feature available to SQS/SNS/EventBridge.
- The purpose of DLQ is to capture messages or events that cannot be processed successfully after multiple attempts.
- DLQ is useful for these reasons:
  - Error Handling: Isolating failed messages for debugging and analysis
  - Preventing Message Loss: Ensuring that messages are not lost due to processing failures
  - Retry Logic: Allowing systems to retry processing messages later or manually intervene
  - Source: https://docs.aws.amazon.com/AWSSimpleQueueService/latest/SQSDeveloperGuide/sqs-dead-letter-queues.html

### How would you enable a notification to your email when messages are added to the DLQ?
- Create an SNS Topic
  - On Amazon SNS console, create a new topic
  - Subscribe a target email address to the topic.
  - Target will receive a confirmation email
  - Confirm the subscription
    
- Set up a CloudWatch Alarm
  - On Amazon CloudWatch console, create a new alarm for DLQ
  - Select the metric for the DLQ (e.g., ApproximateNumberOfMessagesVisible for SQS DLQ)
  - Set the threshold to trigger the alarm when the number of messages in the DLQ is greater than "X"
  - Configure the alarm to send a notification to the SNS topic
    
- Configure the DLQ
  - Ensure that the SNS subscription has a DLQ configured
  - Set the Maximum Receives or Retry Policy to move messages to the DLQ after a specified number of failures
    
- Test the Setup
  - Add a message to the main queue that will fail processing and be moved to the DLQ
  - Verify that the CloudWatch alarm triggers and sends an email notification via SNS
    
- Source: https://docs.aws.amazon.com/AmazonCloudWatch/latest/monitoring/AlarmThatSendsEmail.html
