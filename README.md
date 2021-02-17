# Servian-Challenge-Solution

## Solution Overview

In this solution I am going to dockerize the application and deploy it to AWS. I am using Cloudformation as IAC to provition all resources needed by this solution, AWS RDS (postgres) as a database for the app and ECS with FARGATE to depoy and scale the app. This solution consider the high availibility on both RDS and ECS service.

## Solution Prerequisite

  1. AWS Account
  2. AWS IAM User with needed IAM plicies to provistion all the infrastructes, resources and IAM roles defined in the CloudForamtion Template.
  3. Access Key and Secret Access Key for that user.


## CICD

I am using github workflow defined in [pipeline.yml] ( .github/workflows/pipeline.yml)
  
