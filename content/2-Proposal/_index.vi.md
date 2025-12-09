---
title: "Đề xuất"
date: 2025-11-30
weight: 2
chapter: false
pre: " <b> 2. </b> "
---

# Nền tảng Chia sẻ Video

## 1. Tóm tắt Tổng quan

Đề xuất này phác thảo việc phát triển một Nền tảng Chia sẻ Video có khả năng mở rộng tận dụng các dịch vụ đám mây AWS. Nền tảng sẽ cho phép người dùng tải lên, phát trực tuyến và chia sẻ nội dung video với các tính năng bao gồm xác thực người dùng, quản lý nội dung và phát trực tuyến video theo thời gian thực.

Mục tiêu chính:

- Xây dựng nền tảng chia sẻ video an toàn, có khả năng mở rộng
- Triển khai xác thực và phân quyền người dùng
- Cung cấp khả năng phát trực tuyến video chất lượng cao
- Đảm bảo quản lý cơ sở hạ tầng hiệu quả về chi phí
- Mang lại trải nghiệm người dùng liền mạch trên các thiết bị

Giải pháp sử dụng các dịch vụ AWS bao gồm Amplify cho hosting frontend, Cognito cho xác thực, S3 cho lưu trữ, CloudFront cho phân phối nội dung và Interactive Video Service cho khả năng phát trực tuyến.

## 2. Phát biểu Vấn đề

### Vấn đề là gì?

Các giải pháp chia sẻ video hiện tại đối mặt với một số thách thức:

- Chi phí cơ sở hạ tầng cao cho lưu trữ và phát trực tuyến video
- Yêu cầu thiết lập và bảo trì phức tạp
- Khả năng mở rộng hạn chế trong thời gian sử dụng cao điểm
- Lỗ hổng bảo mật trong xác thực người dùng
- Chất lượng video kém và vấn đề buffering

### Giải pháp

Nền tảng chia sẻ video dựa trên AWS của chúng tôi giải quyết những thách thức này bằng cách:

- Tận dụng mô hình định giá trả theo mức sử dụng hiệu quả về chi phí của AWS
- Sử dụng các dịch vụ được quản lý để giảm chi phí vận hành
- Triển khai khả năng tự động mở rộng để xử lý tăng đột biến lưu lượng
- Cung cấp bảo mật cấp doanh nghiệp thông qua AWS Cognito
- Phân phối phát trực tuyến video chất lượng cao qua Amazon IVS và CloudFront

### Lợi ích và Lợi tức Đầu tư

**Tiết kiệm Chi phí:**

- Giảm 40-60% chi phí cơ sở hạ tầng so với hosting truyền thống
- Không cần đầu tư phần cứng trước
- Mô hình trả theo mức sử dụng tối ưu hóa chi phí vận hành

**Cải thiện Hiệu suất:**

- Độ khả dụng 99.9%
- Phân phối nội dung toàn cầu với độ trễ thấp
- Tự động mở rộng xử lý tăng lưu lượng gấp 10 lần một cách liền mạch

**Giá trị Kinh doanh:**

- Thời gian ra thị trường nhanh hơn (3-6 tháng so với 12+ tháng)
- Trải nghiệm người dùng nâng cao thúc đẩy tương tác cao hơn
- Kiến trúc có khả năng mở rộng hỗ trợ tăng trưởng kinh doanh

## 3. Kiến trúc Giải pháp

![Architecture Diagram](/video-sharing-platform.png)

### Các Dịch vụ AWS Sử dụng

**Amplify:** Nền tảng hosting và triển khai frontend cho các ứng dụng React/Vue.js với tích hợp CI/CD.

**Cognito:** Dịch vụ xác thực và phân quyền người dùng cung cấp đăng ký, đăng nhập và kiểm soát truy cập an toàn.

**App Runner:** Hosting API backend được container hóa với tự động mở rộng và cân bằng tải.

**DynamoDB:** Cơ sở dữ liệu NoSQL để lưu trữ hồ sơ người dùng, metadata video và dữ liệu ứng dụng.

**S3:** Lưu trữ đối tượng cho các file video, thumbnail và tài sản tĩnh với chính sách phiên bản và vòng đời.

