---
title: "Nhật ký Tuần 10"
date: "2025-11-10"
weight: 10
chapter: false
pre: " <b> 1.10. </b>"
---

### Mục tiêu Tuần 10:

* Thiết kế Kiến trúc Dự án Cuối kỳ
* Lập kế hoạch triển khai infrastructure
* Tạo thông số kỹ thuật chi tiết

### Công việc cần thực hiện trong tuần:
| Ngày | Công việc | Ngày bắt đầu | Ngày hoàn thành | Tài liệu tham khảo |
| --- | --- | --- | --- | --- |
| 1 | **Architecture Planning** <br> - Thiết kế hệ thống kiến trúc <br> - Định nghĩa cấu trúc microservices <br> - Tạo sơ đồ luồng dữ liệu | 10/11/2025 | 10/11/2025 | Architecture Docs |
| 2 | **Infrastructure Design** <br> - Lập kế VPC architecture <br> - Thiết kế lớp security <br> - Lập kế CI/CD pipeline | 11/11/2025 | 11/11/2025 | AWS Well-Architected |
| 3 | **Database Architecture** <br> - Thiết kế data models <br> - Lập kế RDS/DynamoDB usage <br> - Thiết kế caching strategy | 12/11/2025 | 12/11/2025 | Database Patterns |
| 4 | **Security Architecture** <br> - Thiết kế authentication flow với Cognito <br> - Lập kế WAF rules <br> - Thiết kế IAM structure | 13/11/2025 | 13/11/2025 | Security Best Practices |
| 5 | **CDN & Global Design** <br> - Lập kế CloudFront distribution <br> - Thiết kế Route 53 strategy <br> - Lập kế S3 bucket structure | 14/11/2025 | 14/11/2025 | AWS Patterns |
| 6-7 | **Create Terraform Scripts** <br> - Viết infrastructure code <br> - Tạo modular design <br> - Lập kế deployment stages | 15-16/11/2025 | 16/11/2025 | Terraform |

### Thành tựu Tuần 10:

* Tài liệu hóa kiến trúc hệ thống hoàn chỉnh
- Thiết kế infrastructure chi tiết với các cân nhắc security
* Kế hoạch kiến trúc data toàn diện
* Scripts Terraform để triển khai infrastructure
* Thiết kế CI/CD pipeline sử dụng AWS CodePipeline
* Kiến trúc security với WAF và Cognito integration
* Chiến lược triển khai toàn cầu với CloudFront và Route 53
* Infrastructure code modular để tái sử dụng

### Khó khăn đối mặt:

* Cân bằng độ phức tạp vs khả năng mở rộng
* Trade-offs giữa security và accessibility
* Tối ưu hóa chi phí trong thiết kế

### Giải pháp đã thực hiện:

* Tạo architecture decision records (ADRs)
* Xây dựng mô hình ước tính chi phí
* Thiết kế phương pháp triển khai theo giai đoạn
* Tạo chiến lược testing infrastructure

### Kế hoạch Tuần tới:

* Bắt đầu coding backend services
* Triển khai core infrastructure với Terraform
* Thiết lập CI/CD pipeline


