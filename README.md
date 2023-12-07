# Terraform LocalStack Demo

Welcome to the Terraform LocalStack Demo. In this demo we  provisioned
some AWS resources by using Terraform. To be independent of any AWS accounts, we've prepared
a docker-compose configuration that will start the [localstack](https://github.com/localstack) 
AWS cloud stack on your machine. Terraform is already fully configured to work together with 
localstack. Please see the usage section on how to authenticate.

# Demo

We'd like to track a list of files that have been uploaded. For this we require:
- A S3 Bucket to where we upload files
- A Lambda that get's triggered after a file upload.

# Usage

## Start localstack

```shell
docker-compose up -d
```

Watch the logs for `Execution of "preload_services" took 986.95ms`

## Authentication
```shell
export AWS_ACCESS_KEY_ID=test
export AWS_SECRET_ACCESS_KEY=test
export AWS_REGION=eu-central-1
```

## Init, plan, Apply and Destroy
```shell
terraform init
terraform plan
terraform apply --auto-approve
terraform destroy --auto-approve
```

## AWS CLI examples
### S3
Copy file to bucket
```shell
aws --region eu-central-1 --endpoint-url http://localhost:4566 s3 cp file.md s3://test-bucket-070/
```

List file in bucket
```shell
aws --region eu-central-1 --endpoint-url http://localhost:4566 s3 ls s3://test-bucket-070/
```

Delete file in bucket
```shell
aws --region eu-central-1 --endpoint-url http://localhost:4566 s3 rm s3://test-bucket-070/ --recursive
```

## CloudWatchLog

```shell
aws --region eu-central-1 --endpoint-url http://localhost:4566 logs describe-log-groups

aws --region eu-central-1 --endpoint-url http://localhost:4566 logs describe-log-streams --log-group-name '/aws/lambda/s3_lambda_function'

aws --region eu-central-1 --endpoint-url http://localhost:4566 logs get-log-events --log-group-name '/aws/lambda/s3_lambda_function' --log-stream-name '2023/11/28/[$LATEST]cc2d8745fbcfe2cbf538628c77226df7' --query 'events[*].[timestamp,message]' --output json > log.json
```


################
