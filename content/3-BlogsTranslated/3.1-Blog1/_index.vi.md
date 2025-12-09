---
title: "Blog 1"
date: "2025-11-30"
weight: 1
chapter: false
pre: " <b> 3.1. </b> "
---

## [AWS Storage Blog](https://aws.amazon.com/blogs/storage/)

# Tự động hóa quy trình xử lý yêu cầu bồi thường y tế từ giấy sang điện tử với AWS

Bởi Anil Chinnam và Chris Bladon | ngày 29 THÁNG 5 NĂM 2025 | Trong [Amazon Bedrock](https://aws.amazon.com/blogs/storage/category/artificial-intelligence/amazon-machine-learning/amazon-bedrock/), [Amazon Simple Storage Service (S3)](https://aws.amazon.com/blogs/storage/category/storage/amazon-simple-storage-services-s3/), [AWS Lambda](https://aws.amazon.com/blogs/storage/category/compute/aws-lambda/), [AWS Transfer Family](https://aws.amazon.com/blogs/storage/category/migration/aws-transfer-family/), [Generative AI](https://aws.amazon.com/blogs/storage/category/artificial-intelligence/generative-ai/), [Intermediate (200)](https://aws.amazon.com/blogs/storage/category/learning-levels/intermediate-200/), [Migration & Transfer Services](https://aws.amazon.com/blogs/storage/category/migration/), [Storage](https://aws.amazon.com/blogs/storage/category/storage/), [Technical How-to](https://aws.amazon.com/blogs/storage/category/post-types/technical-how-to/) | [Permalink](https://aws.amazon.com/blogs/storage/automating-paper-to-electronic-healthcare-claims-processing-with-aws/) | [Comments](https://aws.amazon.com/blogs/storage/automating-paper-to-electronic-healthcare-claims-processing-with-aws/#Comments) | [Share](https://aws.amazon.com/vi/blogs/storage/automating-paper-to-electronic-healthcare-claims-processing-with-aws/#)

Các chương trình bảo hiểm y tế xử lý hàng tỷ yêu cầu bồi thường (claims) điện tử mỗi năm. Hội đồng Chăm sóc Sức khỏe Chất lượng và Giá cả phải chăng (CAQH) ước tính rằng khoảng 10% yêu cầu bồi thường vẫn đến dưới dạng tài liệu giấy, tương đương hàng trăm triệu hồ sơ giấy được gửi hàng năm tại Hoa Kỳ. Những yêu cầu bồi thường bằng giấy này tạo ra nút thắt trong xử lý và tiêu tốn một phần chi phí vận hành cùng nguồn lực vượt mức, với chi phí xử lý thủ công có thể cao gấp 10 lần so với xử lý điện tử, đồng thời kéo dài chu kỳ hoàn trả từ vài giờ lên đến vài tuần.

Các nhóm vận hành xử lý yêu cầu bồi thường hiện nay phải xử lý nhiều biểu mẫu giấy khác nhau—CMS-1500 cho dịch vụ chuyên môn, UB-04 cho dịch vụ bệnh viện, biểu mẫu Nha khoa ADA, và các biểu mẫu độc quyền tùy chỉnh—thông qua các quy trình tốn nhiều công sức, bao gồm số hóa biểu mẫu và nhập dữ liệu. Không giống như các yêu cầu bồi thường điện tử đi qua các kênh chuẩn EDI (Electronic Data Interchange), các yêu cầu giấy cần đến các hệ thống riêng biệt sử dụng công nghệ OCR và định dạng độc quyền. Cách tiếp cận song song này dẫn đến tỷ lệ lỗi cao hơn và làm tăng rủi ro tuân thủ HIPAA. Với một chương trình bảo hiểm y tế trung bình, những điểm kém hiệu quả này có thể dẫn đến hàng triệu USD chi phí hành chính hằng năm, lẽ ra có thể được phân bổ cho các dịch vụ thành viên và sáng kiến quản lý chăm sóc.

Để giải quyết các thách thức trong xử lý giấy tờ này, chúng tôi đã xây dựng một giải pháp nhắm mục tiêu tích hợp liền mạch với hệ thống xử lý yêu cầu bồi thường điện tử hiện có, đồng thời giảm thiểu sự can thiệp thủ công. Trong bài viết này, chúng tôi minh họa cách các chương trình bảo hiểm y tế có thể tự động hóa quy trình chuyển đổi từ yêu cầu bồi thường giấy sang điện tử bằng cách sử dụng [AWS Transfer Family web apps](https://aws.amazon.com/aws-transfer-family/), [Amazon Bedrock Data Automation (BDA)](https://aws.amazon.com/bedrock/bda/), và  [AWS B2B Data Interchange](https://aws.amazon.com/b2b-data-interchange/). Mặc dù giải pháp này tập trung vào việc chuyển đổi biểu mẫu yêu cầu bồi thường bệnh viện UB-04 sang định dạng 5010 X12 837 (chuẩn điện tử được HIPAA quy định), nhưng cùng kiến trúc này cũng có thể được áp dụng để tự động hóa quy trình dựa trên giấy tờ trong các ngành khác như bảo hiểm, chuỗi cung ứng, bán lẻ và sản xuất, nơi doanh nghiệp cần biến đổi tài liệu vật lý thành dữ liệu điện tử chuẩn EDI.

### **Tổng quan giải pháp**

Giải pháp này biến đổi các yêu cầu bồi thường chăm sóc sức khỏe từ dạng giấy sang giao dịch điện tử chuẩn hóa thông qua quy trình ba giai đoạn tinh gọn sử dụng kiến trúc serverless của AWS, như minh họa trong hình dưới đây. Việc kết hợp xử lý tài liệu, trích xuất dữ liệu bằng AI, và khả năng chuyển đổi EDI cho phép các chương trình bảo hiểm loại bỏ việc nhập dữ liệu thủ công trong khi vẫn duy trì tuân thủ HIPAA.

Hình sau minh họa kiến trúc:

![][image1]

*Hình 1\. Sơ đồ kiến ​​trúc thể hiện sự kiện chuyển đổi dữ liệu yêu cầu bồi thường giấy tờ thành 837 giao dịch*

Quy trình tổng thể (end-to-end workflow) như sau:

### **Giai đoạn 1: Tiếp nhận tệp an toàn với ứng dụng web AWS Transfer Family**

1. Nhóm vận hành xử lý yêu cầu bồi thường (claims operations teams) tải lên các bản scan của yêu cầu bồi thường giấy thông qua giao diện ứng dụng web Transfer Family, ứng dụng này cung cấp quyền truy cập bảo mật dựa trên trình duyệt tới các bucket [Amazon S3](https://aws.amazon.com/s3/) .

### **Giai đoạn 2: Trích xuất dữ liệu thông minh với BDA**

2. Amazon S3 phát hiện các tệp mới được tải lên **tiền tố bucket đầu vào** và phát ra một sự kiện [Amazon EventBridge](https://aws.amazon.com/eventbridge/) tương ứng. Các quy tắc của Amazon EventBridge phát hiện các sự kiện này và kích hoạt các hàm  [AWS Lambda](https://aws.amazon.com/lambda/)  để xử lý yêu cầu bồi thường.

3. Hàm Lambda gọi BDA để trích xuất thông minh dữ liệu có cấu trúc từ các biểu mẫu yêu cầu bồi thường.

### **Giai đoạn 3: Chuyển đổi và tạo EDI với AWS B2B Data Interchange**

4. B2B Data Interchange theo dõi vị trí S3 để tìm các tài liệu JSON mới và tự động chuyển đổi dữ liệu đã được trích xuất thành các giao dịch 837 EDI chuẩn hóa.

5. Các tệp EDI kết quả được đưa vào một bucket S3, sẵn sàng để tích hợp với hệ thống xử lý yêu cầu bồi thường hiện có của chương trình bảo hiểm y tế.

### Kiến trúc này mang lại các kết quả kinh doanh quan trọng tác động trực tiếp đến lợi nhuận của chương trình bảo hiểm y tế:

* Giảm tới 80% chi phí xử lý mỗi yêu cầu bồi thường nhờ trích xuất dữ liệu thông minh và xử lý tự động

* Rút ngắn thời gian xử lý từ nhiều ngày xuống còn vài phút, tăng tốc chu kỳ hoàn trả cho nhà cung cấp dịch vụ y tế

* Cải thiện độ chính xác dữ liệu với tỷ lệ lỗi thấp hơn so với xử lý thủ công

* Nâng cao tuân thủ HIPAA thông qua mã hóa đầu-cuối, kiểm soát truy cập chi tiết và ghi nhật ký kiểm toán

* Tăng hiệu quả vận hành, cho phép nhân viên tập trung vào các hoạt động dịch vụ thành viên có giá trị cao thay vì nhập dữ liệu

* Khả năng mở rộng liền mạch từ hàng trăm lên đến hàng triệu yêu cầu bồi thường mỗi năm mà không cần lo ngại về lập kế hoạch dung lượng

Tất cả các dịch vụ được sử dụng trong giải pháp này đều là dịch vụ đủ điều kiện cho HIPAA ([HIPAA Eligible Services](https://aws.amazon.com/compliance/hipaa-eligible-services-reference/)), nghĩa là chúng đủ điều kiện cho các khối lượng công việc liên quan đến thông tin sức khỏe cá nhân được bảo vệ điện tử (ePHI) khi được bao phủ bởi Thỏa thuận Đối tác Kinh doanh ( [Business Associate Addendum (BAA)](https://aws.amazon.com/compliance/hipaa-compliance/)) với AWS.

Không giống như các giải pháp OCR và EDI truyền thống vốn yêu cầu đầu tư ban đầu lớn, cách tiếp cận serverless này mang lại hiệu suất ổn định trong giai đoạn cao điểm xử lý, đồng thời giảm thiểu chi phí quản lý hạ tầng.

Trong các phần tiếp theo, chúng tôi sẽ khám phá chi tiết từng giai đoạn của quy trình này, xem xét cách các dịch vụ AWS phối hợp với nhau để tạo ra trải nghiệm xử lý yêu cầu bồi thường liền mạch, đồng thời giải quyết các thách thức vận hành chính: giảm lỗi xử lý thủ công, tăng tốc chu kỳ hoàn trả, và cắt giảm chi phí.

### **Giai đoạn 1: Tiếp nhận tệp an toàn với ứng dụng web AWS Transfer Family**

Thách thức đầu tiên trong việc tự động hóa xử lý yêu cầu bồi thường giấy là tiếp nhận tài liệu an toàn, thân thiện với người dùng. Nhóm vận hành cần một cách rõ ràng và an toàn để tải lên các bản số hóa của yêu cầu bồi thường trên giấy. **Transfer Family web apps** là một ứng dụng web được quản lý toàn phần, không cần code, dựa trên trình duyệt, cho phép người dùng đã xác thực có thể liệt kê, tải lên, tải xuống, sao chép, và xóa tệp trong các bucket Amazon S3 cụ thể. Giải pháp này cung cấp cho nhóm vận hành một giao diện **zero-code** để nộp các biểu mẫu chăm sóc sức khỏe một cách an toàn.

Dịch vụ này xác thực người dùng thông qua [AWS Identity and Access Management (IAM) Identity Center](https://aws.amazon.com/iam/identity-center/), cho phép nhân viên sử dụng thông tin đăng nhập tổ chức hiện có của họ thông qua các giao thức chuẩn công nghiệp (SAML 2.0 hoặc OIDC). Sau khi xác thực, người dùng truy cập vào một cổng web tùy chỉnh và có thương hiệu nơi họ có thể tải lên các biểu mẫu yêu cầu bồi thường (CMS-1500, UB-04, ADA dental hoặc biểu mẫu tùy chỉnh) bằng thao tác kéo-thả trực quan, như minh họa trong Hình 2\.

![][image2]

*Hình 2\. Giao diện ứng dụng web Transfer Family hiển thị các tùy chọn quản lý tệp*

Giao diện web không chỉ đơn giản hóa quy trình tải lên mà còn cung cấp khả năng kiểm tra, ghi lại từng thao tác trong các bản ghi kiểm tra toàn diện, hỗ trợ các yêu cầu tuân thủ HIPAA. Người dùng có thể thực hiện cả tải lên riêng lẻ và hàng loạt tùy theo nhu cầu công việc. Hình 3 minh họa cách lưu trữ các biểu mẫu UB-04 đã tải lên.

![][image3]  
*Hình 3\. Tải biểu mẫu UB-04 lên thư mục đến*

**Bảo mật được duy trì xuyên suốt toàn bộ quy trình:** tất cả các tệp đều được mã hóa trong quá trình truyền tải bằng **SSL/TLS** và khi lưu trữ bằng  [AWS Key Management Service (AWS KMS)](https://aws.amazon.com/kms/) trong Amazon S3. Khi các biểu mẫu mới đến trong **tiền tố (prefix) chỉ định cho input claims**, một quy tắc EventBridge sẽ tự động kích hoạt một hàm Lambda, chuyển chúng sang **Giai đoạn 2** của pipeline, nơi BDA thực hiện việc trích xuất dữ liệu thông minh.

### **Giai đoạn 2: Trích xuất dữ liệu thông minh với BDA**

Sau khi tiếp nhận an toàn, thách thức quan trọng tiếp theo là trích xuất chính xác dữ liệu có cấu trúc từ các biểu mẫu y tế phức tạp. Bedrock Data Automation (BDA) là một tính năng được quản lý toàn phần trong [Amazon Bedrock](https://aws.amazon.com/bedrock/), giúp đơn giản hóa việc trích xuất thông tin giá trị từ nội dung phi cấu trúc đa phương thức như tài liệu, hình ảnh, âm thanh, và video. BDA cung cấp một trải nghiệm API thống nhất, loại bỏ nhu cầu quản lý nhiều mô hình AI và dịch vụ khác nhau, đồng thời tích hợp sẵn các chức năng visual grounding (định vị trực quan) và confidence scores (điểm tin cậy) để xác thực dữ liệu được trích xuất.

Đối với loại tài liệu và hình ảnh, BDA cho phép bạn định nghĩa khi nào dữ liệu cần được trích xuất nguyên gốc ([standard](https://docs.aws.amazon.com/bedrock/latest/userguide/bda-standard-output.html)) và khi nào cần được suy luận ([custom output](https://docs.aws.amazon.com/bedrock/latest/userguide/bda-custom-output-idp.html)), giúp kiểm soát hoàn toàn quá trình. Sự linh hoạt này được cung cấp thông qua các cấu hình blueprint.

**Trích xuất dữ liệu y tế với các blueprint trong BDA**

Trong BDA, blueprint định nghĩa cách trích xuất và xử lý dữ liệu từ tài liệu. Mỗi blueprint bao gồm các định nghĩa trường dữ liệu (field definitions), hướng dẫn (instructions), quy tắc xác thực (validation rules), và đặc tả lược đồ đầu ra (output schema specifications), tất cả đều có thể được điều chỉnh phù hợp với các loại biểu mẫu y tế khác nhau. Blueprint có thể được tạo bằng prompt ngôn ngữ tự nhiên trong console, hoặc định nghĩa thủ công để có mức độ kiểm soát chính xác hơn.

Các nhóm kỹ thuật nên tạo các blueprint riêng cho CMS-1500, UB-04, biểu mẫu nha khoa ADA và các biểu mẫu tùy chỉnh, và tinh chỉnh chúng theo thời gian khi cần thiết. Mỗi blueprint chỉ rõ chính xác những phần tử dữ liệu nào cần được trích xuất từ từng loại biểu mẫu và cách chúng cần được cấu trúc.

Hình 4 minh họa một cấu hình blueprint cho biểu mẫu UB-04 trong AWS console, nơi bạn có thể định nghĩa các trường như thông tin bệnh nhân, mã chẩn đoán, và chi tiết thủ thuật.

![][image4]

*Hình 4\. Trích xuất biểu mẫu UB-04 trong bảng điều khiển AWS hiển thị hướng dẫn cấp trường, kết quả và điểm tin cậy*

Khi xử lý biểu mẫu yêu cầu bồi thường, hàm Lambda được kích hoạt bởi EventBridge sẽ gọi các API BDA để phân tích tài liệu, phân loại thành một loại biểu mẫu cụ thể, áp dụng bản thiết kế trích xuất phù hợp và trích xuất dữ liệu chăm sóc sức khỏe có liên quan. Tài liệu JSON có cấu trúc kết quả chứa tất cả thông tin từ yêu cầu bồi thường giấy tờ gốc, sẵn sàng cho quá trình xử lý tiếp theo.

Hình 5 hiển thị đầu ra JSON có cấu trúc do Amazon Bedrock Data Automation tạo ra từ biểu mẫu UB-04. Việc chuyển đổi này từ dữ liệu tài liệu phi cấu trúc sang các trường có cấu trúc, có thể đọc được bằng máy là rất quan trọng cho quá trình xử lý tiếp theo, với mỗi cặp khóa-giá trị đại diện cho thông tin yêu cầu bồi thường quan trọng được trích xuất từ ​​biểu mẫu giấy tờ.

![][image5]  
*Hình 5\. Tài liệu JSON với các trường được trích xuất từ ​​biểu mẫu UB-04 bằng BDA*

Sau đây là một ví dụ về dữ liệu được trích xuất trong JSON từ một tài liệu UB-04 mẫu, nêu bật một số trường chính:

JSON  
{  
    "claim": {  
        "admit\_diagnosis": "E871",  
        "statement\_covers\_period": "01012024 through 03012024",  
        "patient\_birthdate": "08221967",  
        "provider\_address": "555 Main St",  
        "npi": "0011002200"  
	}  
}

Khi việc trích xuất hoàn tất, hàm Lambda sẽ ghi tài liệu JSON có cấu trúc này vào một tiền tố (prefix) Amazon S3 được B2B Data Interchange giám sát, tạo ra sự chuyển tiếp liền mạch sang giai đoạn chuyển đổi EDI mà không cần can thiệp thủ công.

### **Giai đoạn 3: Chuyển đổi và tạo EDI với AWS B2B Data Interchange**

Dữ liệu có cấu trúc được trích xuất từ các yêu cầu bồi thường trên giấy có nghĩa là bước quan trọng cuối cùng là chuyển đổi thông tin này thành các giao dịch HIPAA 5010 X12 837 tiêu chuẩn, có thể được xử lý bởi các hệ thống bồi thường hiện có. Electronic Data Interchange (EDI) là định dạng chuẩn được sử dụng trong toàn bộ lĩnh vực chăm sóc sức khỏe cho các giao dịch điện tử giữa nhà cung cấp dịch vụ, đơn vị chi trả, và các bên liên quan khác.

B2B Data Interchange tự động hóa việc chuyển đổi và tạo ra các tài liệu Electronic Interchange Data (EDI) từ và sang các định dạng dữ liệu JSON và XML. Dịch vụ này cung cấp cho các gói bảo hiểm y tế một giao diện low-code cùng mô hình tính phí theo mức sử dụng (pay-as-you-go), giúp giảm thời gian, độ phức tạp, và chi phí liên quan đến việc quản lý và trao đổi dữ liệu giao dịch xuyên suốt các ranh giới tổ chức.

**Hiểu về quy trình chuyển đổi EDI**

B2B Data Interchange đơn giản hóa quá trình chuyển đổi từ dữ liệu UB-04 JSON đã được trích xuất sang các giao dịch 837 Institutional tiêu chuẩn thông qua bốn thành phần:

* **Profiles**: Lưu trữ chi tiết kinh doanh và thông tin liên hệ của tổ chức bạn. Thông tin này được chia sẻ với các đối tác giao dịch.

* **Transformers**: Xác định cách dữ liệu JSON được trích xuất từ BDA ánh xạ sang các phân đoạn EDI tiêu chuẩn trong định dạng 837\.

* **Trading capabilities**: Thiết lập các pipeline xử lý tự động để giám sát các vị trí S3 và kích hoạt quá trình chuyển đổi.

* **Partnerships**: Kết nối các thành phần lại với nhau để cho phép trao đổi tài liệu tự động.

Một đổi mới quan trọng trong B2B Data Interchange là khả năng ánh xạ EDI được hỗ trợ bởi AI sinh (generative AI), được vận hành bởi [Amazon Bedrock](https://aws.amazon.com/bedrock/). Tính năng “Generate Mapping” được hiển thị trong Hình 6 phân tích dữ liệu yêu cầu bồi thường đã được trích xuất và các giao dịch EDI mẫu để tự động đề xuất các ánh xạ phù hợp, rút ngắn thời gian triển khai từ vài ngày xuống còn vài phút, đồng thời giúp quá trình chuyển đổi EDI trở nên dễ tiếp cận hơn đối với các nhóm không có chuyên môn chuyên sâu.

![][image6]

*Hình 6\. Trao đổi dữ liệu AWS B2B với khả năng tạo bản đồ*

Sau khi được cấu hình, toàn bộ quy trình chuyển đổi sẽ được tự động hóa:

1. B2B Data Interchange giám sát tiền tố Amazon S3 cho các tài liệu JSON mới từ BDA  
2. Khi được phát hiện, nó sẽ chuyển đổi dữ liệu JSON có cấu trúc thành các tài liệu X12 837  
3. Tùy chọn xác thực các tài liệu đã chuyển đổi theo các tiêu chuẩn X12  
4. Các tệp EDI kết quả được chuyển đến một thư mục đầu ra được chỉ định để tích hợp

Sau đây là một đoạn mẫu từ tệp X12 được tạo, hiển thị các thành phần chính của yêu cầu chăm sóc sức khỏe được chuẩn hóa:

ST\*837\*0001\*005010\~ (Transaction start segment)

BHT\*0019\*00\*0123\*20250422\*2020\*CH\~ (Header information)

NM1\*41\*2\*Acme Hospital\*\*\*\*\*46\*12345\~ (Provider data)

PER\*IC\*JOHN DOE\*TE\*1234567890\~ (Contact information)

Đầu ra cuối cùng của quy trình này là một tập hợp các tệp EDI được chuẩn hóa, sẵn sàng tích hợp với hệ thống xử lý yêu cầu bồi thường hiện có của bạn. Mỗi tệp chứa tất cả thông tin chăm sóc sức khỏe có cấu trúc được trích xuất từ ​​biểu mẫu giấy gốc. Các tệp này tuân theo quy ước đặt tên chuẩn và được sắp xếp theo ID đối tác thương mại để dễ dàng tích hợp.

![][image7]

*Hình 7\. HIPAA 5010 837 X12 được ghi vào tiền tố đầu ra của thùng Amazon S3*

Tất cả các hoạt động chuyển đổi đều được ghi log vào [Amazon CloudWatch](https://aws.amazon.com/cloudwatch/) và phát sự kiện tới Amazon EventBridge, cho phép giám sát toàn diện về số lượng tài liệu, trạng thái và lỗi. Điều này giúp các nhóm vận hành có thể theo dõi số lượng tài liệu, trạng thái chuyển đổi, và lỗi xử lý thông qua các bảng điều khiển CloudWatch tùy chỉnh.

### **Bước tiếp theo**

Giờ bạn đã hiểu cách giải pháp hoạt động, bạn có thể triển khai nó trong môi trường AWS của mình. Trước khi khởi chạy stack CloudFormation, hãy hoàn thành các điều kiện tiên quyết sau:

#### **Điều kiện tiên quyết**

* Cấu hình AWS IAM Identity Center

  * Kích hoạt AWS IAM Identity Center theo [tài liệu hướng dẫn](https://docs.aws.amazon.com/singlesignon/latest/userguide/enable-identity-center.html#to-enable-identity-center-instance).

  * Cấu hình nguồn định danh của bạn (AWS Managed hoặc IdP bên ngoài).

  * Tạo một nhóm có tên ‘ClaimsOperationsTeam’ theo [hướng dẫn khởi động nhanh](https://docs.aws.amazon.com/singlesignon/latest/userguide/quick-start-default-idc.html).

  * Ghi lại Group ID trong phần General Information.

  * Cấu hình cài đặt MFA khi cần.

* Kích hoạt Access Grants trong vùng (region) của bạn

  * Làm theo [tài liệu](https://docs.aws.amazon.com/AmazonS3/latest/userguide/access-grants-instance-create.html) để tạo một access grant instance và liên kết nó với ARN của IdC.

### **Triển khai giải pháp**

Sử dụng nút **“**Launch Stack**”** bên dưới để triển khai template [AWS CloudFormation](https://aws.amazon.com/cloudformation/) vào tài khoản AWS của bạn.

[**![][image8]**](https://us-east-1.console.aws.amazon.com/cloudformation/home?region=us-east-1#/stacks/quickcreate?templateURL=https://awsstorageblogresources.s3.us-west-2.amazonaws.com/blog1555/paper_claim_automation_transfer_family.yaml)

Khi được nhắc, hãy cung cấp các tham số sau:

* BusinessName: Tên công ty của bạn

* GranteeIdentifier: Group ID từ bước 1

* IdentityCenterInstanceArn: ARN của Identity Center (định dạng: arn:aws:sso:::instance/ssoins-xxxxxxxxxxxxxxxx)

* InboundPath: inbound/

* NamePrefix: paper-claims

* OutboundPath: outbound/

### **Xác minh việc triển khai**

Sau khi việc triển khai hoàn tất:

1. Gán nhóm **‘**ClaimsOperationsTeam**’** cho web app trong [AWS Transfer Family console](https://console.aws.amazon.com/transfer/home).

2. Truy cập URL web app và đăng nhập.

3. Tải lên một tệp UB-04 mẫu vào thư mục **inbound** (tải [tệp mẫu](https://awsstorageblogresources.s3.us-west-2.amazonaws.com/blog1555/example-paper-claim-ub04.pdf)).

4. Xác minh rằng kết quả EDI xuất hiện trong thư mục outbound.

---

### **Dọn dẹp**

Để tránh phát sinh chi phí trong tương lai, hãy xóa stack CloudFormation. Để biết hướng dẫn, tham khảo mục [xóa stack](https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/cfn-console-delete-stack.html) trên [CloudFormation console](https://console.aws.amazon.com/cloudformation/?pg=ln&cp=bn).

### **Kết luận**

Trong bài viết này, chúng tôi đã trình bày cách chuyển đổi các yêu cầu bồi thường y tế trên giấy thành định dạng điện tử bằng cách sử dụng các dịch vụ AWS. Giải pháp này giúp các gói bảo hiểm y tế cắt giảm chi phí xử lý tới 80%, rút ngắn thời gian xử lý từ nhiều ngày xuống chỉ còn vài phút, và cải thiện độ chính xác dữ liệu — tất cả đều tuân thủ các yêu cầu của HIPAA. Bằng cách tự động hóa các quy trình thủ công tốn nhiều công sức này, các tổ chức có thể chuyển hướng nguồn lực quý giá khỏi việc nhập liệu thủ công sang các sáng kiến tập trung vào thành viên. Hãy bắt đầu hành trình tự động hóa xử lý yêu cầu bồi thường với template CloudFormation của chúng tôi và xem cách nó có thể giải quyết các thách thức xử lý cụ thể của bạn.

THẺ: [Amazon Simple Storage Service (Amazon S3)](https://aws.amazon.com/blogs/storage/tag/amazon-simple-storage-service-amazon-s3/), [AWS Cloud Storage](https://aws.amazon.com/blogs/storage/tag/aws-cloud-storage/), [AWS Lambda](https://aws.amazon.com/blogs/storage/tag/aws-lambda/), [AWS Transfer Family](https://aws.amazon.com/blogs/storage/tag/aws-transfer-family/)

### Anil Chinnam

Anil Chinnam là Kiến trúc sư Giải pháp tại AWS. Anh là một người đam mê AI Sáng tạo, tâm huyết với việc chuyển đổi các công nghệ tiên tiến thành giá trị kinh doanh hữu hình cho khách hàng chăm sóc sức khỏe. Là một cố vấn kỹ thuật đáng tin cậy, anh giúp khách hàng thúc đẩy việc áp dụng công nghệ đám mây và kết quả chuyển đổi kinh doanh.

####   

### Chris Bladon

Chris Bladon là Kiến trúc sư Giải pháp Cấp cao tại AWS. Anh chuyên hỗ trợ các đơn vị thanh toán dịch vụ chăm sóc sức khỏe và khách hàng trong lĩnh vực khoa học đời sống xây dựng các giải pháp sáng tạo bằng cách sử dụng các dịch vụ AI tạo sinh của AWS như Amazon Bedrock và Amazon Q Developer.