**CloudFront:** CDN toàn cầu cho phân phối nội dung nhanh và phát trực tuyến video với bộ nhớ đệm edge.

**Amazon IVS (Interactive Video Service):** Dịch vụ phát trực tuyến video theo thời gian thực cho phát sóng trực tiếp và nội dung theo yêu cầu với độ trễ thấp.

**AWS Elemental MediaConvert:** Được sử dụng để chuyển đổi video gốc thành nhiều định dạng, nhiều độ phân giải, nhiều codec, để phát trực tuyến hoặc tải xuống.

**AWS Lambda:** Dịch vụ tính toán serverless cho xử lý theo sự kiện, kích hoạt chuyển mã video và các tác vụ tự động hóa backend.

**Code Pipeline:** Pipeline CI/CD cho kiểm thử, xây dựng và triển khai tự động.

**Code Build:** Dịch vụ build để biên dịch mã nguồn, chạy kiểm thử và tạo gói triển khai.

**Elastic Container Registry:** Registry container Docker để lưu trữ và quản lý các image ứng dụng.

### Thiết kế Thành phần

**Frontend Layer:**

- Ứng dụng web dựa trên React được host trên Amplify
- Thiết kế responsive hỗ trợ mobile và desktop
- Trình phát video theo thời gian thực với phát trực tuyến adaptive bitrate

**API Layer:**

- RESTful APIs được xây dựng với Node.js/Express
- Được container hóa và triển khai trên App Runner
- AWS Lambda cho các API endpoint serverless
- Tích hợp xác thực dựa trên JWT

**Data Layer:**

- Các bảng DynamoDB cho dữ liệu người dùng và metadata video
- Các bucket S3 cho lưu trữ video với phân tầng thông minh

**Security Layer:**

- Cognito user pools cho xác thực
- IAM roles và policies cho kiểm soát truy cập

**Video Processing Layer:**

- Các hàm AWS Lambda được kích hoạt bởi sự kiện S3
- AWS Elemental MediaConvert cho chuyển mã video
- Chuyển đổi tự động sang nhiều định dạng và độ phân giải
- Đầu ra HLS và DASH cho phát trực tuyến adaptive

**Streaming Architecture:**

- Amazon IVS cho khả năng phát trực tuyến trực tiếp
- CloudFront cho phân phối video toàn cầu
- Phát trực tuyến adaptive bitrate cho chất lượng tối ưu

### Các Trường hợp Sử dụng

**Sự kiện Phát trực tuyến Trực tiếp:**

- Phát sóng theo thời gian thực các hội nghị, webinar và sự kiện doanh nghiệp
- Phát trực tuyến đa bitrate cho trải nghiệm người xem tối ưu

**Video theo Yêu cầu (VOD):**

- Tải lên và chia sẻ nội dung giáo dục, hướng dẫn và tài liệu đào tạo
- Truy cập nội dung an toàn với quyền người dùng

**Chia sẻ Video Xã hội:**

- Chia sẻ nội dung do người dùng tạo
- Tính năng cộng đồng với bình luận và đánh giá

## 4. Triển khai Kỹ thuật

### Giai đoạn 1: Thiết lập Cơ sở hạ tầng

**Cấu hình Tài khoản AWS:**

- Cấu hình IAM roles và policies cho quyền truy cập tối thiểu

**Triển khai Dịch vụ Cốt lõi:**

- Triển khai các bảng DynamoDB với lập chỉ mục phù hợp
- Cấu hình các bucket S3 với mã hóa và chính sách vòng đời
- Thiết lập Cognito user pools và identity pools

### Giai đoạn 2: Phát triển Backend

**Phát triển API:**

- Xây dựng RESTful APIs sử dụng framework Node.js/Express
- Triển khai xác thực JWT với tích hợp Cognito
- Tạo các endpoint tải lên/xử lý video
- Phát triển các hàm Lambda cho các tác vụ theo sự kiện
- Phát triển API quản lý người dùng và nội dung

**Schema Cơ sở dữ liệu:**

