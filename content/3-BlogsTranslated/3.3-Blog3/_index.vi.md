---
title: "Blog 3"
date: "2025-09-09"
weight: 1
chapter: false
pre: " <b> 3.3. </b> "
---

# Cách xuất dữ liệu ra Amazon S3 Tables bằng AWS Step Function Distributed Map

*bởi Chetan Makvana và Aidan Eglin | vào ngày 01 THÁNG 10 NĂM 2025 | trong mục Advanced (300), Amazon S3 Tables, AWS Step Functions, Serverless, Technical How-to*

Các công ty chạy các **serverless workloads** thường cần thực hiện các hoạt động **extract, transform, and load (ETL)** trên các tệp dữ liệu được lưu trữ trong các bucket của **Amazon Simple Storage Service (Amazon S3)**. Mặc dù các phương pháp truyền thống như **AWS Lambda trigger for Amazon S3** hoặc **Amazon S3 Event Notifications** có thể xử lý các hoạt động này, chúng có thể không đáp ứng đủ khi các luồng công việc đòi hỏi khả năng hiển thị, kiểm soát nâng cao hoặc sự can thiệp của con người. Ví dụ, một số quy trình có thể cần xem xét thủ công các bản ghi bị lỗi hoặc phê duyệt rõ ràng trước khi chuyển sang các giai đoạn tiếp theo. Các giải pháp tự xây dựng của khách hàng cho những vấn đề này có thể phức tạp và dễ bị lỗi.

**AWS Step Functions** giải quyết những thách thức này bằng cách cung cấp các khả năng quản lý và giám sát luồng công việc được tích hợp sẵn. Tính năng **Step Functions Distributed Map** được thiết kế cho các luồng công việc xử lý dữ liệu song song, **high-throughput** để các công ty có thể xử lý các công việc ETL phức tạp, **fan-out processing**, và trực quan hóa dữ liệu ở quy mô lớn. Distributed Map xử lý mỗi mục trong bộ dữ liệu như một **child workflow** độc lập, xử lý hàng triệu bản ghi trong khi vẫn duy trì các cơ chế kiểm soát đồng thời, khả năng chịu lỗi, và theo dõi tiến trình được tích hợp sẵn. Dữ liệu đã xử lý có thể được xuất liền mạch đến nhiều đích khác nhau, bao gồm cả **Amazon S3 Tables** với sự hỗ trợ của **Apache Iceberg**.

Trong bài viết này, chúng tôi chỉ ra cách dùng Step Functions Distributed Map để xử lý các Amazon S3 objects và xuất kết quả ra Amazon S3 Tables, tạo một **data processing pipeline** có khả năng mở rộng và dễ bảo trì.

Xem **GitHub repository** đi kèm để biết hướng dẫn chi tiết về việc triển khai giải pháp này cũng như mã nguồn mẫu.

---

## Tổng quan giải pháp

Hãy xem xét một công ty điện tử tiêu dùng thường xuyên tham gia các triển lãm thương mại và hội nghị ngành. Trong các sự kiện này, những người tham dự quan tâm sẽ điền vào các biểu mẫu đăng ký bằng giấy để yêu cầu demo sản phẩm, nhận bản tin, hoặc tham gia các chương trình truy cập sớm. Sau sự kiện, đội ngũ của công ty quét hàng trăm nghìn biểu mẫu này và tải chúng lên Amazon S3. Thay vì xem xét thủ công từng biểu mẫu, công ty muốn tự động hóa việc trích xuất các chi tiết chính của khách hàng như tên, địa chỉ email, địa chỉ gửi thư, và lĩnh vực quan tâm. Họ muốn lưu trữ dữ liệu có cấu trúc này trong S3 Tables với định dạng Apache Iceberg để phục vụ cho việc phân tích và nhắm mục tiêu các chiến dịch marketing ở các công đoạn sau.

Hãy xem cách giải pháp trong bài viết này sử dụng Distributed Map để xử lý các tệp PDF song song, trích xuất dữ liệu bằng **Amazon Textract**, và ghi trực tiếp đầu ra đã được làm sạch vào S3 Tables. Kết quả là một quy trình tiếp nhận dữ liệu sau sự kiện có khả năng mở rộng và hoàn toàn serverless, như được hiển thị trong hình sau.

![Tổng quan giải pháp](/images/blog3/solution-overview.png)

Luồng công việc xử lý dữ liệu như được hiển thị trong sơ đồ trên bao gồm các bước sau:

