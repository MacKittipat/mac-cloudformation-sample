# CloudFormation Sample

## Execute CloudFormation Template
```
// Setup VPC 
aws cloudformation create-stack --stack-name mac-sample-vpc --template-body file://sample-vpc.yml

// Setup servers and services
aws cloudformation create-stack --stack-name mac-sample-ec2 --template-body file://sample-ec2.yml --parameters ParameterKey=VpcId,ParameterValue=vpc-xxx ParameterKey=SubnetId,ParameterValue=subnet-xxx

aws cloudformation create-stack --stack-name mac-sample-ecs-fargate --template-body file://sample-ecs-fargate.yml --capabilities CAPABILITY_NAMED_IAM

// Public API Gateway + Lambda
aws cloudformation create-stack --stack-name sample-public-rest-api-lambda --template-body file://sample-public-rest-api-lambda.yml
aws cloudformation create-stack --stack-name sample-public-http-api-lambda --template-body file://sample-public-http-api-lambda.yml

// Public HTTP API Gateway + Private ALB
aws cloudformation create-stack --stack-name sample-ecs-fargate-private-alb --template-body file://sample-ecs-fargate-private-alb.yml --capabilities CAPABILITY_NAMED_IAM
aws cloudformation create-stack --stack-name sample-public-http-api-private-service --template-body file://sample-public-http-api-private-service.yml

// Public HTTP API Gateway + Private NLB + Cognito 
aws cloudformation create-stack --stack-name sample-ecs-fargate-private-nlb --template-body file://sample-ecs-fargate-private-nlb.yml --capabilities CAPABILITY_NAMED_IAM
aws cloudformation create-stack --stack-name sample-public-http-api-cognito --template-body file://sample-public-http-api-cognito.yml

// Public REST API Gateway + Private NLB
aws cloudformation create-stack --stack-name sample-ecs-fargate-private-nlb --template-body file://sample-ecs-fargate-private-nlb.yml --capabilities CAPABILITY_NAMED_IAM
aws cloudformation create-stack --stack-name sample-public-rest-api-private-service --template-body file://sample-public-rest-api-private-service.yml --capabilities CAPABILITY_NAMED_IAM

// Setup other resources
aws cloudformation create-stack --stack-name mac-s3 --template-body file://sample-s3.yml

aws cloudformation create-stack --stack-name sample-private-nlb --template-body file://sample-private-nlb.yml

aws cloudformation create-stack --stack-name mac-rds --template-body file://sample-rds.yml

aws cloudformation create-stack --stack-name mac-lambda-cognito --template-body file://sample-lambda-cognito.yml

aws cloudformation create-stack --stack-name mac-kms --template-body file://sample-kms.yml
```

## Connect to EC2
```
ssh -i {KEY_PAIR_FILE} ubuntu@{EC2_IP}
ssh -i {KEY_PAIR_FILE} ec2-user@{EC2_IP}
```

## Note for sample-lambda-cognito

### Get Token
```
curl -X POST --user {CLIENT_ID}:{CLIENT_SECRET} '{COGNITO_DOMAIN}/oauth2/token?grant_type=client_credentials&scope={SCOPE}' -H 'Content-Type: application/x-www-form-urlencoded'

curl -X POST --user {CLIENT_ID}:{CLIENT_SECRET} 'https://mac-hello-world.auth.ap-southeast-1.amazoncognito.com/oauth2/token?grant_type=client_credentials&scope=https://localhost/helloworld.read' -H 'Content-Type: application/x-www-form-urlencoded'
```

* COGNITO_DOMAIN is in `General settings > Domain name`
* CLIENT_ID and CLIENT_SECRET is in `General settings > App clients`
* SCOPE is in `General settings > App client settings` under `Allowed Custom Scopes`

After execute cloudformation template, you must click `Save app client changes` button to avoid `{"error":"invalid_grant}"`
1. Open Congnito in AWS Web Console
2. Click menu `General settings > App Clients > Show Details`
3. Click button `Save app client changes`

### Call Lambda
```
curl -H "Authorization: {TOKEN}" {API_ENDPOINT}/HelloWorld
```

### Reference 
* https://github.com/MacKittipat/mac-boot-docker
* https://github.com/1Strategy/fargate-cloudformation-example  
* https://github.com/aws-samples/aws-apigw-http-api-private--integrations
* https://github.com/nathanpeck/ecs-cloudformation
