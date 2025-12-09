---
title: "Blog 1"
date: "2025-09-09"
weight: 1
chapter: false
pre: " <b> 3.1. </b> "
---

# Rox tăng tốc năng suất bán hàng với AI agents được hỗ trợ bởi Amazon Bedrock

*viết bởi Santhan Pamulapati, Andrew Brown, Santhosh Kumar Manavasi Lakshminarayana, Shriram Sridharan, và Taeuk Kang | vào ngày 1 tháng 10 năm 2025 | trong mục Amazon Bedrock, Amazon Machine Learning, Customer Solutions*

Bài viết này được đồng tác giả bởi Shriram Sridharan, Taeuk Kang, và Santhosh Kumar Manavasi Lakshminarayanan từ Rox.

**Rox** đang xây dựng một hệ thống điều hành doanh thu cho kỷ nguyên ứng dụng AI.

Các đội ngũ kinh doanh hiện đại phụ thuộc vào nhiều dữ liệu hơn bao giờ hết, chẳng hạn như hệ thống quản lý quan hệ khách hàng (CRM), tự động hóa marketing, hệ thống tài chính, phiếu hỗ trợ, và dữ liệu sử dụng sản phẩm trực tiếp. Mặc dù mỗi hệ thống đều có vai trò riêng, nhưng khi kết hợp lại, chúng tạo silos làm chậm người bán và bỏ lỡ những điều sâu sắc chưa được khai thác.

Rox giải quyết vấn đề này bằng cách cung cấp một **hệ thống điều hành doanh thu**: một lớp thống nhất giúp kết nối các tín hiệu này lại với nhau và trang bị AI agents để thực thi các luồng công việc go-to-market (GTM). Thay vì đối chiếu báo cáo hoặc cập nhật các trường dữ liệu, nhân viên bán hàng nhận thông tin theo thời gian thực và tự động hóa trong luồng công việc hằng ngày của họ.

