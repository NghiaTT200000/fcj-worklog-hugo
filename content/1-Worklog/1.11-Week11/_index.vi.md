---
title: "Nhật ký Tuần 11"
date: "2025-11-17"
weight: 11
chapter: false
pre: " <b> 1.11. </b> "
---

### Mục tiêu Tuần 11:

* Phát triển Backend Services
* Triển khai Lambda Function
* Thiết lập CI/CD Pipeline

### Công việc cần thực hiện trong tuần:
| Ngày | Công việc | Ngày bắt đầu | Ngày hoàn thành | Tài liệu tham khảo |
| --- | --- | --- | --- | --- |
| 1 | **Infrastructure Deployment** <br> - Áp dụng Terraform scripts <br> - Thiết lập VPC và security groups <br> - Cấu hình S3 buckets | 17/11/2025 | 17/11/2025 | Terraform |
| 2 | **Database Setup** <br> - Triển khai RDS instances <br> - Cấu hình DynamoDB tables <br> - Thiết lập ElastiCache | 18/11/2025 | 18/11/2025 | AWS Console |
| 3 | **Lambda Development** <br> - Tạo Lambda functions <br> - Triển khai API endpoints <br> - Thiết lập API Gateway | 19/11/2025 | 19/11/2025 | VS Code |
| 4 | **Cognito Integration** <br> - Thiết lập User Pool <br> - Cấu hình authentication <br> - Triển khai auth trong Lambda | 20/11/2025 | 20/11/2025 | AWS Console |
| 5 | **CodeCommit Setup** <br> - Khởi tạo repositories <br> - Push code ban đầu <br> - Thiết lập branch strategy | 21/11/2025 | 21/11/2025 | Git |
| 6-7 | **CI/CD Pipeline** <br> - Cấu hình CodeBuild <br> - Thiết lập CodePipeline <br> - Test automated deployments | 22-23/11/2025 | 23/11/2025 | AWS Code* |

### Thành tựu Tuần 11:

* Triển khai thành công infrastructure sử dụng Terraform
* Cấu hình hoàn toàn database layer với RDS và DynamoDB
* Triển khai backend services với Lambda
* Tích hợp hệ thống authentication Cognito
* Thiết lập version control với CodeCommit
* Xây dựng CI/CD pipeline với automated deployments
* Tạo Lambda functions modular cho các dịch vụ khác nhau
* Triển khai error handling và logging phù hợp

### Khó khăn đối mặt:

* Quản lý state Terraform
* Tối ưu hóa cold start Lambda
* Debug CI/CD pipeline
* Độ phức tạp của cấu hình Cognito

### Giải pháp đã thực hiện:

* Sử dụng remote state storage cho Terraform
* Triển khai Lambda best practices cho performance
* Tạo pipeline testing strategies
* Xây dựng templates cấu hình Cognito

### Kế hoạch Tuần tới:

* Phát triển frontend với Amplify
* Testing và bảo đảm chất lượng
* Tối ưu hóa performance
* Tài liệu hóa và deployment


