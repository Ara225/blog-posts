I recently spent days trying to figure out how to make Cognito authentication with a REST API work in the AWS CDK, to the point that I even filed a (unnecessary) bug report, so I figured I might as well make that the subject of my first dev.to post as it's pretty short and sweet.

### The problem
Adding a authorizer to the API is deceptively easy. You have to use the underlying CloudFormation resource as this feature isn't fully built out in the CDK yet, but the authorizer gets added to the API in a completely normal manner with the below code.

```python
api = aws_apigateway.RestApi(self, 'API', rest_api_name='API')
auth = aws_apigateway.CfnAuthorizer(self, "adminSectionAuth", rest_api_id=api.rest_api_id,
                                        type='COGNITO_USER_POOLS', 
                                        identity_source='method.request.header.Authorization',
                                        provider_arns=[
                                            'arn:aws:cognito-idp:...'],
                                        name="adminSectionAuth"
                                    )
```

However, adding it to the method is another matter. Passing the object doesn't work - it doesn't error out initially, but the ID of the authorizer doesn't populate in the template so it fails as soon as CDK tries to create the resource. 

```python
resource = api.root.add_resource("endpoint")
lambda_function = aws_lambda.Function(self, "lambdaFunction",
                                      handler='app.lambda_handler',
                                      runtime=aws_lambda.Runtime.PYTHON_3_8,
                                      code=aws_lambda.Code.from_asset("path/to/code")
                                      )
lambda_integration = aws_apigateway.LambdaIntegration(lambda_function, proxy=True)
method = resource.add_method("GET", lambda_integration, 
                             authorization_type=AuthorizationType.COGNITO,
                             authorizer=auth
                            )
```
Passing the logical_id or ref properties of the object don't work either - the authorizer parameter needs to be a object.

### The Answer                                

It turns out that you actually have to override properties of the object to get it working, namely the troublesome AuthorizerId field that wasn't populating before.

```python
resource = api.root.add_resource("endpoint")
lambda_function = aws_lambda.Function(self, "lambdaFunction",
                                      handler='app.lambda_handler',
                                      runtime=aws_lambda.Runtime.PYTHON_3_8,
                                      code=aws_lambda.Code.from_asset("path/to/code")
                                      )
lambda_integration = aws_apigateway.LambdaIntegration(lambda_function, proxy=True)
method = resource.add_method("GET", lambda_integration)
method_resource = method.node.find_child('Resource')
method_resource.add_property_override('AuthorizationType', 'COGNITO_USER_POOLS')
method_resource.add_property_override('AuthorizerId', {"Ref": auth.logical_id})
```
I found this on GitHub in a very informative comment {% github https://github.com/aws/aws-cdk/issues/723#issuecomment-504753280 %}
