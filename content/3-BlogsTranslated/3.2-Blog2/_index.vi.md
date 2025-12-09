---
title: "Blog 2"
date: "2025-11-30"
weight: 1
chapter: false
pre: " <b> 3.2. </b> "
---



---
title: "Blog 2"
date: "2025-11-30"
weight: 1
chapter: false
pre: " <b> 3.2. </b> "
---

## [**AWS Web3 Blog**](https://aws.amazon.com/blogs/web3/)

 **Sử dụng DAO để quản trị dữ liệu huấn luyện LLM, Phần 3: Từ IPFS đến cơ sở tri thức**  
tác giả: Shankar Subramaniam | ngày 01/11/2024 | trong mục [Advanced (300)](https://aws.amazon.com/blogs/web3/category/learning-levels/advanced-300/), [Amazon Bedrock](https://aws.amazon.com/blogs/web3/category/artificial-intelligence/amazon-machine-learning/amazon-bedrock/), [Artificial Intelligence](https://aws.amazon.com/blogs/web3/category/artificial-intelligence/), [Blockchain](https://aws.amazon.com/blogs/web3/category/blockchain/), [Generative AI](https://aws.amazon.com/blogs/web3/category/artificial-intelligence/generative-ai/), [Technical How-to](https://aws.amazon.com/blogs/web3/category/post-types/technical-how-to/) | [Permalink](https://aws.amazon.com/blogs/web3/use-a-dao-to-govern-llm-training-data-part-3-from-ipfs-to-the-knowledge-base/) |  [Share](https://aws.amazon.com/vi/blogs/web3/use-a-dao-to-govern-llm-training-data-part-3-from-ipfs-to-the-knowledge-base/#)

Trong [Phần 1](https://aws.amazon.com/blogs/database/part-1-use-a-dao-to-govern-llm-training-data-retrieval-augmented-generation/) của loạt bài này, chúng tôi đã giới thiệu khái niệm sử dụng một [tổ chức tự trị phi tập trung](https://ethereum.org/en/dao/) (DAO) để quản trị vòng đời của một mô hình AI, tập trung vào việc **tiếp nhận dữ liệu huấn luyện**. Chúng tôi đã phác thảo kiến trúc tổng thể, thiết lập một **cơ sở tri thức LLM** với [Amazon Bedrock](https://aws.amazon.com/bedrock/), và đồng bộ nó với Ethereum Improvement Proposals (EIPs).

Trong [Phần 2](https://aws.amazon.com/blogs/database/part-2-use-a-dao-to-govern-llm-training-data-the-smart-contract/), chúng tôi đã tạo và triển khai một hợp đồng thông minh tối giản trên mạng thử nghiệm Ethereum Sepolia bằng Remix và MetaMask, qua đó thiết lập một cơ chế để quản trị việc dữ liệu huấn luyện nào có thể được tải lên cơ sở tri thức và ai có quyền thực hiện.

Trong bài viết này, chúng tôi sẽ thiết lập  [Amazon API Gateway](https://aws.amazon.com/api-gateway) và triển khai các hàm [AWS Lambda](http://aws.amazon.com/lambda) để sao chép dữ liệu từ InterPlanetary File System (IPFS) sang [Amazon Simple Storage Service](http://aws.amazon.com/s3)  (Amazon S3) và khởi chạy một tác vụ nạp dữ liệu vào cơ sở tri thức.

### **Tổng quan giải pháp**

Trong Phần 2, chúng tôi đã tạo một hợp đồng thông minh chứa các định danh file IPFS và các địa chỉ Ethereum được phép tải nội dung của những file IPFS đó để huấn luyện mô hình.

Trong bài này, chúng tôi tập trung vào con đường mà dữ liệu có thể đi từ một nhà cung cấp dữ liệu đáng tin cậy (chủ sở hữu của một trong những địa chỉ Ethereum đó) đến cơ sở tri thức LLM. Sơ đồ sau minh họa luồng dữ liệu.

![][image1]

Chúng ta sẽ tạo các thành phần sau để triển khai luồng dữ liệu này:

* Một hàm Lambda gọi là s32kb, dùng để làm mới nội dung của cơ sở tri thức khi có nội dung mới được thêm vào S3 bucket

* Một trigger S3 để gọi hàm Lambda s32kb

* Một hàm Lambda gọi là ipfs2s3, dùng để tải nội dung từ IPFS vào S3 bucket

* Một API Gateway để gọi hàm ipfs2s3

### **Điều kiện tiên quyết**

Xem lại các điều kiện tiên quyết đã nêu trong [Phần 1](https://aws.amazon.com/blogs/database/part-1-use-a-dao-to-govern-llm-training-data-retrieval-augmented-generation/) của loạt bài này, và hoàn thành các bước trong [Phần 1](https://aws.amazon.com/blogs/database/part-1-use-a-dao-to-govern-llm-training-data-retrieval-augmented-generation/) và [Phần 2](https://aws.amazon.com/blogs/database/part-2-use-a-dao-to-govern-llm-training-data-the-smart-contract/) để thiết lập các thành phần cần thiết của giải pháp.

### **Thiết lập hàm Lambda s32kb**

Trong phần này, chúng ta sẽ đi qua các bước để thiết lập hàm s32kb.

#### **Tạo IAM role cho s32kb**

Trước khi tạo hàm Lambda, bạn cần tạo một [AWS Identity and Access Management](https://aws.amazon.com/iam/) (IAM) **role** mà hàm Lambda sẽ sử dụng trong quá trình thực thi. Thực hiện các bước sau:

1. Mở terminal [AWS CloudShell](https://aws.amazon.com/cloudshell/) và tải lên các file sau:

   * [s32kb\_trust\_policy.json](https://aws-blogs-artifacts-public.s3.us-east-1.amazonaws.com/DBBLOG-3971/s32kb_trust_policy.json) – trust policy dùng để tạo role cho hàm s32kb

   * [s32kb\_inline\_policy\_template.json](https://aws-blogs-artifacts-public.s3.us-east-1.amazonaws.com/DBBLOG-3971/s32kb_inline_policy_template.json) – inline policy dùng để tạo role cho hàm s32kb

   * [s32kb.py](https://aws-blogs-artifacts-public.s3.us-east-1.amazonaws.com/DBBLOG-3971/s32kb.py) – script Python để tạo một hàm Lambda tự động cập nhật cơ sở tri thức khi có file mới được tải lên S3 bucket

   * [s32kb.py.zip](https://aws-blogs-artifacts-public.s3.us-east-1.amazonaws.com/DBBLOG-3971/s32kb.py.zip) – file .zip chứa mã nguồn Lambda ([s32kb.py](http://s32kb.py))

2. Tạo một tài liệu JSON cho inline policy (policy này cần thiết để cấp quyền cho hàm Lambda ghi log vào [Amazon CloudWatch](http://aws.amazon.com/cloudwatch)):  
   ACCOUNT=$(aws sts get-caller-identity \--query "Account" \--output text) && \\  
   cat s32kb\_inline\_policy\_template.json | sed \-e s+ACCOUNT+$ACCOUNT+g \> s32kb\_inline\_policy.json  
     
     
     
3. Tạo IAM role:   
   aws iam create-role \\  
   \--role-name s32kb \\  
   \--assume-role-policy-document file://s32kb\_trust\_policy.json  
4. Gắn managed policy **AmazonBedrockFullAccess**:  
   aws iam attach-role-policy \--role-name s32kb \--policy-arn arn:aws:iam::aws:policy/AmazonBedrockFullAccess

5. Gắn inline policy vừa tạo:  
   aws iam put-role-policy \\  
   \--role-name s32kb \\  
   \--policy-name s32kb\_inline\_policy \\  
   \--policy-document file://s32kb\_inline\_policy.json  
   

#### **Tạo hàm Lambda s32kb**

Thực hiện các bước sau để tạo hàm s32kb:

1. Mở file s32kb.py bằng trình soạn thảo ưa thích (ở đây dùng vi) và xem nội dung bên trong.

    File này khởi tạo một Amazon Bedrock agent, và sử dụng agent này để bắt đầu một knowledge base ingestion job. Ngoài ra, cần thiết lập hai biến môi trường:

   * KB\_ID – chứa ID của cơ sở tri thức

   * KB\_DATA\_SOURCE\_ID – chứa ID của nguồn dữ liệu (S3 bucket)

2. Thực hiện các bước sau để tra cứu các giá trị đó:

   * Trên console Amazon Bedrock, chọn **Knowledge bases** trong bảng điều hướng.

   * Chọn cơ sở tri thức crypto-ai-kb.

   * Ghi lại ID của cơ sở tri thức trong phần **Knowledge base overview**.

   * Trong mục **Data source**, chọn EIPs data source.

   * Ghi lại ID của nguồn dữ liệu trong phần **Data source overview**.

3. Lưu lại các giá trị này trong CloudShell:

export KB\_ID=*\<Knowledge base ID\>*

export KB\_DATA\_SOURCE\_ID=*\<Data source ID\>*

4. Tạo hàm Lambda:

ACCOUNT=$(aws sts get-caller-identity \--query "Account" \--output text) && \\

aws lambda create-function \\

\--function-name s32kb \\

\--timeout 300 \\

\--runtime python3.12 \\

\--architectures x86\_64 \\

\--zip-file fileb://s32kb.py.zip \\

\--handler s32kb.handler \\

\--role arn:aws:iam::$ACCOUNT:role/s32kb \\

\--environment  Variables=\\{KB\_ID=$KB\_ID,KB\_DATA\_SOURCE\_ID=$KB\_DATA\_SOURCE\_ID\\}

### **Tạo một trigger S3 để gọi hàm Lambda s32kb**

Hoàn thành các bước sau để cấu hình một trigger nhằm chạy hàm s32kb tự động bất cứ khi nào có file mới được tải lên S3 bucket:	

1. Trên bảng điều khiển Lambda, chọn **Functions** trong bảng điều hướng.

2. Chọn hàm s32kb.

3. Chọn **Add trigger**.

4. Trong phần **Trigger configuration**, tại **Select a source**, chọn **S3**.

5. Với **Bucket**, chọn crypto-ai-kb-*\<your\_account\_id\>*

6. Với **Event types**, chọn **All object create events** và **All object delete events**.

7. Chọn ô xác nhận (acknowledgement check box).

8. Chọn **Add**.

---

### **Kiểm thử hàm Lambda s32kb**

Hãy thêm một file mới vào bucket và kiểm chứng rằng hàm Lambda đã được gọi thông qua trigger. Để tiếp nối nghiên cứu trước đó về **danksharding** trong [Phần 1](https://aws.amazon.com/blogs/database/part-1-use-a-dao-to-govern-llm-training-data-retrieval-augmented-generation/), chúng ta làm giàu cơ sở tri thức với tài liệu đặc tả nâng cấp mạng “Cancun”:

Mở terminal CloudShell và nhập các lệnh sau:

ACCOUNT=$(aws sts get-caller-identity \--query "Account" \--output text) && \\

wget https://github.com/ethereum/execution-specs/blob/master/network-upgrades/mainnet-upgrades/cancun.md && \\

aws s3 cp ./cancun.md s3://crypto-ai-kb-$ACCOUNT/

Kiểm tra rằng hàm Lambda đã được chạy thành công:

* Trên console CloudWatch, điều hướng đến/aws/Lambda/s32kb log group.

* Kiểm tra rằng có một log stream với **Thời gian sự kiện cuối cùng** trùng với thời gian hiện tại, và chọn nó.

* Xem log và xác nhận rằng hàm Lambda trả về 202 HTTPStatusCode .

Ngoài ra, hãy kiểm tra trạng thái của job Amazon Bedrock:

* Trên console Amazon Bedrock, điều hướng đến crypto-ai-kb knowledge base.

* Trong mục *Data source*, xác nhận rằng giá trị **Last sync time** trùng với thời gian hiện tại.

Nếu muốn đi xa hơn, bạn có thể truy vấn cơ sở tri thức về những thông tin được đề cập cụ thể trong đặc tả nâng cấp mạng này.

### **Thiết lập hàm Lambda ipfs2s3**

Trong phần này, chúng ta sẽ đi qua các bước để thiết lập hàm ipfs2s3.

#### **Tạo IAM role cho ipfs2s3**

Hoàn thành các bước sau để tạo IAM role cho ipfs2s3:

1. Mở terminal **CloudShell** và tải lên các file sau:

   * [ipfs2s3\_trust\_policy.json](https://aws-blogs-artifacts-public.s3.us-east-1.amazonaws.com/DBBLOG-3971/ipfs2s3_trust_policy.json)– trust policy dùng để tạo role.

   * [ipfs2s3\_inline\_policy\_template.json](https://aws-blogs-artifacts-public.s3.us-east-1.amazonaws.com/DBBLOG-3971/ipfs2s3_inline_policy_template.json) – inline policy dùng để tạo role.

   * [ipfs2s3.py](https://aws-blogs-artifacts-public.s3.us-east-1.amazonaws.com/DBBLOG-3971/ipfs2s3.py) – script Python để tạo hàm Lambda tải file từ IPFS lên S3 bucket.

   * [ipfs2s3.py.zip](https://aws-blogs-artifacts-public.s3.us-east-1.amazonaws.com/DBBLOG-3971/ipfs2s3.py.zip) – file .zip chứa mã Lambda (ipfs2s3.py).

2. Tạo một tài liệu JSON cho inline policy (policy này cần thiết để cấp quyền cho hàm Lambda ghi log vào CloudWatch):

ACCOUNT=$(aws sts get-caller-identity \--query "Account" \--output text) && \\

cat ipfs2s3\_inline\_policy\_template.json | sed \-e s+ACCOUNT+$ACCOUNT+g \> ipfs2s3\_inline\_policy.json

3. Tạo Lambda execution role:

aws iam create-role \\

\--role-name ipfs2s3 \\

\--assume-role-policy-document file://ipfs2s3\_trust\_policy.json

4. Gắn managed policy AmazonS3FullAccess:

aws iam attach-role-policy \--role-name ipfs2s3 \--policy-arn arn:aws:iam::aws:policy/AmazonS3FullAccess

5. Gắn inline policy vừa tạo:

aws iam put-role-policy \\

\--role-name ipfs2s3 \\

\--policy-name ipfs2s3\_inline\_policy \\

\--policy-document file://ipfs2s3\_inline\_policy.json

#### **Tạo hàm Lambda ipfs2s3**

Để tải một file từ mạng IPFS và upload lên Amazon S3, chúng ta tạo hàm Lambda ipfs2s3 kết nối đến một public IPFS gateway để tải xuống CID được truyền vào như một tham số sự kiện cho Lambda. Sau đó, hàm sẽ tải file đã tải xuống vào S3 bucket được cấu hình trong biến môi trường. Bạn cũng cung cấp tên file để sử dụng làm tham số sự kiện.

Để tăng tính bảo mật và độ tin cậy, bạn có thể tạo một **IPFS node (hoặc cluster)** trong môi trường của mình và cập nhật biến môi trường IPFS\_GW\_ENDPOINT để trỏ đến IPFS gateway riêng. Để biết hướng dẫn chi tiết về cách tạo hạ tầng IPFS riêng, tham khảo chuỗi  [IPFS on AWS](https://aws.amazon.com/blogs/database/part-1-ipfs-on-aws-discover-ipfs-on-a-virtual-machine/).

Mở file  [ipfs2s3.py](https://aws-blogs-artifacts-public.s3.us-east-1.amazonaws.com/DBBLOG-3971/ipfs2s3.py)  và xem nội dung. Sau đó, tạo hàm Lambda bằng lệnh sau:

ACCOUNT=$(aws sts get-caller-identity \--query "Account" \--output text) && \\

aws lambda create-function \\

\--function-name ipfs2s3 \\

\--timeout 30 \\

\--runtime python3.12 \\

\--architectures x86\_64 \\

\--zip-file fileb://ipfs2s3.py.zip \\

\--handler ipfs2s3.handler \\

\--role arn:aws:iam::$ACCOUNT:role/ipfs2s3 \\

\--environment Variables=\\{IPFS\_GW\_ENDPOINT=https://ipfs.io,S3\_TARGET\_BUCKET=crypto-ai-kb-$ACCOUNT\\}

### **Tạo một API Gateway**

Hoàn thành các bước sau để tạo API Gateway:

1. Trên **console API Gateway**, chọn **APIs** trong bảng điều hướng.

2. Chọn **Create API**.

3. Trong mục **HTTP API**, chọn **Build**.

4. Với **API name**, nhập một tên (ví dụ: crypto-ai).

5. Trong ***Create and configure integrations***, chọn ***Add integration***.

6. Chọn ***Lambda***.

7. Với **Lambda function**, chọn hàm ipfs2s3.

8. Với **Version**, chọn **2.0**.

9. Chọn **Next**.

10. Xem lại route mặc định và chọn **Next**.

11. Xem lại stage mặc định và chọn **Next**.

12. Chọn **Create**.

13. Mở API vừa tạo và ghi lại endpoint mặc định:

export API\_ENDPOINT=*\<Default endpoint\>*

### **Kiểm thử hàm Lambda ipfs2s3**

Giờ bạn có thể xác thực rằng mình có thể gọi hàm Lambda thông qua endpoint API Gateway.

Giả sử chúng ta muốn bổ sung cho cơ sở tri thức với [Amazon Managed Blockchain (AMB) – Ethereum Developer Guide](https://docs.aws.amazon.com/pdfs/managed-blockchain/latest/ethereum-dev/amazon-managed-blockchain-ethereum-dev.pdf).

1. Tải guide này lên một dịch vụ IPFS pinning như Filebase và ghi lại CID (CID của bạn có thể khác):

export CID=QmWGTo7gVXvdX2YWJg3hKH5JksXsL5tRBfiKLY9MxRbuLS && \\

export FILENAME=amazon-managed-blockchain-ethereum-dev.pdf

2. Gọi endpoint API:

curl $API\_ENDPOINT/ipfs2s3?cid=$CID\\\&filename=$FILENAME

Bạn sẽ nhận được một thông báo tương tự như sau:

"Successfully uploaded [amazon-managed-blockchain-ethereum-dev.pdf](https://console.filebase.com/object/crypto-ai/amazon-managed-blockchain-ethereum-dev.pdf) from CID QmWGTo7gVXvdX2YWJg3hKH5JksXsL5tRBfiKLY9MxRbuLS"

3. Kiểm tra rằng file amazon-managed-blockchain-ethereum-dev.pdf đã được upload lên S3 bucket và nguồn dữ liệu của knowledge base đã được đồng bộ lại.

Ngoài ra, bạn có thể truy vấn cơ sở tri thức về những thông tin được đề cập cụ thể trong guide này.

### **Dọn dẹp**

Bạn có thể giữ lại các thành phần đã xây dựng trong bài viết này vì sẽ tái sử dụng ở phần tiếp theo. Hoặc, bạn có thể làm theo hướng dẫn dọn dẹp trong Phần 4 để xóa chúng.

### **Kết luận**

Trong Phần 3 của loạt bài gồm bốn phần, chúng tôi đã trình bày cách tạo hai hàm Lambda và một API Gateway cho phép bạn tự động cập nhật một Amazon Bedrock knowledge base với dữ liệu từ mạng IPFS. Trong [Phần 4](https://aws.amazon.com/blogs/database/part-4-use-a-dao-to-govern-llm-training-data-metamask-authentication/) , chúng tôi sẽ trình bày cách tạo một frontend và sử dụng MetaMask để xác thực người dùng bằng danh tính web3 của họ.

### **Về các tác giả**

**Guillaume Goutaudier** là Kiến trúc sư Doanh nghiệp Cao cấp (Sr Enterprise Architect) tại AWS. Anh hỗ trợ các công ty xây dựng quan hệ đối tác kỹ thuật chiến lược với AWS. Anh cũng đam mê các công nghệ blockchain và là thành viên của Cộng đồng Kỹ thuật Thực địa (Technical Field Community) về blockchain.

			

**Shankar Subramaniam** là Kiến trúc sư Doanh nghiệp Cao cấp (Sr Enterprise Architect) trong Tổ chức Đối tác AWS (AWS Partner Organization), phụ trách các hoạt động hợp tác và quản trị trong khuôn khổ Strategic Partnership Collaboration and Governance (SPCG). Anh là thành viên của Cộng đồng Kỹ thuật Thực địa về Trí tuệ nhân tạo và Học máy (Artificial Intelligence and Machine Learning).