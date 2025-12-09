---
title: "Blog 2"
date: "2025-09-09"
weight: 1
chapter: false
pre: " <b> 3.2. </b> "
---


# Cách Laravel Nightwatch xử lý hàng tỷ sự kiện observability theo thời gian thực với Amazon MSK và ClickHouse Cloud

*bởi Masudur Rahaman Sayem, James Carpenter, Jess Archer, và Johnny Mirza | vào ngày 01 THÁNG 10 NĂM 2025 | trong mục Amazon Managed Streaming for Apache Kafka (Amazon MSK), Analytics, Expert (400), Technical How-to*

**Laravel**, một trong những web framework phổ biến nhất thế giới, đã ra mắt nền tảng observability của riêng mình, **Laravel Nightwatch**, để cung cấp cho các nhà phát triển những hiểu biết sâu sắc theo thời gian thực về hiệu suất ứng dụng. Được xây dựng hoàn toàn trên các dịch vụ được quản lý của AWS và **ClickHouse Cloud**, dịch vụ này đã xử lý hơn **một tỷ events mỗi ngày** trong khi vẫn duy trì độ trễ truy vấn dưới một giây, mang lại cho các nhà phát triển khả năng hiển thị tức thì về tình trạng ứng dụng của họ.

Bằng cách kết hợp **Amazon Managed Streaming for Apache Kafka (Amazon MSK)** với ClickHouse Cloud và **AWS Lambda**, Laravel Nightwatch cung cấp high-volume, low-latency monitoring ở quy mô lớn, trong khi đó vẫn duy trì sự đơn giản và trải nghiệm của nhà phát triển mà Laravel vốn nổi tiếng.

---

## Thách thức: Cung cấp real-time monitoring cho một cộng đồng nhà phát triển toàn cầu

Framework Laravel cung cấp năng lượng cho hàng triệu ứng dụng trên toàn thế giới, phục vụ hàng tỷ yêu cầu mỗi tháng. Mỗi yêu cầu có thể tạo ra hàng trăm observability events, chẳng hạn như database queries, queued jobs, cache lookups, emails, notifications, và exceptions. Đối với việc ra mắt của Nightwatch, Laravel đã dự đoán sự chấp nhận ngay lập tức từ cộng đồng toàn cầu của mình, với hàng chục nghìn ứng dụng gửi events liên tục từ ngày đầu tiên.

Laravel Nightwatch cần một kiến trúc có thể:
* Tiếp nhận hàng triệu JSON events mỗi giây từ các ứng dụng của khách hàng một cách đáng tin cậy.
* Cung cấp các truy vấn phân tích dưới một giây cho các real-time dashboards.
* Mở rộng theo chiều ngang để xử lý các đột biến lưu lượng không thể đoán trước.
* Cung cấp tất cả những điều này một cách hiệu quả về chi phí và ít phải bảo trì.

Thách thức là xử lý dữ liệu ở quy mô toàn cầu và cung cấp những hiểu biết sâu sắc về tình trạng ứng dụng mà không ảnh hưởng đến trải nghiệm thiết lập đơn giản cho các nhà phát triển.

---

## Giải pháp: Một decoupled streaming và analytics pipeline

Laravel Nightwatch đã triển khai một kiến trúc **dual-database**, **streaming-first**, được hiển thị trong hình trên, giúp tách biệt các transactional workloads và analytical workloads.

* **Transactional workloads** – user account, organization settings, billing, và các khối lượng công việc tương tự chạy trên **Amazon RDS** for PostgreSQL.
* **Analytical workloads** – các telemetry events, metrics, query logs, và request traces được xử lý bởi **ClickHouse Cloud**.

### Các thành phần chính

Các thành phần của giải pháp bao gồm:

#### Ingestion layer
* **Amazon API Gateway** nhận telemetry từ các agents của Laravel được nhúng trong ứng dụng của khách hàng.
* **Lambda** xác thực và bổ sung thông tin cho events. Các events đã được xác thực và bổ sung thông tin được publish đến Amazon MSK, được phân vùng để có khả năng mở rộng.

#### Streaming to analytics
* **ClickPipes** trong ClickHouse Cloud đăng ký trực tiếp vào các topics của MSK, giảm nhu cầu xây dựng và quản lý các extract, transform, and load (ETL) pipelines.
* **Materialized views** trong ClickHouse tiền tổng hợp và chuyển đổi raw JSON thành dạng query-ready.

#### Dashboards và delivery
* Nightwatch dashboard, được xây dựng với Laravel, Inertia, và React, chạy trên **AWS Fargate for Amazon ECS**.
* **Amazon ElastiCache for Redis** tăng tốc sessions và cache lookups.
* **Cloudflare CDN** cung cấp low-latency delivery đến người dùng toàn cầu.

