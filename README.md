# CloudFormation Sample

## Run cloudformation
```
aws cloudformation create-stack --stack-name mac-sample-vpc --template-body file://sample-vpc.yml

aws cloudformation create-stack --stack-name mac-sample-ec2 --template-body file://sample-ec2.yml --parameters ParameterKey=VpcId,ParameterValue=vpc-xxx ParameterKey=SubnetId,ParameterValue=subnet-xxx

aws cloudformation create-stack --stack-name mac-sample-ecs-fargate --template-body file://sample-ecs-fargate.yml --capabilities CAPABILITY_NAMED_IAM

aws cloudformation create-stack --stack-name mac-rds --template-body file://sample-rds.yml

aws cloudformation create-stack --stack-name mac-lambda-cognito --template-body file://sample-lambda-cognito.yml

aws cloudformation update-stack --stack-name mac-sample-vpc --template-body file://sample-vpc.yml

aws cloudformation delete-stack --stack-name mac-sample-vpc 
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
* SCOPE is in `General settings > App clients` under `Allowed Custom Scopes`

After execute cloudformation template, you must click `Save app client changes` button to avoid `{"error":"invalid_grant}"`
1. Open Congnito in AWS Web Console
2. Click menu `General settings > App Clients > Show Details`
3. Click button `Save app client changes`

### Call Lambda
```
curl -H "Authorization: {TOKEN}" {API_ENDPOINT}/HelloWorld
```
