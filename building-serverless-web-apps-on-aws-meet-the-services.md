Despite the name, serverless isn't about not having servers; it's about abstracting the servers away so that developers and/or operations don't need to think about them. This means having to learn about services and software specific to a cloud provider instead that may not come as naturally to you and aren't as transferable. However, serverless can unlock a lot of power in terms of cost saving, scalability and not having to worry about servers, if used in the right situations. 

In this post, I'm introducing the AWS serverless services most commonly used for building serverless web applications.

### S3
The ubiquitous S3, famous for its users' security mess ups, which are actually rather hard to do. It's a file storage solution - you just dump files in a S3 bucket and pay for what you use in terms of data transfer and storage space, based on your chosen storage class (basically defines the speed of access and redundancy of a file). As well as simply storing files, a S3 bucket can trigger workflows on file upload, automatically change the storage class of files, and even serve a static website. 

### Lambda
AWS Lambda allows you to essentially just run functions on demand - triggered by events from other AWS services, or manual execution - and only pay for the time they're actually executing. Lambda functions can be in practically any language: most popular ones are supported out of the box, and support for other languages can be added in.

The free tier - one million executions per month - is forever, and the pricing, if you exceed that limit is reasonable. It scales seamlessly and virtually infinitely, but does have a limit on how many functions can be running at once. Cold starts can be an issue - because there is a dedicated VM or container underneath, it takes time to start up. AWS does keep the underlying infrastructure up for a while after each execution, at no cost to you, so this is a non-issue if the function is busy enough, and it's a fairly small delay anyway. 

### API Gateway
API Gateway is designed to go in front of other AWS services and enable them to be accessed over HTTPS with straight forward security, caching and routing options. It's often used with Lambda to create a REST API or some sort of webapp. Authentication can be done based on Cognito, API keys, IP address or more.

### DynamoDB
DynamoDB is a fast, simple, auto scaling NoSQL database solution. Being a NoSQL DB, though, relational data or even data with lots of primary keys isn't really its strong suit: joins aren't a thing, and you need to manually add secondary indexes if you want to search by multiple primary keys (you can add two out of the box, but they're treated as one), without returning more items than you need.

### CloudFront
CloudFront is a caching service - it caches webpages at edge locations, so that user requests aren't always hitting the place you're serving them from and are served from locations closer to the user. It also lets you use HTTPS with custom SSL certificates so you can get HTTPS on a S3 website.

### CloudWatch
CloudWatch does a lot of things, mostly centred around security, monitoring, alerting and logging. From a serverless application development point of view, the two most useful features are CloudWatch Events, which lets you schedule Lambda executions and the logging - everything that your Lambda functions print to the screen ends up here, so it's invaluable for debugging.

### Conclusion 
These are by far not the only serverless services useful for web development. Some of the other major ones are SNS, and SQS which enable asynchronous task processing, Fargate which lets you run containers serverlessly (for a premium), SES which sends emails, other database solutions such as Amazon Aurora Serverless (a serverless SQL database) and Cognito, a user authentication service. There are even services like AWS Amplify that simplify and abstract away the services. However, the core service fill the needs of most basic serverless apps. 

By the way, this post is going to be  part of a series exploring web development with AWS serverless services. So, if you liked it, you might want to follow me. Do be aware that I post all kinds of random stuff though :).