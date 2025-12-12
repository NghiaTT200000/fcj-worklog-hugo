# Terraform Architecture Workshop Report: SnapResume Serverless Application

## Introduction
This report provides a comprehensive guide on recreating the Terraform architecture for the SnapResume application - a serverless resume building platform deployed on AWS. The architecture demonstrates best practices for building scalable, secure, and cost-effective applications using AWS managed services.

## Architecture Overview

### High-Level Architecture
The SnapResume application implements a modern serverless architecture composed of:

- **Frontend**: React SPA hosted on AWS Amplify
- **Backend**: Serverless API using API Gateway and Lambda
- **Authentication**: AWS Cognito User Pools
- **Database**: DynamoDB with single-table design
- **Storage**: S3 for user uploads and static assets
- **CDN**: CloudFront for global content delivery
- **CI/CD**: AWS CodePipeline with CodeBuild
- **Monitoring**: CloudWatch with custom dashboards and alarms
- **Security**: WAF, VPC endpoints, and IAM best practices

### AWS Services Used

| Service | Purpose | Configuration |
|---------|---------|---------------|
| **AWS Amplify** | Frontend hosting and CI/CD | Connected to GitHub repository |
| **API Gateway** | REST API endpoints | Regional endpoint with Cognito authorizer |
| **AWS Lambda** | Backend business logic | Node.js 20.x runtime with 512MB memory |
| **AWS Cognito** | User authentication | User pools with MFA support |
| **DynamoDB** | NoSQL database | On-demand capacity with GSIs |
| **Amazon S3** | Object storage | Versioned with lifecycle policies |
| **CloudFront** | CDN distribution | OAI for secure access |
| **CodePipeline** | CI/CD pipeline | Automated deployment from Git |
| **CloudWatch** | Monitoring & logging | Dashboards and SNS alerts |
| **AWS WAF** | Application security | Common attack protection |

## Terraform Implementation Guide

### 1. Project Structure
```
terraform/
├── providers.tf         # AWS providers configuration
├── variables.tf         # Input variables
├── outputs.tf          # Output values
├── amplify.tf          # Amplify app configuration
├── api-gateway.tf      # API Gateway resources
├── lambda.tf           # Lambda functions and roles
├── cognito.tf          # Cognito user pools and identity pools
├── dynamodb.tf         # DynamoDB tables
├── s3-cloudfront.tf    # S3 buckets and CloudFront distributions
├── cicd.tf             # CodePipeline and CodeBuild
├── cloudwatch.tf       # Monitoring and alarms
└── route53-waf.tf      # DNS and WAF configuration
```

### 2. Provider Configuration

```hcl
terraform {
  required_version = ">= 1.0"

  required_providers {
    aws = {
      source  = "hashicorp/aws"
      version = "~> 5.0"
    }
  }
}

# Main provider for resources
provider "aws" {
  region = var.aws_region

  default_tags {
    tags = {
      Project     = var.project_name
      Environment = var.environment
      ManagedBy   = "Terraform"
    }
  }
}

# Additional provider for CloudFront (must be us-east-1)
provider "aws" {
  alias  = "us_east_1"
  region = "us-east-1"
}
```

### 3. Key Variables

```hcl
variable "aws_region" {
  description = "AWS region for resources"
  type        = string
  default     = "us-east-1"
}

variable "project_name" {
  description = "Project name"
  type        = string
  default     = "resume-snap"
}

variable "domain_name" {
  description = "Domain name for the application"
  type        = string
  default     = ""
}

variable "github_repo" {
  description = "GitHub repository (format: owner/repo)"
  type        = string
  default     = "your-org/your-repo"
}
```

### 4. Backend Services Implementation

#### DynamoDB Single-Table Design

```hcl
# Users Table
resource "aws_dynamodb_table" "users" {
  name           = "${var.project_name}-${var.environment}-users"
  billing_mode   = var.dynamodb_billing_mode
  hash_key       = "userId"
  range_key      = "email"

  point_in_time_recovery {
    enabled = true
  }

  stream_enabled   = true
  stream_view_type = "NEW_AND_OLD_IMAGES"
}

# Multi-entity Table (Sections, Templates, etc.)
resource "aws_dynamodb_table" "sections" {
  name           = "${var.project_name}-${var.environment}-sections"
  billing_mode   = var.dynamodb_billing_mode
  hash_key       = "PK"
  range_key      = "SK"

  attribute {
    name = "GSI1PK"
    type = "S"
  }

  attribute {
    name = "GSI1SK"
    type = "S"
  }

  global_secondary_index {
    name            = "GSI1"
    hash_key        = "GSI1PK"
    range_key       = "GSI1SK"
    projection_type = "ALL"
  }
}
```

#### Lambda Functions

