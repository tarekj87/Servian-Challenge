# Servian-Challenge-Solution

## Solution Overview

In this solution I am going to dockerize the application and deploy it to AWS. I am using Cloudformation as IAC to provition all resources needed by this solution, AWS RDS (postgres) as a database for the app and ECS with FARGATE to depoy and scale the app. This solution consider the high availibility on both RDS and ECS service.

## Solution Pre-requisite

  1. AWS Account
  2. AWS IAM User with needed IAM plicies to provistion all the infrastructes, resources and IAM roles defined in the CloudForamtion Template.
  3. Access Key and Secret Access Key for that user.
  4. update github XXXXXXXX


## CI/CD

I am using github workflow defined in [pipeline file](.github/workflows/pipeline.yml) to build and push two docker containers to AWS ECR. the first docker container is defined by the [file](DockerfileSeed) which is going to seed the database. The second docker container is defined by the [file](Dockerfile) whch is going to run our application.

## About CloudFormation Templates:

  1. [First Stack](MyNetwork) is to provision the VPC, Subnets Route tables, Nat Gateway and all resources needed by netowrking for this solution.
  2. [Second Stack](MyRDS) is going to provison ECR repository, Multi AZ RDS with postgres engine and SSM prameters to save the username, password, db name (Because you can override the defaule value of parametes) and RDS endpoint ( Because AWS specify the RDS endpoint during the provisoning)
  3. [Thirds Stack](MyCluster) is going to create the ECS cluster with two task definitions , one service, application load balancer and all the resources needed for ECS and ALB. 

## Issues I faced in this solution:

  1. The code couldn't override the value of variables defined in [toml file](conf.toml) when passing them as environment variables. So I couldn't configure ECS to get those value from SSM and inject them inside the container as environmnet values. To override this issue, I am building the conf.toml file inside CICD workspace [pipeline](github/workflows/pipeline.yml)
  2. Cloudformation does not support running a task only once (to seed the db), and only support a Service to run that task which means the Service will keep seeding the db forever. I didn't solve this issue (Seeding the db in this solution) but to do this conider the following solution:
      
      1. Seeding the db inside CICD (I think CircleCI has module to run one-off task isnide ECS)
      2. You can use AWS Lambda Docker (a new feature in lambda) to the docker instance thats going to seed the db. But that required provisoning the Labda function inside the VPC ( to get access to RDS) and you need a way to invoke the lamda function (API Gateway for example)
