---
title: "Blog 3"
date: "2025-11-30"
weight: 1
chapter: false
pre: " <b> 3.3. </b> "
---

## [AWS Database Blog](https://aws.amazon.com/blogs/database/)

# Tiến hóa mô hình dữ liệu bảng Amazon DynamoDB của bạn

Bởi Imhoertha Ojior | Trong ngày 10 THÁNG 7 NĂM 2025 | Trong [Advanced (300)](https://aws.amazon.com/blogs/database/category/learning-levels/advanced-300/), [Amazon DynamoDB](https://aws.amazon.com/blogs/database/category/database/amazon-dynamodb/), [Technical How-to](https://aws.amazon.com/blogs/database/category/post-types/technical-how-to/) | [Permalink](https://aws.amazon.com/blogs/database/evolve-your-amazon-dynamodb-tables-data-model/) | [Comments](https://aws.amazon.com/blogs/database/evolve-your-amazon-dynamodb-tables-data-model/#Comments) | [Share](https://aws.amazon.com/vi/blogs/database/evolve-your-amazon-dynamodb-tables-data-model/#)

Hầu hết các ứng dụng phần mềm đều phát triển theo thời gian, thường yêu cầu thay đổi lược đồ (schema) cơ sở dữ liệu.  [Amazon DynamoDB](https://aws.amazon.com/dynamodb/) cung cấp thiết kế **không có lược đồ** – bạn chỉ cần định nghĩa các thuộc tính của khóa chính khi tạo bảng. Tính linh hoạt này cho phép bạn lưu trữ cả dữ liệu có cấu trúc và bán cấu trúc, giúp việc cập nhật mô hình dữ liệu có thể được triển khai mà không cần dừng dịch vụ.

Trong bài viết này, chúng tôi sẽ chỉ cho bạn cách **phát triển mô hình dữ liệu của bảng DynamoDB** để đáp ứng các yêu cầu thay đổi của ứng dụng trong khi vẫn duy trì **thời gian chết bằng 0** trong hệ thống sản xuất. Chúng tôi sẽ khám phá hai kỹ thuật chính kèm theo ví dụ mà bạn có thể áp dụng cho ứng dụng của riêng mình:

* Thêm thuộc tính mới

* Tạo thực thể mới

### **Thêm thuộc tính mới vào các item trong bảng**

Khi các tính năng của ứng dụng phát triển, có thể sẽ cần thêm các thuộc tính mới để lưu trữ dữ liệu cho những tính năng này. Tùy thuộc vào yêu cầu của tính năng, bạn có thể bắt đầu tạo các item mới mà không cần phải cập nhật các item hiện có trước.

Khi ứng dụng của bạn yêu cầu một thuộc tính mới trong mô hình dữ liệu của bảng mà không ảnh hưởng đến mẫu truy cập (access pattern) của ứng dụng, bạn có thể thêm thuộc tính đó vào các item mới được ghi vào bảng.

Nếu bạn có một mẫu truy cập phụ thuộc vào thuộc tính mới, bạn có thể tạo [chỉ mục phụ toàn cục](https://docs.aws.amazon.com/amazondynamodb/latest/developerguide/GSI.html) (GSI – [global secondary index](https://docs.aws.amazon.com/amazondynamodb/latest/developerguide/GSI.html)) sử dụng thuộc tính mới này trong khóa chính của nó. Các item sẽ được thêm vào GSI khi các item mới được tạo với thuộc tính mới hoặc khi các item hiện có được cập nhật để bao gồm thuộc tính này.

Hãy xem xét một bảng DynamoDB chứa đơn hàng của một trang web tạp hóa. Mỗi đơn hàng được định danh bởi một partition key (CustomerId) và một sort key (OrderId). Hình sau minh họa mô hình dữ liệu của bảng DynamoDB này.

![][image1]

Chúng tôi thêm một thuộc tính mới có tên là DeliveryDate và kích hoạt một mẫu truy cập mới bằng cách sử dụng một GSI có tên là DeliveryDate-Index. Điều này cho phép ứng dụng tìm kiếm đơn hàng dựa trên ngày giao hàng của đơn hàng. Hình sau đây minh họa mô hình dữ liệu cho bảng DynamoDB đã cập nhật.

![][image2]

Hình sau đây hiển thị mô hình dữ liệu cho DeliveryDate-Index GSI.

![][image3]

Với bản cập nhật này, chúng ta có thể truy xuất các đơn hàng được giao vào một ngày cụ thể từ bảng DynamoDB. Đoạn mã Python sau đây minh họa cách truy xuất đơn hàng từ bảng DynamoDB bằng ngày giao hàng:

try:  
    \# Query grocery orders by a delivery date  
    response \= client.query(  
        TableName\='Orders',  
        IndexName\='DeliveryDate-index',  
        KeyConditionExpression\='DeliveryDate \= :delivery\_date',  
        ExpressionAttributeValues\={  
            ':delivery\_date': { 'S': '2025-03-21' }  
        }  
    )  
    return response  
except Exception as e:  
    print(f"An error occurred: {str(e)}")  
    return \[\]

Đối với các item hiện có trong bảng mà chưa có thuộc tính mới, bạn có thể cập nhật ứng dụng của mình để chấp nhận trường hợp khi lấy dữ liệu về mà một số item không có thuộc tính này, sau đó dần dần thiết lập thuộc tính cho các item đó. Cách này có thể làm tăng độ phức tạp vận hành của bảng vì bạn sẽ phải quản lý nhiều phiên bản khác nhau của cùng một thực thể trong một khoảng thời gian dài.Ngoài ra, bạn có thể thực hiện cập nhật hàng loạt (bulk update) các item trong bảng DynamoDB để thiết lập thuộc tính mới. Phương án này nhanh hơn và ít phức tạp hơn về mặt vận hành, nhưng sẽ phát sinh chi phí do phải cập nhật toàn bộ item cần thuộc tính mới.

Sự đánh đổi ở đây nằm ở thời gian cần để cập nhật dần dần các item hiện có so với chi phí khi thực hiện cập nhật hàng loạt. Cách tiếp cận tốt nhất phụ thuộc vào trường hợp sử dụng cụ thể của bạn, kích thước bảng và yêu cầu của ứng dụng. Với những thuộc tính mới cần thiết cho hầu hết chức năng của ứng dụng, việc cập nhật các item hiện có trước khi tạo item mới là một lựa chọn hợp lý. Với những thuộc tính mới tùy chọn hoặc thuộc tính mới trong một bảng rất lớn, cách tiếp cận theo giai đoạn, bắt đầu với các item mới, có thể thực tế hơn.

Nếu ứng dụng của bạn yêu cầu tất cả item hiện có phải có thuộc tính mới trước khi tạo các item mới, bạn có thể cập nhật hàng loạt item trong bảng DynamoDB bằng [AWS Step Functions](https://aws.amazon.com/step-functions/) (xem [Bulk update Amazon DynamoDB tables with AWS Step Functions](https://aws.amazon.com/blogs/database/bulk-update-amazon-dynamodb-tables-with-aws-step-functions/)) hoặc [AWS Glue](https://aws.amazon.com/glue/).

Xem thêm tại  [Cost-effective bulk processing with Amazon DynamoDB](https://aws.amazon.com/blogs/database/cost-effective-bulk-processing-with-amazon-dynamodb/) để tìm hiểu cách thực hiện cập nhật hàng loạt tiết kiệm chi phí cho bảng DynamoDB.

### **Thêm một thực thể mới vào bảng của bạn**

Do DynamoDB có schema linh hoạt, bạn có thể lưu trữ và truy xuất hiệu quả các thực thể khác nhau trong cùng một bảng DynamoDB mà không cần thực hiện thao tác JOIN.

Khi ứng dụng của bạn yêu cầu thêm một thực thể mới vào mô hình dữ liệu của bảng, quy trình tương tự như việc thêm thuộc tính mới. Nếu các thuộc tính của thực thể mới không được ứng dụng sử dụng trong mẫu truy cập, bạn có thể thêm thực thể mới vào mô hình dữ liệu và sau đó thực hiện cập nhật hàng loạt bảng DynamoDB nếu cần.

Nếu một thuộc tính lồng nhau (nested attribute) trong một thực thể mới hoặc hiện có được ứng dụng sử dụng trong mẫu truy cập, bạn có thể tạo một thực thể mới trong bảng để hỗ trợ mẫu truy cập đó.

Hãy xem xét một bảng DynamoDB chứa đơn hàng của một trang web tạp hóa. Mỗi đơn hàng được định danh bởi partition key (CustomerId) và một  [composite sort key](https://docs.aws.amazon.com/amazondynamodb/latest/developerguide/data-modeling-blocks.html#data-modeling-blocks-composite) (OrderDate\#OrderId), và mỗi đơn hàng có một danh sách các mặt hàng tạp hóa đã mua. Hình dưới đây minh họa mô hình dữ liệu cho bảng DynamoDB này.

![][image4]

Khi trang web phát triển, chúng ta muốn cho phép khách hàng mua sắm từ các đơn hàng trước đó bằng cách đánh dấu các mặt hàng trong các đơn hàng trước làm mặt hàng yêu thích. Người mua sau đó có thể truy xuất danh sách các mặt hàng đã từng được gắn nhãn là yêu thích khi họ đặt các đơn hàng mới.

Để hỗ trợ những tính năng này trên trang web, chúng ta thêm một **thuộc tính Boolean lồng nhau Favourite** vào **thực thể Items** của bảng Orders. Điều này cho phép người mua đánh dấu các mặt hàng trong đơn hàng tạp hóa của họ thành các mặt hàng yêu thích.

Chúng ta cũng thêm một **thực thể mới** được định danh bằng **partition key (CustomerId)** và **composite sort key (FAVOURITE\#ItemId)** vào bảng Orders. Chúng ta sử dụng **composite sort key với tiền tố – FAVOURITE\#** cho thực thể mới này để có thể truy xuất tất cả các mặt hàng yêu thích của một CustomerId nhất định bằng cách sử dụng toán tử truy vấn phạm vi begins\_with.

Hãy tham khảo *DynamoDB Developer Guide* để tìm hiểu thêm về [**các best practice khi sử dụng sort key để tổ chức dữ liệu trong DynamoDB**.](https://docs.aws.amazon.com/amazondynamodb/latest/developerguide/bp-sort-keys.html)

Hình dưới đây minh họa mô hình dữ liệu đã được cập nhật cho bảng DynamoDB.

![][image5]

Với bản cập nhật này, chúng ta có thể gắn nhãn một mặt hàng tạp hóa trong đơn hàng là mặt hàng yêu thích khi ghi các đơn hàng mới vào bảng DynamoDB. Chúng ta cũng có thể truy xuất danh sách các mặt hàng yêu thích cho một người mua. Đoạn mã Python sau đây minh họa cách truy xuất các mặt hàng yêu thích từ bảng Orders:

try:

    \# Search for a customer’s favourite items

    response \= client.query(

        TableName\='Orders',

        KeyConditionExpression\='CustomerId \= :customer\_id AND begins\_with(SK, :sk)',

        ExpressionAttributeValues\={

            ':customer\_id': { 'S': '7970241400' },

            ':sk': { 'S': 'FAVOURITE\#' }

        }

    )

    return response

except Exception as e:

    print(f"An error occurred: {str(e)}")

    return \[\]

### **Các best practice khi cập nhật mô hình bảng DynamoDB của bạn**

Xem xét các điểm sau đây**:**

* **Xem xét chỉ project một tập con các thuộc tính của item sang index mới** – Khi thêm một GSI mới vào bảng DynamoDB, hãy cân nhắc chỉ project các thuộc tính mà bạn thực sự cần.  [projection](https://docs.aws.amazon.com/amazondynamodb/latest/developerguide/GSI.html#GSI.Projections)  đề cập đến các thuộc tính được sao chép từ bảng gốc sang index. Khi tạo GSI, bạn có thể chọn project các thuộc tính khóa của bảng, các thuộc tính cụ thể, hoặc toàn bộ thuộc tính của bảng gốc. Projection sang GSI không thể thay đổi sau khi index đã được tạo. Nếu cần phát triển data model của một GSI hiện có, bạn phải tạo một GSI mới. Các thuộc tính được project sang index ảnh hưởng đến chi phí lưu trữ và ghi dữ liệu của index.

* **Ghi lại các thay đổi trong mô hình dữ liệu của bảng** – Tài liệu hóa các thay đổi trong mô hình dữ liệu của bảng khi tính năng ứng dụng phát triển. Bao gồm thông tin chi tiết và lý do đằng sau mỗi thay đổi. Điều này giúp duy trì tính nhất quán cho dữ liệu được lưu trong DynamoDB.

* **Hoàn tất các kiểm tra toàn vẹn dữ liệu trong ứng dụng** – DynamoDB không duy trì mối quan hệ giữa các thực thể khác nhau được lưu trong bảng. Bạn phải duy trì tất cả quan hệ thực thể trong ứng dụng. Chúng tôi khuyến nghị sử dụng [transactions](https://docs.aws.amazon.com/amazondynamodb/latest/developerguide/transactions.html) **và  [conditional writes](https://docs.aws.amazon.com/amazondynamodb/latest/developerguide/WorkingWithItems.html#WorkingWithItems.ConditionalUpdate)** khi thao tác với nhiều item liên quan để duy trì tính nhất quán và toàn vẹn dữ liệu. Ví dụ: dùng lệnh TransactWriteItems để đảm bảo rằng khi bạn tạo một thực thể favourite mới trong bảng Orders, thì cả thao tác cập nhật item của đơn hàng thành favourite và thao tác tạo thực thể favourite đều được hoàn thành như một giao dịch duy nhất.

Đoạn mã Python sau minh họa cách thực hiện một lệnh TransactWriteItems trên bảng Orders:

try:

    \# Execute the transaction

    response \= client.transact\_write\_items(

        TransactItems\=\[

            \# Item 1: Create a new favourite item

            {   

                'Put': {

                    'TableName': 'Orders',

                    'Item': {

                        'CustomerId': { 'S': '7970241400' },

                        'SK': { 'S': 'FAVOURITE\#484295' },

                        'ItemId': { 'S': '484295' },

                        'ItemPrice': { 'S': '2.99' },

                        'ItemName': { 'S': 'Eggs' },

                        'ItemDescription': { 'S': 'Free Range Eggs' },

                        'ItemCategory': { 'S': 'Fresh' }

                    }

                }

            },

            

            \# Item 2: Update an existing order's item type

            {

                'Update': {

                    'TableName': 'Orders',

                    'Key': {

                        'CustomerId': { 'S': '7970241400' },

                        'SK': { 'S': '2025-03-01\#2121195' }

                    },

                    'UpdateExpression': 'Set \#Items\[0\].Favourite \= :Favourite',

                    'ExpressionAttributeNames': {

                        '\#Items': 'Items'

                    },

                    'ExpressionAttributeValues': {

                        ':Favourite': { 'BOOL': True },

                        ':ItemId': { 'S': '484295' }

                    },

                    'ConditionExpression': '\#Items\[0\].Id \= :ItemId'

                }

            }

        \]

    )

    return response

except Exception as e:

    print(f"An error occurred: {e}")

    return \[\]

Trong ví dụ trên, **UpdateExpression** đặt giá trị của thuộc tính Favourite của phần tử đầu tiên trong mảng Items thành True.

### **Những sai lầm thường gặp cần tránh**

Hãy ghi nhớ những mẹo khắc phục sự cố sau đây:

* **Giảm thiểu số lượng index trên bảng** – GSIs cho phép các access pattern thay thế đối với dữ liệu trong DynamoDB. Tuy nhiên, mỗi index mới đều làm tăng chi phí lưu trữ và ghi dữ liệu. Throughput được provision cho từng GSI là độc lập với bảng gốc. Do đó, các thao tác ghi, cập nhật, hoặc xóa trên bảng gốc ảnh hưởng đến item trong GSI sẽ tiêu thụ write capacity của GSI. Để tối ưu chi phí, hãy cân nhắc project KEYS\_ONLY vào GSI. Cách này tạo ra GSI nhỏ nhất có thể, nhưng ứng dụng của bạn có thể phải thực hiện hai lệnh gọi DynamoDB cho access pattern mới. Đây là sự đánh đổi giữa chi phí lưu trữ/ghi và chi phí thực hiện thêm lệnh đọc.

* **Tránh dùng DynamoDB cho analytical access patterns** – DynamoDB lý tưởng cho các use case xử lý giao dịch trực tuyến (OLTP) vì access pattern đã được xác định rõ. Không nên cập nhật data model của DynamoDB khi ứng dụng tiến hóa để bao gồm analytical access pattern. DynamoDB cung cấp khả năng tích hợp zero-ETL với nhiều dịch vụ AWS khác được thiết kế chuyên biệt cho phân tích, bao gồm  [Amazon OpenSearch Service](https://docs.aws.amazon.com/amazondynamodb/latest/developerguide/bp-integration-opensearch.html), [Amazon Redshift](https://docs.aws.amazon.com/amazondynamodb/latest/developerguide/RedshiftforDynamoDB-zero-etl.html), và [Amazon Simple Storage Service (Amazon S3)](https://docs.aws.amazon.com/amazondynamodb/latest/developerguide/S3DataExport.HowItWorks.html)

### **Kết luận**

Trong bài viết này, chúng tôi đã chỉ cho bạn cách cập nhật mô hình dữ liệu của bảng DynamoDB khi yêu cầu của ứng dụng thay đổi. Bạn có thể sử dụng các kỹ thuật đã thảo luận để cập nhật data model của bảng DynamoDB mà không cần downtime ứng dụng.

Hãy thử áp dụng các kỹ thuật này cho use case của bạn, và chia sẻ phản hồi trong phần bình luận.

### **Về các tác giả**

### Imhoertha Ojior

[Imhoertha](https://www.linkedin.com/in/iojior/) là Kiến trúc sư Giải pháp tại AWS ở London. Anh tâm huyết với việc giúp khách hàng thiết kế các ứng dụng phân tán đòi hỏi hiệu suất cơ sở dữ liệu ổn định ở mọi quy mô.