```hcl
# Main API Lambda
resource "aws_lambda_function" "api" {
  filename      = "lambda.zip"
  function_name = "${var.project_name}-${var.environment}-api"
  role          = aws_iam_role.lambda_exec.arn
  handler       = "index.lambdaHandler"
  runtime       = "nodejs20.x"
  timeout       = 30
  memory_size   = 512

  environment {
    variables = {
      ENVIRONMENT              = var.environment
      DYNAMODB_USERS_TABLE     = aws_dynamodb_table.users.name
      DYNAMODB_SECTIONS_TABLE  = aws_dynamodb_table.sections.name
      COGNITO_USER_POOL_ID     = aws_cognito_user_pool.main.id
      S3_BUCKET                = aws_s3_bucket.user_uploads.bucket
    }
  }

  tracing_config {
    mode = "Active"
  }
}
```

#### API Gateway Configuration

```hcl
resource "aws_api_gateway_rest_api" "main" {
  name        = "${var.project_name}-${var.environment}-api"
  description = "API Gateway for ${var.project_name}"

  endpoint_configuration {
    types = ["REGIONAL"]
  }
}

# Cognito Authorizer
resource "aws_api_gateway_authorizer" "cognito" {
  name            = "${var.project_name}-cognito-authorizer"
  type            = "COGNITO_USER_POOLS"
  rest_api_id     = aws_api_gateway_rest_api.main.id
  provider_arns   = [aws_cognito_user_pool.main.arn]
}
```

### 5. Frontend Hosting with Amplify

```hcl
resource "aws_amplify_app" "frontend" {
  name       = "${var.project_name}-${var.environment}-frontend"
  repository = var.github_repo_url

  access_token = var.github_access_token

  # Build specification
  build_spec = <<-EOT
    version: 1
    frontend:
      phases:
        preBuild:
          commands:
            - npm ci
        build:
          commands:
            - npm run build
      artifacts:
        baseDirectory: build
        files:
          - '**/*'
      cache:
        paths:
          - node_modules/**/*
  EOT

  # Custom rules for React Router
  custom_rule {
    source = "/<*>"
    target = "/index.html"
    status = "404-200"
  }
}

# Branch configuration
resource "aws_amplify_branch" "main" {
  app_id      = aws_amplify_app.frontend.id
  branch_name = var.github_branch_frontend

  framework = "React"

  # Environment variables for frontend
  environment_variables = {
    REACT_APP_API_ENDPOINT         = "https://${aws_cloudfront_distribution.static_assets.domain_name}/api"
    REACT_APP_COGNITO_USER_POOL_ID = aws_cognito_user_pool.main.id
    REACT_APP_COGNITO_CLIENT_ID    = aws_cognito_user_pool_client.web.id
  }
}
```

### 6. CI/CD Pipeline

```hcl
resource "aws_codepipeline" "main" {
  name     = "${var.project_name}-${var.environment}-pipeline"
  role_arn = aws_iam_role.codepipeline_role.arn

  artifact_store {
    location = aws_s3_bucket.codepipeline_artifacts.bucket
    type     = "S3"
  }

  stage {
    name = "Source"

    action {
      name             = "Source"
      category         = "Source"
      owner            = "AWS"
      provider         = "CodeStarSourceConnection"
      version          = "1"
      output_artifacts = ["source_output"]

      configuration = {
        ConnectionArn    = aws_codestarconnections_connection.github.arn
        FullRepositoryId = var.github_repo
        BranchName       = var.github_branch_backend
      }
    }
  }

  stage {
    name = "Build"

    action {
      name            = "Build"
      category        = "Build"
      owner           = "AWS"
      provider        = "CodeBuild"
      version         = "1"
      input_artifacts = ["source_output"]

      configuration = {
        ProjectName = aws_codebuild_project.backend.name
      }
    }
  }
}
```

### 7. Security Implementation

#### VPC Endpoints for Private Connectivity

```hcl
# Gateway Endpoint for S3
resource "aws_vpc_endpoint" "s3" {
  vpc_id       = aws_vpc.main.id
  service_name = "com.amazonaws.${var.aws_region}.s3"

  route_table_ids = [aws_route_table.private.id]

  policy = jsonencode({
    Version = "2012-10-17"
    Statement = [
      {
        Effect = "Allow"
        Principal = "*"
        Action = [
          "s3:GetObject",
          "s3:PutObject",
          "s3:ListBucket"
        ]
        Resource = [
          aws_s3_bucket.user_uploads.arn,
          "${aws_s3_bucket.user_uploads.arn}/*"
        ]
      }
    ]
  })
}

# Interface Endpoint for API Gateway
resource "aws_vpc_endpoint" "apigateway" {
  vpc_id              = aws_vpc.main.id
  service_name        = "com.amazonaws.${var.aws_region}.execute-api"
  vpc_endpoint_type   = "Interface"
  private_dns_enabled = true

  subnet_ids = aws_subnet.private[*].id

  security_group_ids = [aws_security_group.vpc_endpoint.id]
}
```

#### WAF Configuration