1.  Người dùng tải các biểu mẫu quan tâm của khách hàng dưới dạng tệp PDF đã quét vào một bucket Amazon S3.
2.  Một quy tắc **Amazon EventBridge Scheduler** được kích hoạt theo các khoảng thời gian đều đặn, khởi tạo một Step Functions workflow.
3.  Lần thực thi workflow này sẽ kích hoạt một trạng thái Step Functions Distributed Map, liệt kê tất cả các tệp PDF đã được tải lên Amazon S3 kể từ lần chạy trước.
4.  Distributed Map duyệt qua danh sách các đối tượng và chuyển mỗi **object’s metadata** (bucket, key, size, entity tag [ETag]) cho một lần thực thi **child workflow**.
5.  Đối với mỗi object, child workflow gọi Amazon Textract với bucket và key được cung cấp để trích xuất **raw text** và các trường liên quan (tên, địa chỉ email, địa chỉ gửi thư, lĩnh vực quan tâm) từ tệp PDF.
6.  Child workflow gửi dữ liệu đã trích xuất đến **Amazon Data Firehose**, được cấu hình để chuyển tiếp dữ liệu đến S3 Tables.
7.  Firehose gom nhóm dữ liệu đến từ child workflow và ghi nó vào S3 Tables theo một khoảng thời gian được định cấu hình sẵn mà bạn chọn.

Với dữ liệu hiện đã có cấu trúc và có thể truy cập trong S3 Tables, người dùng có thể dễ dàng phân tích chúng bằng các truy vấn SQL tiêu chuẩn với **Amazon Athena** hoặc các công cụ **business intelligence** như **Amazon QuickSight**.

---

## Luồng công việc xử lý dữ liệu

EventBridge Scheduler khởi động các Step Functions workflows mới theo các khoảng thời gian đều đặn. Mốc thời gian cho lịch trình này là linh hoạt. Tuy nhiên, khi thiết lập lịch trình của bạn, hãy đảm bảo tần suất phù hợp với khoảng thời gian mà **state machine** của bạn được cấu hình để tìm kiếm các tệp PDF. Ví dụ, nếu state machine của bạn kiểm tra các tệp PDF từ tuần trước, bạn sẽ muốn lên lịch cho nó chạy hàng tuần. Step Functions workflow sau đó thực hiện ba bước sau (lưu ý rằng đây là các bước 4, 5, 6, và 7 trong sơ đồ luồng công việc ở trên):

1.  Trích xuất dữ liệu người dùng liên quan từ các tệp PDF.
2.  Gửi dữ liệu người dùng đã trích xuất đến Firehose.
3.  Ghi dữ liệu vào S3 Tables ở định dạng bảng Apache Iceberg.

Sơ đồ sau minh họa luồng công việc này.

![Sơ đồ luồng công việc](/images/blog3/workflow-diagram.png)

Hãy xem xét chi tiết hơn từng bước của luồng công việc trên.

### Trích xuất dữ liệu người dùng liên quan từ tài liệu PDF

Step Functions sử dụng Distributed Map để xử lý các tệp PDF đồng thời trong các **child workflows** song song. Nó chấp nhận đầu vào từ các tệp JSON, JSONL, CSV, Parquet, các tệp **Amazon S3 manifest** được lưu trữ trong Amazon S3 (dùng để chỉ định các tệp cụ thể cần xử lý), hoặc một **Amazon S3 bucket prefix** (cho phép duyệt qua metadata của tất cả các đối tượng dưới tiền tố đó). Step Functions tự động xử lý việc song song hóa bằng cách chia nhỏ bộ dữ liệu và chạy các child workflows cho mỗi mục, với trường **ItemBatcher** cho phép nhóm nhiều tệp PDF vào một lần thực thi child workflow duy nhất (ví dụ: 10 tệp PDF mỗi batch) để tối ưu hóa hiệu suất và chi phí.

Ảnh chụp màn hình sau từ Step Functions console cho thấy cấu hình cho Distributed Map. Ví dụ, chúng tôi đã cấu hình Distributed Map để xử lý 10 tệp PDF quan tâm của khách hàng trong một child workflow duy nhất.

![Step Functions Console](/images/blog3/step-functions-console.png)

Hình ảnh sau đây cho thấy một ví dụ về các tệp PDF được quét này, bao gồm thông tin khách hàng mà giải pháp của bài viết này xử lý.

![Ví dụ PDF được quét](/images/blog3/scanned-pdf-example.png)

Mỗi child workflow sau đó gọi **Amazon Textract AnalyzeDocument API** với các truy vấn cụ thể để trích xuất thông tin khách hàng.

```json
{
  "Document": {
    "S3Object": {
      "Bucket": "<input PDFs bucket>",
      "Name": "{% $states.input.Key %}"
    }
  },
  "FeatureTypes": [
    "QUERIES"
  ],
  "QueriesConfig": {
    "Queries": [
      {
        "Alias": "full_name",
        "Text": "What is the customer's name?"
      },
      {
        "Alias": "phone_number",
        "Text": "What is the customer’s phone number?"
      },
      {
        "Alias": "mailing_address",
        "Text": "What is the customer’s mailing address?"
      },
      {
        "Alias": "interest",
        "Text": "What is the customer’s interest?"
      }
    ]
  }
}
```
API phân tích mỗi tệp PDF được quét và trả về một cấu trúc JSON chứa thông tin khách hàng đã được trích xuất.

