# Tutorial: Configuring Messages Arriving in an Amazon SQS Queue to Trigger an AWS Lambda Function<a name="sqs-configure-lambda-function-trigger"></a>

In this tutorial you'll learn how to configure an existing Amazon SQS queue to trigger an AWS Lambda function when new messages arrive in a queue\.

Lambda functions let you run code without provisioning or managing a server\. For example, you can configure a Lambda function to process messages from one queue while another queue acts as a *dead\-letter queue* for messages that your Lambda function can't process successfully\. When you resolve the issue, you can *redrive* the messages from the dead\-letter queue through the Lambda function\. For more information see [Amazon SQS Dead\-Letter Queues](sqs-dead-letter-queues.md) and also [What is AWS Lambda?](http://docs.aws.amazon.com/lambda/latest/dg/welcome.html) and [Using AWS Lambda with Amazon SQS](http://docs.aws.amazon.com/lambda/latest/dg/with-sqs.html) in the *AWS Lambda Developer Guide*\.

**Note**  
Your queue and Lambda function must be in the same AWS Region\.  
FIFO queues don't support Lambda function triggers\.  
You can associate only one queue with one or more Lambda functions\.  
You can't associate an [encrypted queue](sqs-server-side-encryption.md) that uses an AWS managed Customer Master Key for Amazon SQS with a Lambda function in a different AWS account\.

## Prerequisites<a name="configure-lambda-function-trigger-prerequisites"></a>

To configure Lambda function triggers using the console, you must ensure the following:
+ Your Amazon SQS role must include the following permissions:
  + `lambda:CreateEventSourceMapping`
  + `lambda:ListEventSourceMappings`
  + `lambda:ListFunction`
+ Your Lambda role must include the following permissions:
  + `sqs:ChangeMessageVisibility`
  + `sqs:DeleteMessage`
  + `sqs:GetQueueAttributes`
  + `sqs:ReceiveMessage`

For more information, see [Overview of Managing Access Permissions to Your Amazon Simple Queue Service Resource](sqs-overview-of-managing-access.md)\.

## AWS Management Console<a name="configure-lambda-function-trigger-console"></a>

1. Sign in to the [Amazon SQS console](https://console.aws.amazon.com/sqs/)\.

1. From the list of queues, choose the queue which you want to trigger a Lambda function\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/AWSSimpleQueueService/latest/SQSDeveloperGuide/images/sqs-tutorials-subscribe-queue-to-sns-topic-choose-queue.png)

1. From **Queue Actions**, select **Configure Trigger for Lambda Function**\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/AWSSimpleQueueService/latest/SQSDeveloperGuide/images/sqs-tutorials-configure-incoming-messages-trigger-lambda-function-drop-down.png)

1. 

   In the **Configure Incoming Messages to Trigger a Lambda Function** dialog box, do one of the following:
   + To use an existing Lambda function, **Select a Lambda Function** from the list\.
   + To create a new Lambda function in the AWS Lambda console, choose **Create New**\. For more information, see [Create a Simple Lambda Function](http://docs.aws.amazon.com/lambda/latest/dg/get-started-create-function.html) in the *AWS Lambda Developer Guide*\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/AWSSimpleQueueService/latest/SQSDeveloperGuide/images/sqs-tutorials-configure-incoming-messages-trigger-lambda-function-dialog-box.png)

1. Choose **Save**\.

1. In the **Lambda Function Trigger Configuration Result** dialog box, review the Lambda function that will be triggered by your Amazon SQS queue and choose **OK**\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/AWSSimpleQueueService/latest/SQSDeveloperGuide/images/sqs-tutorials-configure-incoming-messages-trigger-lambda-function-result.png)
**Note**  
It takes approximately 1 minute for the Lambda function to become associated with your queue\.

   The Lambda function and its status are displayed on the **Lambda Triggers** tab\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/AWSSimpleQueueService/latest/SQSDeveloperGuide/images/sqs-tutorials-configure-incoming-messages-trigger-lambda-function-association.png)
   + To verify the results of the configuration, you can [send a message to your queue](sqs-send-message.md) and then view the triggered Lambda function in the Lambda console\.
   + To delete the association between a Lambda function and your queue, choose ![\[Image NOT FOUND\]](http://docs.aws.amazon.com/AWSSimpleQueueService/latest/SQSDeveloperGuide/images/sqs-delete-queue-tag.png) next to a Lambda function ARN\.
 
 To create the Lambda for event source mapping though cloudformation-\
 LambdaStream:\
   Type: AWS::Lambda::EventSourceMapping\
   Properties:\
     BatchSize: 1\
     Enabled: True\
     EventSourceArn:\
       Fn::GetAtt: <SQSQueue>.Outputs.QueueARN\
     FunctionName:\
       Fn::GetAtt: <Lambda>.Outputs.LambdaArn\
