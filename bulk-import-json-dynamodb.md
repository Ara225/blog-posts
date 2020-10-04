# Overview
I recently needed to import a lot of JSON data into DynamoDB for an API project I'm was working on. I couldn't find any pre baked solutions online, so I built my own. It's really very simple. It's a Node.js CLI script which accepts two command line arguments: the JSON file to load the data from (it expects this to contain a list of the items you want inserted) and the name of the DynamoDB table to insert it into. The only thing you'll need to change is to replace INSERT_REGION_NAME_HERE with your region, unless your items are very big - 25 is the max number of items that can be included in a batch request but there's also a separate size limit per request, so you might have to reduce that number.

For this sort of request, the individual keys in the items need to be wrapped in DynamoDB's type declaration syntax (e.g. { "S":"Blah"} to indicate that "Blah" is supposed to be a string"). Luckily, the npm module dynamodb-data-types does this all, but if you want to translate this code into another language, you'll probably need to do it yourself. 

# Prerequisites
* Node.js - Version doesn't matter much.
* AWS credentials setup on your local machine (see AWS's site)
* AWS SDK for Node.js (aws-sdk on npm)
* dynamodb-data-types npm module

# Code
```js
// Load the AWS SDK for Node.js
const AWS = require('aws-sdk');
const attr = require('dynamodb-data-types').AttributeValue;
// Set the region
AWS.config.update({ region: 'INSERT_REGION_NAME_HERE' });
let data = require(process.argv[2]);
// Create DynamoDB service object
var ddb = new AWS.DynamoDB({ apiVersion: '2012-08-10'});
let items = [];
for (let index = 0; index < data.length; index++) {
    const element = data[index];
    // Format element in the correct format for DynamoDB's API 
    let item = {
        PutRequest: {
            Item: attr.wrap(element)
        }
    };
    items.push(item);
    
    if (items.length == 25 || index == data.length-1) {
        let params = {
            RequestItems: {
            }
        };
        params["RequestItems"][process.argv[3]] = items;
        // Async function call to write the items to the DB 
        ddb.batchWriteItem(params, function (err, data) {
            if (err) {
                console.log("Error", err);
            } else {
                console.log("Success", data);
            }
        });
        items = [];
    }
}
```