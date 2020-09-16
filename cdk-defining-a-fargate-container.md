# Overview
This is an attempt to break down the steps needed to define Fargate containers with the CDK.

Code examples are in Python, but should be easy enough to translate if needed. Fargate containers can be slightly more difficult to work with than most things in the CDK, as they require you to create an IAM policy separately. You also need to create a dockerfile separately. You can see my GitHub project https://github.com/Ara225/serverless-audiobook-maker to see this code in situ

1. Import modules
```python
from aws_cdk import (core
                     aws_ecs, 
                     aws_ecs_patterns,
                     aws_iam,
                     aws_ec2
                    )
```

2. Create an IAM execution role.
```python
role = aws_iam.Role(self, "FargateContainerRole", assumed_by=aws_iam.ServicePrincipal("ecs-tasks.amazonaws.com"))
```

3. Add policies granting the permissions you need
```python
role.add_to_policy(aws_iam.PolicyStatement(actions=["s3:PutObject"], resources=[VideoUploadBucket.bucket_arn+"/*"]))
```

4. Create a VPC for your container to run in
```python
vpc = aws_ec2.Vpc(self, "CdkFargateVpc", max_azs=2)
```
5. Create a cluster
```python
cluster = aws_ecs.Cluster(self, 'FargateCluster', vpc=vpc)
```
6. Add a container image 
```python
image = aws_ecs.ContainerImage.from_asset(PATH_TO_FOLDER_CONTAINING_DOCKER_FILE)
```

7. Create a task definition
```python
task_definition = aws_ecs.FargateTaskDefinition(
    self, "FargateContainerTaskDefinition", execution_role=role, task_role=role, cpu=1024, memory_limit_mib=3072
)
```

8. Add container to the task definition. Don't think logging is necessary but is useful
```python
container = task_definition.add_container(
    "Container", image=image,
    logging=aws_ecs.AwsLogDriver(stream_prefix="videoProcessingContainer")
)
```

9. Optional - Add a port mapping
```python
port_mapping = aws_ecs.PortMapping(container_port=80, host_port=80)
container.add_port_mappings(port_mapping)
```