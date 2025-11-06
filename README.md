# AWS-Serverless-Microservices-Project
This project demonstrates a serverless architecture where microservices are deployed using AWS Lambda functions. The system exposes REST APIs via Amazon API Gateway, stores data in Amazon DynamoDB, and leverages AWS Step Functions Power Tuning to optimize Lambda performance and cost.
#
### Project Design

![Project Design](images/project-design.svg)
#

### Concept of Serverless Microservices

Microservices: Small, independent services that each handle a specific piece of functionality.
Serverless: You write and deploy code without provisioning or managing servers. AWS automatically handles scaling, availability, and fault tolerance.
When combined, you get modular, scalable applications that are cheaper and easier to maintain.

#
### AWS Services Used

| Function    | AWS Service       | Description |
| :---:       |     :---:        |   :---:     |
|Compute      |AWS Lambda         |Event-driven compute service and allows users to run code without provisioning or managing servers|
|API Gateway  |Amazon API Gateway |Service for creating, publishing, maintaining, monitoring, and securing REST, HTTP, and WebSocket APIs at any scale|
|Data Storage |Amazon DynamoDB    |NoSQL database for scalable and fast , predictable performance storage
|Application Integration|AWS Step Functions |Service that uses visual workflows, called state machines, to coordinate the components of microservices and distributed applications

#
### AWS Lambda Power Tuning 
It is powered by AWS Step Functions tool designed to help optimize AWS Lambda functions for cost and/or performance by identifying the best memory and CPU configuration
i. Lambda function with multiple power configurations ( from 128 MB to 10 GB)
ii. CPU and network scale linearly with memory
iii. Choosing too little memory -> slow function -> higher duration cost
iv. Choosing too much memory -> faster but more expensive per ms
Power Tuning finds the perfect spot between speed and cost.

### How It Works
AWS provides a Step Functions state machine to automatically run your Lambda with multiple memory configurations and record:
1. Execution duration
2. Cost per invocation
3. Success/failure

It then outputs a visualization showing:
1. Fastest configuration
2. Cheapest configuration
3. Best balance (Speed vs Cost)

### Steps to Use AWS Lambda Power Tuning
**Step 1: Deploy the Power Tuning Step Function**

AWS has a pre-built Step Function:
Go to Serverless Application Repository -> Click Available Applications type “power” and 
Click checkbox “Show apps that create custom IAM roles or resource policies”

![AWS Lambda Power Tuning](images/aws-lambda-power-tuning.jpg)

Select "aws-lambda-power-tuning"
Scroll down, keep everything as it is
Click checkbox "I acknowledge that this app creates custom IAM roles"
Click "Deploy" -> This will create:
* Step Function
* IAM role for execution

**Step 2: Configure a Run**
* Open Step Functions in AWS Console.
* Click "Start execution" on the Lambda Power Tuning state machine.
* Provide JSON input:

```
{
  "lambdaARN": "your-lambda-function-arn",
  "powerValues": [128, 256, 512, 1024],
  "num": 10,
  "payload": { "operation": "list",
    "tableName": "your-dynamoDB-table-name",
    "payload": {}
},
  "parallelInvocation": true,
  "strategy": "cost"
}

```
**Step 3: Run and Monitor**

The Step Function will invoke your Lambda N times at each memory setting.
It collects:  Duration, Cost per invocation, Failures (if any)

**Step 4:Graphical Visualization**
* X-axis: Memory (MB)
* Y-axis: Cost / Duration

![Lamda Power Tuning ](images/aws-lambda-power-tuning-result.jpg)

From the graph, able to pick Cheapest -> minimal cost or Fastest -> minimal duration on derived result can get best balanced -> reasonable cost + speed

**Step 5: Applied the Optimal Memory Setting for Lambda Function**
![Lambda Fubction Memory Setting](images/lambda-function-memory-setting.jpg)
#

### Performance Metrics on simulation of concurrent users and requests using Postman’s Collection Runner 
##Performance Analysis and Results


