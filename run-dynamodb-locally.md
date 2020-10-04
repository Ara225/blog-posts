Running a DynamoDB instance locally is great for testing or just messing around without incurring any cost - it's works exactly the same as the DynamoDB in the cloud All you need to do is to create a local instance and add an endpoint (JS), or endpoint_url (Python) option pointing to it when creating a DynamoDB object in the AWS SDK, or a --endpoint-url option when using the CLI. 

# Step 1 - Creating your Local Instance
The easiest method is via the DynamoDB Docker image. The process for this varies slightly depending on how you're testing your code though and what you're making.

## If you're using AWS SAM Local
At the time I was first trying to do this, I was building a serverless API with AWS SAM using AWS SAM local for testing, which would do a complete API Gateway in a container so I needed the SAM container and the DynamoDB container to talk to each other over the network. I found a Docker compose file which sets up the network and container in [rynop's answer on StackOverflow](https://stackoverflow.com/questions/48926260/connecting-aws-sam-local-with-dynamodb-in-docker). Here's my version of that:

```yaml
version: '3.5'

services:
  dynamo:
    container_name: local-dynamodb
    image: amazon/dynamodb-local
    networks:
      - local-dynamodb
    ports:
      - '8000:8000'
    volumes:
      - dynamodata:/home/dynamodblocal
    working_dir: /home/dynamodblocal
    command: '-jar DynamoDBLocal.jar -sharedDb -dbPath .'

networks:
  local-dynamodb:
    name: local-dynamodb

volumes:
  dynamodata: {}
```

To run this, save it in a file called docker-compose.yml and run ```docker-compose up -d dynamo```

Once this is setup, you can run commands something like the ones below in your project folder and get the DynamoDB and SAM containers talking to each other. To detect that you're running in this environment, you can check for the AWS_SAM_LOCAL environment variable inside your lambda code. To connect to the DB, use http://dynamo:8000 as the endpoint URL.

```bash
sam build --use-container
sam local start-api --docker-network local-dynamodb --skip-pull-image --profile default --parameter-overrides 'ParameterKey=StageName,ParameterValue=local'
```

## If you're using something else
If you're running your code in a container, you'll need to do something similar to the above. Otherwise, you can do it that way (the container will be accessible on http://localhost:8000) but it's probably easier to run the below command:
```bash
docker run --rm -d  -p 8000:8000/tcp amazon/dynamodb-local:latest
```
This runs the latest version of the DynamoDB container with port 8000 forwarded to localhost. So, the endpoint URL that needs to be set is http://localhost:8000.

# 2. - Create the Table
To create a table, simply create a JSON file with the below contents (tweaked as necessary for your environment), then use the AWS CLI command below to create that table.
```json
{
    "TableName": "testTable",
    "KeySchema": [
      { "AttributeName": "id", "KeyType": "HASH" }
    ],
    "AttributeDefinitions": [
      { "AttributeName": "id", "AttributeType": "N" }
    ],
    "ProvisionedThroughput": {
      "ReadCapacityUnits": 2,
      "WriteCapacityUnits": 2
    }
}
```
```bash
aws dynamodb create-table --cli-input-json file://local-db-create.json --endpoint-url http://localhost:8000
```