# CloudFormation Sample

### Run cloudformation
```
aws cloudformation create-stack --stack-name mac-sample-ec2 --template-body file://sample-vpc.yml

aws cloudformation create-stack --stack-name mac-sample-ec2 --template-body file://sample-ec2.yml --parameters ParameterKey=MacVpcId,ParameterValue=vpc-xxx ParameterKey=MacPublicSubnetId,ParameterValue=subnet-xxx

aws cloudformation update-stack --stack-name mac-sample-ec2 --template-body file://sample-vpc.yml

aws cloudformation delete-stack --stack-name mac-sample-ec2 
```

### Connect to EC2
```
ssh -i {KEY_PAIR_FILE} ubuntu@{EC2_IP}
ssh -i {KEY_PAIR_FILE} ec2-user@{EC2_IP}
```

