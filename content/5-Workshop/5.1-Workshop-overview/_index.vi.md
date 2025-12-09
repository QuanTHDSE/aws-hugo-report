---
title: "Giới thiệu"
date: 2025-11-30
weight: 1
chapter: false
pre: " <b> 5.1 </b> "
---

### Kiến trúc

![Diagram](workshop.png)

### Amazon ECR (Elastic Container Registry)

Amazon ECR là dịch vụ registry Docker image được quản lý hoàn toàn, cho phép bạn lưu trữ, quản lý và phiên bản hóa các container image một cách bảo mật. Nó tích hợp liền mạch với ECS, CodeBuild và CodePipeline.

### AWS CodePipeline

AWS CodePipeline là dịch vụ CI/CD được quản lý hoàn toàn, tự động hóa quy trình phát hành phần mềm. Bất cứ khi nào có thay đổi được đẩy lên kho mã nguồn, CodePipeline sẽ kích hoạt các giai đoạn như Source → Build → Deploy mà không cần can thiệp thủ công.

### AWS CodeBuild

AWS CodeBuild là dịch vụ build được quản lý hoàn toàn, biên dịch mã nguồn, chạy kiểm thử, xây dựng Docker image, gắn thẻ và đẩy chúng lên ECR. Tất cả các bước build được định nghĩa trong file buildspec.yml đơn giản.

### Tổng quan Workshop

Trong workshop này, bạn sẽ học cách xây dựng pipeline CI/CD tự động cho các ứng dụng container hóa sử dụng AWS CodePipeline, CodeBuild và Amazon ECR. Bạn sẽ tạo một quy trình hoàn chỉnh để lấy mã nguồn, xây dựng Docker image và đẩy nó một cách bảo mật lên kho ECR. Trong suốt workshop, bạn sẽ cấu hình các dịch vụ AWS thiết yếu, làm việc với file buildspec và hiểu cách mỗi thành phần đóng góp vào quy trình triển khai tự động và mượt mà.