![Kiến trúc giải pháp](/images/blog2/architecture-diagram.png)

---

## Tại sao lại là Amazon MSK và ClickHouse Cloud?

Nightwatch đòi hỏi một streaming backbone bền bỉ, có khả năng mở rộng theo chiều ngang và ít phải bảo trì.

Với **Amazon MSK Express brokers**, chúng tôi đã đạt được hơn **một tỷ events mỗi giây** trong quá trình load testing, hưởng lợi từ low-latency, elastic scaling, và simplified operations. MSK Express brokers không yêu cầu storage sizing hoặc provisioning, mở rộng nhanh hơn gấp 20 lần, và phục hồi nhanh hơn 90% so với các Apache Kafka brokers tiêu chuẩn - tất cả trong khi vẫn thực thi best-practice defaults và client quotas để có hiệu suất đáng tin cậy. Sự tích hợp liền mạch của nó với các dịch vụ AWS khác-như Lambda, **Amazon Simple Storage Service (Amazon S3)**, và **Amazon CloudWatch**-đã giúp việc xây dựng một resilient, end-to-end streaming architecture trở nên đơn giản.

Để tiếp nhận và chuyển đổi các events này trong thời gian thực, Nightwatch sử dụng ClickHouse Cloud và nền tảng tích hợp được quản lý của nó, **ClickPipes**. ClickHouse Cloud vượt trội trong các analytical workloads bằng cách cung cấp hiệu suất truy vấn cho phân tích nhanh hơn tới **100 lần** so với các row-based databases truyền thống. Các thuật toán nén tiên tiến của nó giúp tiết kiệm tới **90% dung lượng lưu trữ**, giảm đáng kể chi phí cơ sở hạ tầng trong khi vẫn duy trì hiệu suất cao. Với columnar architecture và execution engine được tối ưu hóa, ClickHouse Cloud có thể truy vấn hàng tỷ hàng trong vòng chưa đầy 1 giây, cho phép Laravel Nightwatch phục vụ các real-time dashboards và analytics ở quy mô toàn cầu.

Bằng cách tích hợp Amazon MSK và ClickHouse bằng ClickPipes, Laravel cũng đã giảm bớt gánh nặng vận hành của việc xây dựng và quản lý các ETL pipelines, giảm độ trễ và sự phức tạp.

---

## Vượt qua thách thức

### Sự phức tạp trong kiểm thử
Trong khi việc đo lường hiệu năng tổng hợp và các bộ dữ liệu thử nghiệm mang lại kết quả hữu ích, cần có một khối lượng công việc thực tế hơn để kiểm tra nghiêm ngặt cơ sở hạ tầng và mã nguồn trước khi triển khai lên sản phẩm. Nhóm đã sử dụng **Terraform** để quản lý cơ sở hạ tầng cùng với application code, tạo ra nhiều môi trường dev và test, và cho phép họ kiểm tra nền tảng nội bộ với các ứng dụng của chính mình trước mỗi lần phát hành.

### Cơ sở hạ tầng đa khu vực
Nhu cầu phục vụ nhiều khu vực lưu trữ dữ liệu cũng mang lại những thách thức—với độ trễ, sự phức tạp và chi phí là những mối quan tâm hàng đầu. Tuy nhiên, AWS, ClickHouse Cloud, và Cloudflare stack đã cung cấp một bộ công cụ mạng và các tùy chọn mở rộng mạnh mẽ. Trong khi VPC peering, RDS replication, và global server load balancing đảm nhận phần việc nặng về mạng, khả năng mở rộng và điều chỉnh kích thước phù hợp cho từng tài nguyên đã giúp giữ chi phí ở mức tối thiểu.

### Hiệu suất truy vấn ở quy mô lớn
Materialized views, phân vùng chuỗi thời gian thông minh, và các codecs chuyên dụng của ClickHouse đã giúp đảm bảo rằng các truy vấn vẫn dưới một giây ngay cả khi khối lượng dữ liệu tăng lên hàng tỷ. Trong khi đó, việc tách biệt tính toán cho phép các khối lượng công việc riêng biệt mở rộng độc lập trong khi truy cập cùng một dữ liệu, với các cụm được điều chỉnh kích thước phù hợp theo chiều ngang và chiều dọc tùy thuộc vào yêu cầu của mỗi tải.

---

## Kết quả

Việc ra mắt Laravel Nightwatch đã vượt qua mong đợi:
* **5,300** người dùng đăng ký trong 24 giờ đầu tiên
* **500 triệu** events được xử lý trong ngày đầu tiên
* **97 ms** độ trễ trung bình của dashboard request
* **760,000** exceptions được ghi lại và phân tích trong thời gian thực

