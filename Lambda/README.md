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
    - Free tier of 1,000,000 Lambda requests and 400,000 GBs of compute time
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
 