### Gửi dữ liệu người dùng đã trích xuất đến Firehose
Child workflow sau đó sử dụng một Firehose PutRecordBatch API action với service integrations để đưa thông tin khách hàng đã trích xuất vào hàng đợi để xử lý thêm. Yêu cầu hành động PutRecordBatch bao gồm tên luồng Firehose và các bản ghi dữ liệu. Các bản ghi dữ liệu bao gồm một blob dữ liệu từ bước 1 chứa thông tin khách hàng đã trích xuất, như trong ví dụ sau.

```json
{
  "DeliveryStreamName": "put_raw_form_data_100",
  "Records": [
    {
      "Data": "{\"full_name\":\"Anthony Ayala\",\"phone_number\":\"001-384-925-0701\",\"mailing_address\":\"38548 Joshua Wall Suite 974, East Heatherfort, OH 32669\",\"interest\":\"Fitness Trackers\",\"processed_date\":\"2025-05-01\"}"
    },
    {
      "Data": "{\"full_name\":\"Becky Williams\",\"phone_number\":\"+1-283-499-2466\",\"mailing_address\":\"227 King Forge Suite 241, East Nathanland, PR 05687\",\"interest\":\"Al Assistants\",\"processed_date\":\"2025-05-01\"}"
    }
  ]
}
```
### Ghi dữ liệu vào S3 Tables ở định dạng bảng Apache Iceberg
Firehose quản lý hiệu quả việc data buffering, chuyển đổi định dạng, và phân phối đáng tin cậy đến nhiều đích khác nhau, bao gồm Apache Iceberg, raw files trong Amazon S3, Amazon OpenSearch Service, hoặc any of the other supported destinations. Các bảng Apache Iceberg có thể được tự quản lý trong Amazon S3 hoặc được lưu trữ trong S3 Tables. Mặc dù các bảng Iceberg tự quản lý đòi hỏi tối ưu hóa thủ công—chẳng hạn như compaction và snapshot expiration—S3 Tables tự động tối ưu hóa lưu trữ cho các khối lượng công việc phân tích quy mô lớn, cải thiện hiệu suất truy vấn và giảm chi phí lưu trữ.

Firehose đơn giản hóa quá trình streaming dữ liệu bằng cách cấu hình một luồng phân phối, chọn một nguồn dữ liệu, và đặt một bảng Iceberg làm đích. Sau khi bạn đã thiết lập xong, luồng Firehose đã sẵn sàng để phân phối dữ liệu. Dữ liệu được phân phối có thể được truy vấn từ S3 Tables bằng cách sử dụng Athena, như được hiển thị trong ảnh chụp màn hình sau của giao diện điều khiển Athena.

![Query](/images/blog3/query.png)

Kết quả truy vấn bao gồm tất cả dữ liệu khách hàng đã được xử lý từ các tệp PDF, như được hiển thị trong ảnh chụp màn hình sau.

![Result](/images/blog3/result.png)

Sự tích hợp này thể hiện một giải pháp mạnh mẽ, không cần mã nguồn để chuyển đổi các biểu mẫu raw PDF thành dữ liệu được làm giàu, có thể truy vấn trong một bảng Iceberg. Bạn có thể sử dụng những dữ liệu này để phân tích sâu hơn.

### Kết luận
Trong bài viết này, chúng tôi đã chỉ ra cách xây dựng một giải pháp serverless, có khả năng mở rộng để xử lý tài liệu PDF và xuất dữ liệu đã trích xuất ra S3 Tables bằng cách sử dụng Step Functions Distributed Map. Kiến trúc này mang lại một số lợi ích chính như độ tin cậy, hiệu quả về chi phí, khả năng hiển thị, và khả năng bảo trì. Bằng cách tận dụng các dịch vụ của AWS như Step Functions, Amazon Textract, Firehose, và S3 Tables, các công ty có thể tự động hóa các luồng công việc xử lý tài liệu của mình đồng thời đảm bảo hiệu suất tối ưu và hoạt động xuất sắc. Giải pháp này có thể được điều chỉnh cho nhiều trường hợp sử dụng khác ngoài các biểu mẫu quan tâm của khách hàng, chẳng hạn như xử lý hóa đơn, biểu mẫu ứng tuyển, hoặc bất kỳ kịch bản nào đòi hỏi trích xuất dữ liệu có cấu trúc từ tài liệu ở quy mô lớn.

Mặc dù ví dụ này tập trung vào việc xử lý dữ liệu PDF và ghi vào S3 Tables, Distributed Map có thể xử lý nhiều nguồn đầu vào khác nhau bao gồm các tệp JSON, JSONL, CSV, và Parquet trong Amazon S3; các mục trong bảng Amazon DynamoDB; kết quả truy vấn Athena; và tất cả các API List của AWS có phân trang. Tương tự, thông qua Step Functions service integrations, bạn có thể ghi kết quả ra nhiều đích đến như các bảng DynamoDB tables bằng cách sử dụng tích hợp dịch vụ PutItem.

Để bắt đầu với giải pháp này, hãy xem GitHub repository đi kèm để biết hướng dẫn triển khai và mã nguồn mẫu.