Bằng cách xây dựng trên Amazon MSK và ClickHouse Cloud, chúng tôi đã có khả năng mở rộng từ con số không lên đến hàng tỷ events mà không phải hy sinh hiệu suất hay trải nghiệm nhà phát triển.

---

## Tiếp theo là gì

Laravel có kế hoạch mở rộng Nightwatch với:
* **Nhiều khu vực hơn** để phục vụ khách hàng có yêu cầu về data sovereignty ngoài US và EU.
* **Thu thập dữ liệu rộng hơn** để cung cấp cái nhìn sâu sắc hơn nữa vào customer’s applications.
* **Chứng nhận SOC 2** để phục vụ khách hàng có yêu cầu tuân thủ chặt chẽ hơn.
* **Giám sát và phân tích nâng cao hơn** để xác định các vấn đề trước khi chúng ảnh hưởng người dùng.

Kiến trúc hiện tại hỗ trợ các ứng dụng ở mọi quy mô, từ sở thích cá nhân đến doanh nghiệp (bao gồm cả free tier), và được thiết kế để xử lý hơn một nghìn tỷ events hàng tháng mà không làm giảm hiệu suất.

---

## Kết luận

Laravel Nightwatch chứng minh cách Amazon MSK, ClickHouse Cloud, và các công nghệ AWS serverless có thể kết hợp để tạo một nền tảng giám sát thời gian thực, hiệu quả về chi phí ở quy mô toàn cầu. Bằng cách thiết kế cho quy mô lớn từ ngày đầu, Laravel đã cung cấp các phân tích dưới một giây trên hàng tỷ events, trong khi vẫn duy trì trải nghiệm thân thiện với nhà phát triển mà cộng đồng của họ mong đợi.

---

### Về các tác giả

![Jess Archer](/images/blog2/jess-archer.png)
**Jess Archer** là Engineering Manager và Head of Nightwatch tại Laravel, tập trung vào application observability, performance monitoring, và trải nghiệm nhà phát triển. Cô lãnh đạo nhóm Nightwatch trong khi vẫn trực tiếp tham gia vào codebase. Trước Laravel, Jess đã làm việc trên các nền tảng thu thập dữ liệu lâm sàng, phần mềm cho cơ quan thực thi pháp luật, và các giải pháp chống lừa đảo trong ngành ngân hàng. Sau đó, cô đã đóng góp rộng rãi cho hệ sinh thái open-source của Laravel trước khi chuyển sang vai trò lãnh đạo hiện tại. Jess rất đam mê open source và tạo ra các công cụ giúp các nhà phát triển làm việc hiệu quả hơn.

![James Carpenter](/images/blog2/james-carpenter.png)
**James Carpenter** là Senior Infrastructure Engineer gia nhập Laravel vào năm 2024 với tư cách là Infrastructure Lead cho nhóm Nightwatch, mang theo kinh nghiệm từ 15 năm trong lĩnh vực thể thao và chăm sóc sức khỏe. Chuyên về DevOps và Infrastructure, anh đam mê giải quyết các vấn đề phức tạp và tạo ra những trải nghiệm đặc biệt cho cả khách hàng và nhà phát triển.

![Johnny Mirza](/images/blog2/johnny-mirza.png)
**Johnny Mirza** là Solution Architect tại ClickHouse, làm việc với người dùng trên khắp khu vực APAC. Với hơn 20 năm kinh nghiệm trong lĩnh vực solutions engineering, anh có kinh nghiệm trong việc kiến trúc và triển khai các giải pháp cho các khách hàng doanh nghiệp trong các lĩnh vực viễn thông, truyền thông, bảo hiểm và dịch vụ tài chính. Johnny có chuyên môn cao về tích hợp giữa cơ sở hạ tầng public cloud và on-premise, đồng thời tập trung vào đảm bảo dịch vụ, nền tảng giám sát và các công nghệ open-source. Trước ClickHouse, Johnny đã là thành viên của các nhóm kỹ thuật giải pháp tại Confluent, Splunk, và Optus.

![Masudur Rahaman Sayem](/images/blog2/masudur-rahaman-sayem.png)
**Masudur Rahaman Sayem** là Streaming Data Architect tại AWS với hơn 25 năm kinh nghiệm trong ngành IT. Anh hợp tác với các khách hàng của AWS trên toàn thế giới để kiến trúc và triển khai các giải pháp data streaming phức tạp nhằm giải quyết các thách thức kinh doanh phức tạp. Là một chuyên gia về distributed computing, Sayem chuyên thiết kế kiến trúc hệ thống phân tán quy mô lớn để đạt hiệu suất và khả năng mở rộng tối đa. Anh có sự quan tâm và đam mê sâu sắc đối với kiến trúc phân tán, điều mà anh áp dụng để thiết kế các giải pháp cấp doanh nghiệp ở quy mô internet.