- Users table: userId, avatarUrl, birthDate, channelId, createdAt, email, gender, name, phoneNumber
- Videos table: videoId, channelId, commentCount, createdAt, createdFromStreamId, description, duration, key, likeCount, playbackUrl, status, thumbnailUrl, title, type, updatedAt, userId, viewCount
- VideoLikes table: userId, videoId, createdAt
- Subscriptions table: userId, channelId, createdAt
- StreamSessions table: streamId, channelId, createdAt, description, endedAt, isLive, recordingUrl, startedAt, status, thumbnailUrl, title, updatedAt, userId, viewerCount
- Notifications table: recipientUserId, createdAt, actorAvatarUrl, actorName, actorUserId
- Comments table: videoId, commentId, content, createdAt, isDeleted, isEdited, likeCount, parentCommentId, replyCount, updatedAt, userAvatarUrl, userId, userName
- CommentLikes table: commentId, userId, createdAt
- Channels table: channelId, avatarUrl, channelArn, createdAt, currentStreamId, description, ingestEndpoint, isLive, name, playbackUrl, streamKeyArn, subscriberCount, userId, videoCount

**Container hóa:**

- Tạo Docker containers cho các dịch vụ API
- Đẩy images lên Elastic Container Registry
- Cấu hình App Runner cho triển khai tự động

### Giai đoạn 3: Phát triển Frontend

**Ứng dụng React:**

- Triển khai các thành phần UI responsive
- Tích hợp AWS Amplify SDK cho xác thực
- Xây dựng giao diện tải lên video với theo dõi tiến trình
- Tạo trình phát video với phát trực tuyến adaptive

**Tính năng Chính:**

- Đăng ký/đăng nhập người dùng với xác minh email
- Tải lên video với chức năng kéo và thả
- Phát trực tuyến video theo thời gian thực với lựa chọn chất lượng
- Bảng điều khiển người dùng cho quản lý nội dung

### Giai đoạn 4: Tích hợp Phát trực tuyến

**Thiết lập AWS Lambda & MediaConvert:**

- Tạo các hàm Lambda cho xử lý sự kiện S3
- Triển khai tự động hóa quy trình xử lý video
- Cấu hình các job template MediaConvert cho chuyển mã
- Thiết lập các preset đầu ra (1080p, 720p, 480p)
- Tạo các manifest HLS/DASH cho phát trực tuyến adaptive
- Cập nhật DynamoDB với trạng thái xử lý

**Thiết lập Amazon IVS:**

- Cấu hình các kênh phát trực tuyến và URL phát lại
- Triển khai phát trực tuyến adaptive bitrate
- Thiết lập quy trình ghi và lưu trữ

**Cấu hình CloudFront:**

- Tạo distributions cho phân phối nội dung video
- Cấu hình các vị trí edge cho phạm vi toàn cầu
- Triển khai chiến lược bộ nhớ đệm cho hiệu suất tối ưu

### Giai đoạn 5: Pipeline CI/CD

**Triển khai Tự động:**

- Cấu hình CodePipeline cho quy trình từ nguồn đến production
- Thiết lập CodeBuild cho kiểm thử và xây dựng tự động
- Triển khai chiến lược triển khai blue-green

**Chiến lược Kiểm thử:**

- Unit tests cho các API endpoints
- Integration tests cho tương tác dịch vụ AWS
- Load testing cho xác thực hiệu suất

## 5. Lịch trình & Các Mốc quan trọng

### Thời gian Dự án: 8 Tuần (2 Tháng)

**Tuần 1: Thiết lập & Lập kế hoạch**

- Thiết lập tài khoản AWS và cấu hình IAM
- Hoàn thiện yêu cầu dự án
- Phân công vai trò nhóm
- Triển khai cơ sở hạ tầng cơ bản (S3, DynamoDB, Cognito)

**Tuần 2-3: Phát triển Backend**

- RESTful APIs với .NET
- Xác thực JWT với Cognito
- Các endpoint tải lên video
- Triển khai schema cơ sở dữ liệu
- Triển khai App Runner

**Tuần 4-5: Phát triển Frontend**

- Ứng dụng React với thiết kế responsive
- Luồng xác thực người dùng
- Giao diện tải lên video
- Trình phát video cơ bản
- Triển khai Amplify

**Tuần 6: Tích hợp & Phát trực tuyến**