```hcl
resource "aws_wafv2_web_acl" "main" {
  name  = "${var.project_name}-${var.environment}-waf"
  scope = "CLOUDFRONT"

  default_action {
    allow {}
  }

  rule {
    name     = "ManagedRuleGroup"
    priority = 1

    override {
      action_to_use {
        allow {}
      }

      mania_rule_group {
        name     = "AWSManagedRulesCommonRuleSet"
        vendor_name = "AWS"

        rule_action_override {
          name_to_override = "NoUserAgent_HEADER"
          action_to_use {
            block {}
          }
        }
      }
    }

    visibility_config {
      cloudwatch_metrics_enabled = true
      metric_name               = "ManagedRuleGroup"
      sampled_requests_enabled  = true
    }
  }

  visibility_config {
    cloudwatch_metrics_enabled = true
    metric_name               = "webACL"
    sampled_requests_enabled  = true
  }
}
```

## Deployment Steps

### 1. Prerequisites
- AWS CLI configured with appropriate permissions
- Terraform >= 1.0 installed
- GitHub repository with application code
- GitHub Personal Access Token for Amplify

### 2. Initialize Terraform
```bash
cd terraform
terraform init
```

### 3. Review Variables
Create a `terraform.tfvars` file:
```hcl
aws_region        = "us-east-1"
project_name      = "resume-snap"
environment       = "dev"
domain_name       = "example.com"
github_repo       = "your-org/your-repo"
github_branch_frontend = "main"
github_branch_backend  = "main"
github_access_token    = "ghp_your_token_here"
```

### 4. Plan and Apply
```bash
# Review the execution plan
terraform plan

# Apply the configuration
terraform apply
```

### 5. Configure CI/CD
1. Create GitHub token with repository access
2. Store token securely in AWS Secrets Manager
3. Update Amplify and CodePipeline configurations
4. Push changes to trigger initial deployment

## Monitoring and Observability

### CloudWatch Dashboard
The Terraform configuration creates a comprehensive CloudWatch dashboard that includes:

- **API Metrics**: Request count, latency, error rates
- **Lambda Metrics**: Invocations, duration, throttles, errors
- **DynamoDB Metrics**: Read/Write capacity, throttled requests
- **CloudFront Metrics**: Requests, cache hit ratio, 4xx/5xx errors
- **Amplify Metrics**: Build status, deployment history

### Alarms and Notifications
```hcl
resource "aws_cloudwatch_metric_alarm" "api_error_rate" {
  alarm_name          = "${var.project_name}-api-error-rate"
  comparison_operator = "GreaterThanThreshold"
  evaluation_periods  = "2"
  metric_name         = "5XXError"
  namespace           = "AWS/ApiGateway"
  period              = "300"
  statistic           = "Sum"
  threshold           = "10"

  alarm_actions = [aws_sns_topic.alarms[0].arn]
}
```

## Cost Optimization

### 1. DynamoDB
- Use on-demand capacity for unpredictable workloads
- Implement TTL for temporary data
- Enable point-in-time recovery for critical tables

### 2. Lambda
- Configure appropriate memory and timeout settings
- Use Provisioned Concurrency for production workloads
- Implement dead letter queues for error handling

### 3. CloudFront
- Enable cache optimization for static assets
- Use Origin Access Identity for secure access
- Configure geo-restriction if needed

### 4. S3
- Implement lifecycle policies for object transitions
- Use S3 Intelligent Tiering for data with unknown access patterns
- Enable versioning for critical data

## Security Best Practices

### 1. Network Security
- VPC endpoints for private connectivity
- Security groups with least privilege
- Network ACLs for additional filtering

### 2. IAM Security
- Least privilege access policies
- IAM roles instead of long-term credentials
- Resource-based policies where applicable

### 3. Data Protection
- Encryption at rest and in transit
- Sensitive data in environment variables or Secrets Manager
- Regular rotation of secrets

### 4. Application Security
- AWS WAF for common web attacks
- Cognito for authentication
- API Gateway throttling and usage plans

## Cleanup Procedure

To avoid ongoing charges, clean up resources in order:

```bash
# 1. Destroy all infrastructure
terraform destroy

# 2. Manual cleanup steps:
# - Delete any manually created resources
# - Remove GitHub tokens from AWS Secrets Manager
# - Clean up CloudWatch logs older than retention period
# - Delete any S3 buckets that might contain data
```

## Lessons Learned

1. **Single Table Design**: DynamoDB single-table design reduces costs and simplifies data access patterns
2. **Serverless Benefits**: Significant cost savings for unpredictable workloads
3. **Infrastructure as Code**: Version control and repeatability of deployments
4. **Monitoring**: Proactive monitoring prevents issues before they impact users
5. **Security**: Implement defense-in-depth with multiple security layers

## Conclusion

This Terraform architecture demonstrates a production-ready serverless implementation on AWS. The modular design allows for easy customization and scaling. By following this guide, teams can quickly deploy similar applications with confidence in the infrastructure's reliability, security, and cost-effectiveness.

The architecture leverages AWS managed services to reduce operational overhead while maintaining flexibility for custom business logic. The comprehensive monitoring and security implementations ensure the application remains performant and secure in production environments.

## References

- [AWS Documentation](https://docs.aws.amazon.com/)
- [Terraform AWS Provider](https://registry.terraform.io/providers/hashicorp/aws/latest/docs)
- [AWS Well-Architected Framework](https://docs.aws.amazon.com/wellarchitected/)
- [Serverless Application Model](https://github.com/awslabs/serverless-application-model)