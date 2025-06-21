# Aws
Cloud Documentation

# AWS Lambda
AWS Lambda is a `serverless compute service` offered by Amazon Web Services (AWS). It lets you run code without provisioning or managing servers. <br>
You just write your function code, `upload it as a zip file` or `container image`, and AWS handles the rest. Lambda automatically allocates compute execution power and runs your code based on the incoming request or event, for any scale of traffic.

## Lambda functions and function handlers
> Lambda function
- Lambda function is a piece of code (+ configuration), that does one specific task. <br>
- It runs only when triggered by some event (like, a user clicking a button on a website or a file being uploaded to S3 bucket etc.)

> Function handler
- The function handler is the entry point of your lambda function - it’s the method AWS calls when your Lambda is triggered.
-  It takes the event and context -> does the work -> and returns a response. Your function runs until the handler returns a response, exits, or times out.

| Parameter   | Description                                                                                                                        |
| ----------- | ---------------------------------------------------------------------------------------------------------------------------------- |
| **event**   | The **input data** passed to your Lambda. This can be from API Gateway, S3, SNS, DynamoDB, Step Functions, etc. It's usually JSON. |
| **context** | Metadata (eg. request Id, timeout, memory etc) about the invocation, function, and environment. Provided by AWS to your function at runtime. |

`event`
This is the trigger input, and varies based on what invoked the Lambda:

| Invocation Source | Sample `event`                          |
| ----------------- | --------------------------------------- |
| API Gateway       | HTTP method, headers, body, etc.        |
| S3 Event          | Bucket name, object key, event type     |
| DynamoDB Stream   | New/old item image, change type         |
| Step Functions    | Whatever input you passed into the step |

`context`
This gives information about the Lambda execution environment. Common context fields:
| Property                         | Description                       |
| -------------------------------- | --------------------------------- |
| `function_name`                  | Name of the Lambda function       |
| `memory_limit_in_mb`             | Memory allocated                  |
| `aws_request_id`                 | Unique ID per invocation          |
| `get_remaining_time_in_millis()` | Time left before Lambda times out |
| `log_stream_name`                | CloudWatch log stream             |


## How Lambda works - workflow?
- `Trigger:` A Lambda function is invoked by an event (like an HTTP request via API Gateway, a file upload in S3, or a scheduled cron job via EventBridge).
- `Execution:` AWS provisions a container, loads the runtime (e.g., Python, Node.js), and runs your function code.
- `Scale:` AWS automatically scales the number of function instances based on demand.
- `Shutdown:` The container is frozen (or terminated) after the execution finishes (for cost savings).

## How to test your Lambda function locally (using AWS SAM CLI & Docker)
1. Create a SAM Template (It defines your Lambda function and its configuration in a way that AWS SAM CLI understands)
Create a file named `template.yaml` in your project directory
```
AWSTemplateFormatVersion: '2010-09-09'
Transform: AWS::Serverless-2016-10-31
Resources:
  MyFunction:
    Type: AWS::Serverless::Function
    Properties:
      Handler: main
      Runtime: go1.x
      CodeUri: .
      Events:
        ApiEvent:
          Type: Api
          Properties:
            Path: /test
            Method: post
```
2. Build the Lambda
```
sam build
```

3. Create a Test Event (event.json)
```
{
  "firstName": "John",
  "lastName": "Doe",
  "age": 30
}
```

4. Invoke the Lambda Locally
```
sam local invoke MyFunction -e event.json
```

5. [Optional] Start a Local API and test with HTTP requests
```
sam local start-api
```
Then send a POST request to http://127.0.0.1:3000/test with your JSON body.

### Automate building, testing, cleaning, deploying using `make` tool
`make` tool can be used to automate the building, testing, cleaning, deploying tasks.

- Install `make` tool
- Create a `Makefile` (configuration file used by `make` tool) in your project directory with the following content:
```
APP_NAME = LambdaFunctionUsingGo
LAMBDA_PAYLOAD = resources/event.json

//.PHONY is a special built-in command (or target) that tells `make` that This target name is not a filename — always run the associated commands, even if a file or directory with the same name exists. Without .PHONY, make checks whether a file or directory with the same name as your target exists, and if it does, it skips running that target. <br>
.PHONY: build clean local-invoke

build:
	@echo "Building Lambda..."
	sam build

clean:
	PowerShell -Command "Remove-Item -Recurse -Force .aws-sam\\build"

run:
# sam local invoke $(APP_NAME) --event $(LAMBDA_PAYLOAD)
	sam local invoke --docker-network lambda-mongo-net $(APP_NAME) --event $(LAMBDA_PAYLOAD)
```

- You can now avoid manually typing long commands and you can chain tasks together (Eg. clean -> build -> deploy) <br>
Example, now you can just run below simple commands:

| Command             | What it does                              |
| ------------------- | ----------------------------------------- |
| `make build`        | Cleans and builds with `sam build`        |
| `make clean`        | Deletes `.aws-sam/build`                  |
| `make run`          | Runs the Lambda locally with `event.json` |

# Important Notes
When you use tools like the AWS SAM CLI (sam local invoke) or AWS Lambda Extensions for local testing, they automatically run your Lambda function inside a Docker container to simulate the Lambda environment.

This means:
- You need Docker installed and running on your machine.
- Your Lambda code is running inside a Docker container, even if you didn't build the image yourself.
- You will need to create `docker network` & run containers on that network, if you want to connect to other services (like a local database) from your Lambda function.

# Amazon EKS (Elastic Kubernetes Service)
Amazon EKS (Elastic Kubernetes Service) is a managed Kubernetes service provided by AWS.

🌱 In simpler terms:
- Kubernetes is a system that helps you run and manage containerized applications.
- Amazon EKS takes care of setting up and managing Kubernetes for you, so you don’t have to install it yourself.
- EKS runs your Kubernetes control plane (brain of Kubernetes) on AWS in a secure and highly available way.

| Control Plane         | Worker Node           |
| --------------------- | --------------------- |
| Makes decisions       | Executes them         |
| Runs Kubernetes brain | Runs your apps (pods) |
| Managed by AWS in EKS | Can be EC2 or Fargate |

- Even though EKS manages the control plane, your application pods still need to run somewhere. That somewhere is usually `EC2`. It can also be 
  `EC2` runs your application pods as worker nodes.

- When you create an EKS cluster, you usually set up:
> EKS Control Plane – Fully managed by AWS.
> Node Group (worker nodes) – This is a group of EC2 instances that run the Kubernetes worker nodes.

<img width="195" alt="image" src="https://github.com/user-attachments/assets/1d385444-fb31-494a-b0fe-80aa36689abf" />


Example flow: <br>
Let’s say you deploy a microservice to your EKS cluster:
- EKS sees a Deployment manifest and decides to run 3 pods.
- It looks at available EC2 worker nodes in the cluster.
- It places those pods on the EC2s with enough resources.
- The containers run inside pods on those EC2 instances.

You can view your EKS nodes (EC2s) in the AWS Console:
1. Go to EC2 → Instances
2. Filter by tag: eks:cluster-name=<your-cluster-name>

or, in terminal
`kubectl get nodes -o wide`

## ECS vs EKS?

# Amazon EC2 (Elastic Compute Cloud)
Amazon EC2 (Elastic Compute Cloud) is a service that provides virtual machines or server (called EC2 instances) in the cloud. <br>

You can use EC2 to:
- Host web servers
- Run backend services
- Deploy databases
- Or anything else you would run on a Linux or Windows server