- Tích hợp frontend-backend
- Các hàm Lambda cho tự động hóa xử lý video
- Thiết lập MediaConvert cho chuyển mã video
- Thiết lập CloudFront cho phân phối video
- Chức năng phát trực tuyến cơ bản
- Kiểm thử và sửa lỗi

**Tuần 7-8: Triển khai Cuối cùng**

- Triển khai production
- Kiểm thử chấp nhận người dùng
- Hoàn thành tài liệu
- Chuẩn bị trình bày dự án

### Các Mốc quan trọng

**Mốc 1 (Tuần 1):** Cơ sở hạ tầng Sẵn sàng

- Các dịch vụ AWS được cấu hình
- Môi trường phát triển có thể truy cập

**Mốc 2 (Tuần 3):** Backend Hoàn thành

- APIs hoạt động
- Xác thực hoạt động

**Mốc 3 (Tuần 5):** Frontend Hoàn thành

- UI được phát triển đầy đủ
- Tải lên/phát lại video cơ bản hoạt động

**Mốc 4 (Tuần 8):** Ra mắt Production

- Hệ thống được triển khai và kiểm thử
- Tài liệu hoàn thành

## 6. Ước tính Ngân sách

### Chi phí Vận hành Hàng tháng (USD)

**Dịch vụ Tính toán:**

- App Runner (1 dịch vụ): $5-15/tháng
- AWS Lambda: $0-2/tháng
- Amplify Hosting: $0-5/tháng

**Lưu trữ & Cơ sở dữ liệu:**

- S3 Storage: $0-1/tháng
- DynamoDB: $0-2/tháng
- CloudFront Data Transfer: $0-2/tháng

**Dịch vụ Phát trực tuyến:**

- Amazon IVS (100 giờ/tháng): $150-300/tháng
- AWS Elemental MediaConvert: $20-50/tháng

**Bảo mật & Giám sát:**

- Cognito: $0/tháng

**CI/CD**

- CodePipeline & CodeBuild: $1-3/tháng
- ERC: $0-1/tháng
- [Calculator](https://calculator.aws/#/estimate?id=89df8e5261796a621e5cafe03560859b27336a22)

  **Tổng Chi phí Hàng tháng: $17-44/tháng**

## 7. Đánh giá Rủi ro

### Rủi ro Chính

**Rủi ro Kỹ thuật:**

- Độ phức tạp tích hợp → Bắt đầu đơn giản, tăng dần
- Quản lý thời gian → Xây dựng thời gian đệm, ưu tiên các tính năng cốt lõi

**Rủi ro Tài nguyên:**

- Vượt quá AWS Free Tier → Giám sát sử dụng, thiết lập cảnh báo

### Giải pháp Giảm thiểu

**Quản lý Kỹ thuật:**

- CloudFormation templates
- Kiểm thử từng giai đoạn
- Môi trường Dev/staging

**Kế hoạch Dự phòng:**

- **MVP:** Tải lên/phát lại video cơ bản
- **Core:** Xác thực người dùng + phát trực tuyến
- **Advanced:** Phát trực tuyến trực tiếp (tùy chọn)
- Sử dụng AWS Educate credits
- Mock services cho demos

## 8. Kết quả Mong đợi

### Chỉ số Hiệu suất

**Hiệu suất Hệ thống:**

- Tỷ lệ tải lên video thành công: >95%
- Độ trễ phát trực tuyến: <3 giây
- Thời gian hoạt động hệ thống: >99%
- Người dùng đồng thời: 100+
- Thời gian tải trang: <2 giây

### Tiêu chí Thành công

**Yêu cầu MVP:**

- Đăng ký/đăng nhập người dùng
- Tải lên/phát lại video cơ bản
- Xác thực an toàn
- Giao diện responsive
- Giám sát hệ thống

**Mục tiêu Mở rộng:**

- Khả năng phát trực tuyến trực tiếp
- Phân tích nâng cao
- Tính năng xã hội
- Ứng dụng di động đi kèm

### Attachments / References

- [Document](https://docs.google.com/document/d/1mFfQa_uaCAm5v0vkNDjvgzCpmMjl6qLx/edit)

- [Video](https://www.youtube.com/watch?v=b-yUXvy9HMY)

- [Slide](https://www.canva.com/design/DAG65vZ0WMw/_fH1tj8I_9JLBne3YUD-wQ/edit)
