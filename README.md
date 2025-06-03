

# Aws
Cloud Documentation


# AWS Lambda
AWS Lambda is a `serverless compute service` offered by Amazon Web Services (AWS). It lets you run code without provisioning or managing servers. <br>
You just write your function code, upload it, and AWS handles the rest.

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
| **context** | Metadata (eg. request Id, timeout, memory etc) about the invocation, function, and environment. Provided by AWS to your function at runtime.                             |

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
