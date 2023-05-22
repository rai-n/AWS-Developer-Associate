## What is serverless?
* Server is a new paradigm where developers don't manage servers, but deploy code functions.
* Initially, serverless was FaaS (Function as a service), but now includes anything that is managed such as databases, messaging, storage, etc.
* Some serverless services include:
    - Lambda
    - DynamoDB
    - Cognito
    - API Gateway
    - S3
    - SNS & SQS
    - Kinesis Data Firehose
    - Aurora Serverless
    - Step Functions
    - Fargate

### For example
1. Users can get static website from an S3 bucket.
2. Users can log in using Cognito
3. Users can invoke REST API using API Gateway
4. API Gateway can invoke Lambda functions
6. Lambda function can store or retrieve data from DynamoDB

![](https://i.imgur.com/XHxkQSo.png)

### Another example - Thumbnail creation
1. S3 bucket has an event for new image uploaded to S3
2. The new image triggers an S3 event notification and a Lambda function to create a thumbnail will run
3. The new thumbnail could be pushed into S3 bucket
4. Some metadata for the image could be also inserted into DynamoDB

![](https://i.imgur.com/sT2cIWe.png)

### Another example - Serverless CRON job
1. EC2 instance has to be run continuously to trigger CRON jobs at certain times or intervals. This time could be wasted if the CRON jobs are not running.
2. Instead, a Cloudwatch Events EventBridge rule could be triggered to run timed jobs. E.g. trigger a Lambda function every hour.

## Lambda 
1. EC2:
    - Virtual servers in the cloud
    - Limited by RAM and CPU
    - Running continuously 
    - Scaling horizontally by adding / removing servers
2. Lambda:
    - Virtual functions - no servers to manage
    - Limited by time - short execution
    - Run on demand
    - Scaling is automated

### Lambda Pricing
1. Easy pricing
    - Pay per request and compute time
        - $0.20 million requests after free tier
        - duration in increment of 1ms 
    - Free tier of 1,000,000 Lambda requests and 400,000 GBs of compute time
        - == 400,000 seconds if function is 1GB RAM
        - == 3,200,000 seconds if function is 128 MB RAM
        - == After that $1.0 for 600,000 GBs
2. Integrated with whole suite of AWS services
3. Integrated with many programming languages
    - Node.js
    - Python
    - Java 8
    - C#
    - And more
    - Custom Runtime API
    - Has support for Lambda Container Image
        1. The container image itself must implement Lambda Runtime API
        2. ECS/ Fargate is preferred for running arbitrary Docker images
4. Easy monitoring through CloudWatch
5. Easy to get more resources per functions (up to 10GB of RAM)
6. Increasing RAM will also improve CPU and network
 
![](https://i.imgur.com/jDLuEY1.png)

### Creating Lambda function
1. Creating function from scratch

![](https://i.imgur.com/CUat0lW.png)

2. Running demo "Hello world" Lambda function in Python

```
import json
print('Loading function')
def lambda_handler(event, context):
    #print("Received event: " + json.dumps(event, indent=2))
    print("value1 = " + event['key1'])
    print("value2 = " + event['key2'])
    print("value3 = " + event['key3'])
    return event['key1']  # Echo back the first key value
    #raise Exception('Something went wrong')
```

3. Configuring test event which contains a JSON object mocks the structure of request emitted by AWS services to invoke this Lambda function

![](https://i.imgur.com/FYoUdBy.png)

4. Function ran against "demo-event" test event and returned the value 1 as well as printing the values in function log. Some extra details are also listed such as:
- Duration: 2.42 ms	
- Billed Duration: 3 ms	
- Memory Size: 128 MB
- Max Memory Used: 36 MB	
- Init Duration: 114.49 ms

![](https://i.imgur.com/PXUoYAG.png)

5. When the function is run again, the metrics are slightly better as there was no initiation duration since the function was ready to be used
- Duration: 1.55 ms
- Billed Duration: 2 ms
- Memory Size: 128 MB
- Max Memory Used: 37 MB

![](https://i.imgur.com/bbRuR3B.png)

### Important configuration
- Lambda has a maximum execution timeout of 15 minutes and minimum of 1 second
- Lambda can have memory from 128MB to 10240MB

![](https://i.imgur.com/WfqtGlX.png)

### Monitoring 
- You can see amount of invocations, duration as well as success rates in the monitoring tab which is powered by CloudWatch logs
- If you go to the "Logs" tab, you can also see a LogStream of all the invocations for that Lambda function 

![](https://i.imgur.com/plVTGss.png)
![](https://i.imgur.com/5U48x3s.png)

### Testing for errors
- If a Lambda function faces an error and raises an exception, this would raise an error in the execution result. The error message is returned, the type of error as well as a stack trace. This is also uploaded to CloudWatch logs

![](https://i.imgur.com/o9h68Nz.png)
![](https://i.imgur.com/Ags9euS.png)
![](https://i.imgur.com/PjgMs73.png)

- Lambda has access to write to CloudWatch logs or create LogStream is due to those permissions attached via an IAM role

![](https://i.imgur.com/WYzxLce.png)

### Synchronous invocations
#### Overview
AWS Lambda also supports synchronous invocation of a Lambda function. In this invocation mode, the calling application waits for the function to complete processing and returns the response. This can be useful for workloads that require an immediate response or when you want to process events in real-time.

#### Use Case
An example use case for synchronous invocation of a Lambda function is when you want to validate user input in a web or mobile application. The application can send the user input to a Lambda function for validation and wait for the response before proceeding.

#### Tips
1. You can set up custom error handling for synchronous invocations by configuring the function’s error handling settings. This allows you to control how errors are returned to the calling application.
2. You can also monitor the performance of synchronous invocations using Amazon CloudWatch. This allows you to track metrics such as invocation count, duration, and errors.

- Synchronous invocation is when you use the CLI, SDK, API Gateway, ALB. It means that we are waiting for the results
- Errors handling must happen on client side. For example, if the Lambda function fails after invoking on the console, you have to retry, exponential backoff, etc
- Synchronous invocations can be:
    1. User invoked:
        - ELB (ALB)
        - API Gateway
        - CloudFront (Lambda@Edge)
        - S3 Batch
    2. Service invoked: 
        - Cognito
        - Step functions
    3. Other:
        - Lex
        - Alexa
        Kinesis Data Firehose

![](https://i.imgur.com/RYppYfu.png)

### Running "hello world" via CloudShell

```
aws lambda invoke --function-name demo-lambda --cli-binary-format raw-in-base64-out --payload '{"key1": "value1", "key2": "value2", "key3": "value3" }' response.json
```

- Using `cat response.json`, the correct "value1" response is printed. 

![](https://i.imgur.com/sINv4MA.png)

### Lambda Integration with ALB
- To expose a Lambda function as an HTTP(s) endpoint, you can use ALB or API gateway
- The Lambda function must be registered in a target group

![](https://i.imgur.com/2hdqhe4.png)

#### ALB to Lambda: HTTP to JSON 
- HTTP to JSON
![](https://i.imgur.com/XniErJk.png)

##### Lambda to ALB: JSON to HTTP
- JSON to HTTP
![](https://i.imgur.com/heh97YR.png)

### ALB Multi-Header values
- ALB can support multi header values (ALB setting)
- When you enable multi-value headers, HTTP headers and query string parameters that are sent with multiple values are shown as arrays within the AWS Lambda event and response objects
- This lets you pass query strings with same names

![](https://i.imgur.com/VOlxD6k.png)

#### Lambda & ALB 
1. I created a python based "Author from scratch" Lambda function

![](https://i.imgur.com/RTlZMLm.png)

2. I then created an internet facing ALB that is available on eu-west-2a, eu-west-2b and eu-west-2c

![](https://i.imgur.com/1M6agXK.png)

3. I added a security group on the ALB to allow all inbound HTTP traffic so that clients can access it. I also added an HTTP listener on port 80 and added a target group for the lambda function so that the Lambda function is invoked

![](https://i.imgur.com/Oa8NS69.png)

4. I created the target group for the Lambda function

![](https://i.imgur.com/fwowPQP.png)

5. Now when I go to the ALB's DNS name, a file is downloaded with contents "Hello from Lambda!" which is due to the return statement in the Lambda function

![](https://i.imgur.com/JGpMVYk.png)
![](https://i.imgur.com/fEjVa0U.png)

7. Instead of returning a single JSON, if you use the the [response document format](https://docs.aws.amazon.com/lambda/latest/dg/services-alb.html), ELB converts the document to an HTTP success or error response and return it to the user

```
{
    "statusCode": 200,
    "statusDescription": "200 OK",
    "isBase64Encoded": False,
    "headers": {
        "Content-Type": "text/html"
    },
    "body": "<h1>Hello from Lambda!</h1>"
}
```

Now instead of downloading a file, a webpage with the heading is loaded. The `headers.Content-Type` being `text/html` is what causes the body to be parsed as an HTML document

![](https://i.imgur.com/mSEROEO.png)

### Asynchronous invocations 
#### Overview
AWS Lambda supports asynchronous invocation of a Lambda function. In this invocation mode, Lambda sends the event to a function and immediately returns a response without waiting for the function to complete processing. This can be useful for workloads that are not time-sensitive or when you want to process events in the background.

#### Use Case
An example use case for asynchronous invocation of a Lambda function is when you want to process events from an Amazon S3 bucket. When an object is created or deleted in the bucket, an event can be triggered and sent to a Lambda function for processing. The function can then perform actions such as resizing an image or generating a thumbnail.

#### Tips
1. You can configure a dead-letter queue (DLQ) for your Lambda function to handle failed asynchronous invocations. The DLQ can be an Amazon SQS queue or an Amazon SNS topic where the failed event is sent for further processing.
2. You can also set up retry attempts and maximum age of the event for asynchronous invocations. This allows you to control how many times Lambda retries the invocation and how long the event can remain in the queue before it is discarded.

![](https://i.imgur.com/6ftwP9G.png)

- Asynchronous invocations can be:
1. Simple Storage Servers notification
2. SNS 
3. CloudWatch Events / EventBridge
4. CodeCommit (Trigger when new branch, new tag, new push)
5. CodePipeline (Invoke a Lambda function during the pipeline, Lambda must callback)
6. CloudWatch logs (log processing)
7. Simple Email Service
8. CloudFormation
9. Config 
10. IoT/ IoT Events

#### Example 
A dead-letter queue (DLQ) can be used to capture failed asynchronous invocations of a Lambda function. When you configure an Amazon SQS queue as the DLQ for your Lambda function, any failed asynchronous invocation will be sent to the SQS queue for further processing.

Sending error messages from a Lambda function’s DLQ to an SQS queue allows you to retain and analyze the failed events. This can help you identify and troubleshoot issues with your function or the event source. You can also set up additional processing or notifications based on the contents of the failed events in the SQS queue.

1. Update python code to raise an exception

![](https://i.imgur.com/fZBeDqf.png)

2. Edit the asynchronous configuration. I created a standard configuration SQS queue and added it to the Lambda function's dead-letter queue

![](https://i.imgur.com/eaJ9RIv.png)
![](https://i.imgur.com/455IP8Z.png)

3. There is an error that the lambda function does not have permission to run SendMessage on SQS. I opened the execution role for the lambda function and added the "AmazonSQSFullAccess" policy

![](https://i.imgur.com/jS6jo3n.png)
![](https://i.imgur.com/WbRDF8A.png)
![](https://i.imgur.com/JvILBg7.png)

4. To test the asynchronous invocation I used:

```
aws lambda invoke --function-name lambda-alb --cli-binary-format raw-in-base64-out --payload '{"key1": "value1"}' --invocation-type Event --region eu-west-2 response.json
```

5. As the lambda function is expected to raise an exception, after another 1 attempt, the error is sent to dead-letter queue for further processing

![](https://i.imgur.com/9oAdrZ7.png)
![](https://i.imgur.com/2JLfAWT.png)

### CloudWatch Events / EventBridge
Amazon CloudWatch Events (now called Amazon EventBridge) is a service that enables you to set up rules that automatically trigger actions in response to specific events that occur in your AWS environment. You can use EventBridge to create rules that match events from supported AWS services, and route them to one or more targets, such as AWS Lambda functions, Amazon SNS topics, or Amazon SQS queues.

#### Use Case
An example use case for CloudWatch Events/EventBridge is when you want to automate the response to specific changes in your AWS environment. For instance, you can set up a rule that triggers a Lambda function whenever an EC2 instance changes state, or when an object is uploaded to an S3 bucket. This allows you to automate tasks such as starting or stopping instances, or processing uploaded files.

#### Tips
1. You can use Event Patterns to specify which events should trigger your rules. Event Patterns allow you to match events based on their content, so you can create rules that are triggered only by specific events.
2. You can also use Scheduled Events to trigger rules at specific times. This allows you to set up recurring tasks, such as daily backups or weekly reports.
3. You can monitor the performance of your rules using CloudWatch Metrics. This allows you to track metrics such as the number of triggered rules, the number of matched events, and the number of failed invocations.

#### Examples
1. CRON or Rate with EventBridge rule to trigger Lambda function to perform a task every hour
2. CodePipeline EventBridge rule to trigger Lambda function to perform a task when state changes occur

### EventBridge rule
Amazon EventBridge allows you to set up rules that automatically trigger actions in response to specific events. One of the targets you can use for your rules is an AWS Lambda function. This allows you to automatically invoke a Lambda function whenever a specific event occurs in your AWS environment.

#### Use Case
An example use case for using EventBridge rules with Lambda is when you want to automate the response to specific changes in your AWS environment. For instance, you can set up a rule that triggers a Lambda function whenever an EC2 instance changes state, or when an object is uploaded to an S3 bucket. The function can then perform actions such as starting or stopping instances, or processing uploaded files.

#### Tips
1. You can use Event Patterns to specify which events should trigger your rules. Event Patterns allow you to match events based on their content, so you can create rules that are triggered only by specific events.
2. You can also set up multiple targets for the same rule. This allows you to invoke multiple Lambda functions, or send notifications to multiple targets, whenever the specified event occurs.
3. You can monitor the performance of your rules and Lambda functions using CloudWatch Metrics. This allows you to track metrics such as the number of triggered rules, the number of matched events, and the number of failed invocations.

1. Creating a scheduled rule

![](https://i.imgur.com/Gq8RDul.png)

2. Setting schedule to run every minute

![](https://i.imgur.com/dGAiFsQ.png)

3. Setting the rule target to Lambda function and selecting the `demo-eventbridge` function

![](https://i.imgur.com/18HP5Cm.png)
![](https://i.imgur.com/oQ8iOIn.png)

4. Attaching the EventBridge rule to the Lambda function created the resourced base policy which allows Principle (Event BridgeService) invoke the Lambda function if the `InvokeLambdaEveryMinute` rule invokes it

![](https://i.imgur.com/c1YQR4e.png)

5. Each minute, the event is printed and can be seen in the CloudWatch event logs

![](https://i.imgur.com/ERvNcKJ.png)

### S3 Event notifications
Amazon S3 event notifications allow you to receive notifications when certain events occur in your S3 bucket. You can set up notifications to be sent to an Amazon SNS topic, an Amazon SQS queue, or an AWS Lambda function whenever specific events, such as object creation or deletion, occur in your bucket.

#### Use Case
An example use case for S3 event notifications is when you want to automatically process files that are uploaded to your S3 bucket. For instance, you can set up a notification that triggers a Lambda function whenever a new object is created in your bucket. The function can then perform actions such as resizing an image or generating a thumbnail. Events such as the following can be used:
    - S3:ObjectCreated, S3:ObjectRemoved
    - S3:ObjectRestore, S3:ObjectReplication
- S3 event notifications typically deliver events in seconds but sometimes can take a minute or longer.
- If two writes are made to a single non-versioned object at the same time, it is possible that only a single event notification will be sent
- **Make sure to** enable versioning on your bucket if you want to ensure that an event notification is sent for every successful write

![](https://i.imgur.com/iR3cYAx.png)

#### S3 saving metadata
1. Create a standard S3 bucket 

![](https://i.imgur.com/lMpSXSl.png)

2. In the event notification tab, you can press on "Create event notification" to create an event which invokes a Lambda function

![](https://i.imgur.com/de45Gcn.png)

3. Create an event notification called "InvokeLambda" with event type for "All objects create events"

![](https://i.imgur.com/N84UmPg.png)

4. In the destination section, choose to run a Lambda function based on S3 event and select `lambda-s3`

![](https://i.imgur.com/T48EwQZ.png)
![](https://i.imgur.com/f6E6GaM.png)

5. Upload an image to test the S3 event notification 

![](https://i.imgur.com/eH53tu1.png)

6. In the CloudWatch logs, the Lambda function invocation made by S3 can be seen containing `eventSource` as `aws:s3`, bucket name `{'name': 's3-event-event'}`, arn, object key, size, and more. This metadata is enough for the Lambda function to do S3.GetObject API call and get the object that was uploaded.

![](https://i.imgur.com/rEy6sOg.png)