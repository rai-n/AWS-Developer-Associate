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

