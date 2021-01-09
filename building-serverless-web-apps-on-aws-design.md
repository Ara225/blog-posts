I'm going to focus on using serverless to build full stack web apps on AWS, because that's the scenario I'm most interested in, however, it's use case is pretty broad. By the way, AWS Amplify simplifies some or most of the process of building full stack web apps, depending on you use case. For now, though, I'm ignoring it and focusing on the building blocks - they're at their most powerful alone, and once you understand them, AWS Amplify is fairly easy to understand.

# Different approaches
One of the key ways to build a fullstack web app is with a separated backend and frontend, or even just a frontend. HTML, CSS, and JavaScript files are put in a S3 bucket which is setup for static website hosting. If a backend is needed, the JavaScript makes calls to an API. It doesn't really matter what the format of this API is, but an API built with Lambda and API Gateway seems to be pretty much the standard.

The only other major way to do it is by having a Lambda or even

# Problems and Benefits




## Frontend
### S3
## Caching
### CloudFront
## Compute
### Lambda
### Fargate
### Batch
## Database
### DynamoDB
### AWS Auroa
## Messaging
### SQS
### SNS
### SES
## Authentication
### Cognito