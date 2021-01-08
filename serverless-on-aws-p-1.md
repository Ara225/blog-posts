Despite the name, serverless isn't about not having servers; it's about abstracting the servers away so that developers and/or operations don't need to think about them. This means having to learn about services and software specific to a cloud provider instead that may not come as naturally to you and aren't as transferable. However, serverless can unlock a lot of power in terms of cost saving, scalability and not having to worry about servers, if used in the right situations. This is the first part of a series exploring AWS serverless services and the stuff that they can be used for. 

# Meet the Services
AWS provides a wide range of serverless services. Many are very niche, so I'm just going to discuss the ten most mainstream ones here.

### S3
The ubiquitous S3, famous for its users' security mess ups, which are actually rather hard to do. It's a file storage solution - you just dump files in a S3 bucket and pay for what you use in terms of data transfer and storage space, based on your chosen storage class (basically defines the speed of access and redundancy of a file). As well as simply storing files, a S3 bucket can trigger workflows on file upload, automatically change the storage class of files, and even serve a static website. 

### Lambda
AWS Lambda allows you to essentially just run functions on demand and only pay for the time they're actually executing. It has a  generous forever

### API Gateway
### DynamoDB
### CloudFront
### CloudWatch
### SQS
### SNS
### SES
### Cognito
