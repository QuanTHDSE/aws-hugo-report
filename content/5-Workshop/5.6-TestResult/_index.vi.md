---
title: "Kết quả kiểm tra"
date: 2025-11-30
weight: 6
chapter: false
pre: " <b> 5.6 </b> "
---

Sau khi thiết lập hoàn tất, quy trình CI/CD tự động hoạt động như sau:

1. Đẩy thay đổi mã nguồn
   Một developer commit và đẩy mã nguồn mới lên kho lưu trữ (GitHub hoặc CodeCommit).

2. Kích hoạt Pipeline
   CodePipeline tự động phát hiện thay đổi và bắt đầu pipeline.

3. Thực thi giai đoạn Source
   CodePipeline lấy phiên bản mới nhất của mã nguồn và chuyển nó sang giai đoạn tiếp theo.
   ![image1](/image1.png)
4. Giai đoạn Build (CodeBuild)
   CodeBuild kéo mã nguồn, đọc buildspec.yml, đăng nhập vào ECR, xây dựng Docker image, gắn thẻ và đẩy nó lên kho ECR tương ứng.
   (bạn có thể xem quá trình trong pipeline)
   ![image2](/image2.png)
5. Xuất bản Image lên ECR
   Docker image mới được xây dựng sẽ được lưu trữ và phiên bản hóa trong Amazon ECR.