Hôm nay, chúng tôi vui mừng thông báo rằng Rox đã phát hành rộng rãi, với cơ sở hạ tầng Rox được xây dựng trên AWS và được cung cấp trên web, Slack, macOS, và iOS. Trong bài viết này, chúng tôi chia sẻ cách Rox tăng tốc năng suất bán hàng với AI agents được hỗ trợ bởi [Amazon Bedrock](https://aws.amazon.com/bedrock/).

---

## Tổng quan giải pháp

Như đã ghi trong *Rox is transforming revenue teams with AI-driven integration powered by AWS*, các đội ngũ GTM hiện đại cần nhiều hơn một cơ sở dữ liệu tĩnh. Dữ liệu doanh thu trải dài trên nhiều hệ thống, như là dữ liệu sử dụng sản phẩm, tài chính và hỗ trợ, và các đội ngũ cần một hệ thống có thể thống nhất bối cảnh và hành động dựa trên nó theo thời gian thực.

Rox mang lại điều này thông qua một kiến trúc phân lớp trên AWS:

* **System of record** - Một đồ thị tri thức thống nhất, có quản trị sẽ kết hợp từ CRM, tài chính, hỗ trợ, đo lường sản phẩm từ xa, và dữ liệu web.
* **Agent swarms** - Các agents thông minh, nhận biết bối cảnh khách hàng, có khả năng suy luận trên đồ thị và điều phối các luồng công việc đa bước như nghiên cứu, tiếp cận, quản lý cơ hội, và tạo đề xuất.
* **Interfaces across surfaces** - Người bán hàng tương tác trong luồng công việc này ở nơi họ làm việc, như là web application, Slack, iOS, và macOS

Điều này chuyển đổi CRM từ một hệ thống ghi nhận thụ động thành một hệ thống hành động chủ động, để các đội ngũ có thể hành động dựa trên dữ liệu của họ ngay lập tức và một cách thông minh.

Sơ đồ dưới đây minh họa kiến trúc giải pháp.

![Solution Architecture](/images/blog1/architecture-diagram.png)

---

## Lợi ích và đặc điểm của ROX

Hiện đã phát hành rộng rãi, Rox mở rộng từ việc intelligence đến thực thi toàn diện với **Command**, một conversational interface mới có thể điều phối các luồng công việc multi-agent. Command kết hợp với nhiều agents đặc biệt chạy song song.

Một yêu cầu (ví dụ, “chuẩn bị cho tôi về gia hạn ACME và soạn thảo follow-ups”) được mở rộng thành một kế hoạch:
1.  Nghiên cứu tín hiệu sử dụng và hỗ trợ
2.  Xác định các stakeholders còn thiếu
3.  Soạn thảo nội dung tiếp cận
4.  Cập nhật cơ hội
5.  Tập hợp thành một bản đề xuất

Một bước được hoàn thành thông qua tool calls trong hệ thống của bạn và phải tuân theo các phê duyệt từ guardrail. **Comprehensive safety architecture** của chúng tôi sử dụng một hệ thống guardrail đa lớp tinh vi làm tuyến phòng thủ đầu tiên chống lại yêu cầu không phù hợp, có hại hoặc độc hại. Các yêu cầu đầu vào trải qua quá trình phân tích nghiêm ngặt thông qua cơ chế lọc tiên tiến của chúng tôi trước khi đến inference layer. Giai đoạn tiền xử lý đánh giá nhiều khía cạnh về an toàn và sự thích hợp, như là legal compliance assessment và business relevance evaluation, để chắc chắn chỉ những yêu cầu hợp pháp, an toàn, và phù hợp bối cảnh mới được tiến hành đến model execution.

Command phân rã yêu cầu, định tuyến các bước đến đúng các agents, sắp xếp external tool invocations (CRM, calendar, enrichment, email), tổng hợp kết quả vào hệ thống bối cảnh, và trả về luồng mạch lạc sẵn sàng để sử dụng trên web, Slack, iOS, hoặc macOS. Mỗi đề nghị đều có thể giải thích (sources và traces), có thể đảo ngược (audit logs), và nhận biết chính sách (role-based access control, rate limits, required approvals).

---

## Amazon Bedrock cung cấp sức mạnh cho Rox như thế nào

Command đòi hỏi một mô hình có khả năng suy luận qua nhiều bước, điều phối các công cụ, và thích ứng linh hoạt.

Để đáp ứng các yêu cầu, Rox chọn **Anthropic’s Claude Sonnet 4 trên Amazon Bedrock**. Anthropic’s Claude Sonnet 4 đã liên tục chứng tỏ hiệu suất vượt trội về tool-calling và suy luận, cho phép các agent của Rox sắp xếp các luồng công việc như nghiên cứu tài khoản, làm giàu dữ liệu, tiếp cận, quản lý cơ hội, và tạo đề xuất một cách đáng tin cậy.

Amazon Bedrock cung cấp nền tảng để triển khai Rox ở quy mô doanh nghiệp, mang lại bảo mật, linh hoạt để tích hợp với các models mới nhất, và khả năng mở rộng để xử lý hàng ngàn agents đồng thời một cách đáng tin cậy.

### Các tính năng bổ sung

Ngoài Command, Rox bao gồm những tính năng sau:

| Tính năng | Mô tả |
| :--- | :--- |
| **Research** | Cung cấp nghiên cứu sâu về tài khoản và thị trường, dựa trên unified context (kế thừa từ private beta) |
| **Meet** | Cho phép ghi âm, chuyển mã, tóm tắt và biến các cuộc họp thành hành động (kế thừa từ private beta) |
| **Outreach** | Cung cấp tương tác với khách hàng tiềm năng được cá nhân hóa, đặt trong bối cảnh từ unified data (mới) |
| **Revenue** | Giúp bạn theo dõi, cập nhật, và thúc đẩy pipelines trong luồng công việc (mới) |
| **Auto-fill proposals** | Giúp bạn tập hợp các đề xuất được tùy chỉnh trong vài giây từ account context (mới) |
| **Rox apps** | Cung cấp modular extensions để giúp bổ sung các luồng công việc được xây dựng có mục đích (dashboards, trackers) trực tiếp vào hệ thống (mới) |
| **iOS app** | Cung cấp thông báo và chuẩn bị cuộc họp khi đang di chuyển (mới) |
| **Mac app** | Mang lại khả năng chuyển mã các cuộc gọi và thêm chúng vào hệ thống bối cảnh (mới) |
| **Regional expansion** | Hiện đã có mặt tại AWS Middle East (Bahrain) AWS Region, phù hợp với nhu cầu về data residency và sovereignty (mới) |

---

## Tác động ban đầu đến khách hàng

Trong giai đoạn beta, các doanh nghiệp đã thấy được lợi ích ngay lập tức:
* Năng suất của nhân viên đại diện cao hơn **50%**
* Tốc độ bán hàng nhanh hơn **20%**
* Doanh thu mỗi nhân viên đại diện tăng **gấp hai lần**

Ví dụ, các khách hàng thực tế của Rox đã có thể tập trung sắc bén hơn vào các cơ hội có giá trị cao, thúc đẩy mức tăng **40 - 50%** trong giá bán trung bình. Các khách hàng khác đã giảm **90%** thời gian chuẩn bị của nhân viên và chốt giao dịch nhanh hơn, cộng thêm **15%** giao dịch trị giá sáu con số được khám phá thông qua những hiểu biết của Rox. Rox cũng rút ngắn ramp time cho nhân viên mới, với khách hàng báo cáo ramp time nhanh hơn **50%** khi sử dụng Rox.

---

## Thử Rox ngay hôm nay

Tầm nhìn của chúng tôi là các đội ngũ kinh doanh sẽ vận hành với một agent swarm luôn hoạt động, liên tục nghiên cứu tài khoản, tương tác với stakeholders, và thúc đẩy pipeline tiến lên.

Rox hiện đã phát hành rộng rãi. Hãy bắt đầu tại [rox.com](https://rox.com) hoặc truy cập [AWS Marketplace](https://aws.amazon.com/marketplace). Cùng với AWS, chúng tôi sẽ tiếp tục xây dựng hệ thống điều hành dựa trên AI cho các đội ngũ kinh doanh hiện đại.

---

### Về các tác giả

![Shriram Sridharan](/images/blog1/shriram-sridharan.png)
**Shriram Sridharan** là Co-Founder/Engineering Head của Rox, một công ty AI được Sequoia hậu thuẫn. Trước Rox, Shriram đã lãnh đạo data infrastructure team tại Confluent, chịu trách nhiệm làm cho Kafka nhanh hơn và rẻ hơn trên các đám mây. Trước đó, anh là một trong những kỹ sư đời đầu tại Amazon Aurora (pre-launch) tái định hình cơ sở dữ liệu cho đám mây. Aurora là Dịch vụ AWS phát triển nhanh nhất và đã nhận được giải thưởng hệ thống SIGMOD năm 2019.

![Taeuk Kang](/images/blog1/taeuk-kang.png)
**Taeuk Kang** là Founding Engineer tại Rox, làm việc trong lĩnh vực nghiên cứu và kỹ thuật AI. Anh học Khoa học Máy tính tại Stanford. Trước Rox, anh đã xây dựng các large language model agents và retrieval-augmented generation systems tại X (trước đây là Twitter) và thiết kế distributed LLM infrastructure cung cấp năng lượng cho các tính năng cốt lõi của sản phẩm và Trust & Safety, cải thiện sức khỏe tổng thể của nền tảng. Trước đó tại Stripe, anh đã phát triển các streaming and batch data processing pipelines hiệu suất cao tích hợp Apache Flink, Spark, Kafka, và AWS SQS.

![Santhosh Kumar Manavasi Lakshminarayanan](/images/blog1/santhosh-kumar.png)
**Santhosh Kumar Manavasi Lakshminarayanan** lãnh đạo Platform tại Rox. Trước Rox, anh là Director of Engineering tại StreamSets, được IBM mua lại, lãnh đạo StreamSets Cloud Platform giúp các doanh nghiệp lớn vận hành data pipeline của họ ở quy mô lớn trên các nhà cung cấp đám mây hiện đại. Trước StreamSets, anh là senior engineer tại Platform Metadata team tại Informatica.

![Andrew Brown](/images/blog1/andrew-brown.png)
**Andrew Brown** là Account Executive cho AI Startups tại Amazon Web Services (AWS) ở San Francisco, CA. Với nền tảng vững chắc về cloud computing và tập trung vào việc hỗ trợ các startup, Andrew chuyên giúp các công ty mở rộng quy mô hoạt động của họ bằng cách sử dụng các công nghệ của AWS.

![Santhan Pamulapati](/images/blog1/santhan-pamulapati.png)
**Santhan Pamulapati** là Sr. Solutions Architect cho GenAI startups tại AWS, với chuyên môn sâu về thiết kế và xây dựng các giải pháp có khả năng mở rộng nhằm thúc đẩy tăng trưởng của khách hàng. Anh có nền tảng vững chắc trong việc xây dựng các hệ thống HPC tận dụng các dịch vụ của AWS và đã làm việc với các khách hàng chiến lược để giải quyết các thách thức kinh doanh.