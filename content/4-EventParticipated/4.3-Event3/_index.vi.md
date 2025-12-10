---
title: "Event 3"
date: "2025-09-09"
weight: 3
chapter: false
pre: " <b> 4.3. </b> "
---


# Báo cáo tổng hợp: "AWS Cloud Mastery Series #2: DevOps on AWS"

### Mục đích của sự kiện

- Hiểu thế nào là tư duy DevOps.
- Biết về các dịch vụ AWS cho DevOps và quy trình CI/CD.
- Tìm hiểu khái niệm "Cơ sở hạ tầng bằng mã" (IaC).
- Khám phá các dịch vụ Container trên AWS.
- Cách giám sát và quan sát hệ thống (Monitoring & Observability).
- Nghe các kinh nghiệm thực tế (Best Practices) và bài học xương máu.

### Danh sách diễn giả

- **Truong Quang Tinh**
- **Nghiem Le**
- **Long Huynh**
- **Quy Pham**

### Nội dung nổi bật

#### Tư duy DevOps (DevOps Mindset)

- **Hợp tác là chính**: Dev và Ops phải làm việc chung, chia sẻ trách nhiệm chứ không đổ lỗi.
- **Tự động hóa tất cả**: Cái gì lặp lại thì nên viết tool tự động, đừng làm tay.
- **Học liên tục**: Công nghệ đổi mới liên tục nên phải chịu khó học và thử nghiệm cái mới.
- **Đo lường**: Làm gì cũng phải có số liệu để biết tốt hay xấu.

#### Hành trình làm DevOps (Người mới nên biết)

- **Nên làm (Do):**
    - Bắt đầu từ những cái cơ bản nhất (Linux, Network...).
    - Học đi đôi với hành: Tự tay làm các dự án thực tế.
    - Ghi chép tài liệu (Document) lại mọi thứ mình làm.
    - Học cái nào chắc cái đó, đừng lan man.
    - Rèn luyện kỹ năng mềm (giao tiếp).

- **Đừng làm (Don't):**
    - Đừng kẹt trong "địa ngục hướng dẫn" (xem tutorial suốt mà không tự làm).
    - Đừng copy-paste code một cách mù quáng mà không hiểu nó chạy sao.
    - Đừng so sánh mình với người khác, mỗi người một lộ trình.
    - Đừng bỏ cuộc khi gặp lỗi (bug).

#### Quy trình CI/CD

- Hiểu vòng đời của ứng dụng: Từ lúc viết code (coding), kiểm thử (testing), xem lại (review), chạy thử (pre-prod) đến lúc đưa ra cho người dùng thật (production).

#### Cơ sở hạ tầng bằng mã (IaC)

- **Nguyên lý**: Dùng dòng lệnh (code) để quản lý tài nguyên cloud chứ không ngồi click chuột thủ công.
- **Tự động hóa**: Tự động tạo, sửa, xóa hạ tầng để tránh sai sót.
- **Công cụ**: Terraform, OpenTofu, Pulumi.

#### Dịch vụ Container trên AWS

- Quản lý container bằng Docker, Kubernetes, Amazon ECR và Amazon EKS.
- **Amazon App Runner**: Một dịch vụ khá hay giúp triển khai web app trực tiếp từ code mà không cần lo về server hay cấu hình phức tạp (rất hợp cho người mới).

#### Giám sát & Quan sát (Monitoring & Observability)

- Áp dụng các phương pháp chuẩn với **Amazon CloudWatch** và **Amazon X-Ray** để biết hệ thống có đang "khỏe" không.

### Bài học rút ra

#### Các chỉ số DevOps cần quan tâm

- Theo dõi xem việc triển khai có ổn định không.
- Giúp hệ thống linh hoạt hơn, ít lỗi hơn.
- Đảm bảo trải nghiệm người dùng tốt nhất.
- Chứng minh được công nghệ mình dùng là đáng tiền.

#### Sự liên tục trong CI/CD

- Hiểu được bức tranh toàn cảnh của một đường ống (pipeline) CI/CD nó chạy từ đầu đến cuối như thế nào.

#### IaC trong AWS

- Biết cách dùng **Amazon CloudFormation** để tạo các mẫu (template) dựng sẵn hạ tầng, đỡ phải làm tay nhiều lần.

### Áp dụng vào thực tế

- **Xây lộ trình**: Tự lên kế hoạch học tập để theo nghề DevOps.
- **Thử nghiệm CI/CD**: Thử áp dụng pipeline tự động vào dự án hiện tại (đỡ phải deploy tay).
- **Làm Template**: Viết sẵn các mẫu code hạ tầng để dùng lại, giảm thiểu lỗi do con người (human errors).

### Trải nghiệm tham gia

Tham gia buổi workshop **“DevOps on AWS”** thực sự rất mở mang đầu óc, giống như có người vẽ đường cho mình đi trong nghề DevOps vậy. Một số trải nghiệm đáng nhớ:

#### Học hỏi từ các diễn giả
- Các chuyên gia từ FACJ chia sẻ rất thật về công việc DevOps hàng ngày.
- Được xem demo trực tiếp về cách giám sát hệ thống, nhìn rất chuyên nghiệp.

#### Khám phá CI/CD và IaC
- Hiểu được các công ty lớn họ update phần mềm liên tục như thế nào nhờ CI/CD pipeline.
- Biết cách viết code để dựng hạ tầng (dùng CloudFormation/CDK) thay vì chỉnh tay từng chút một.

#### Giám sát hệ thống
- Biết thêm về cách đặt cảnh báo (alerting) và xem dashboard, cũng như quy trình trực sự cố (on-call).

#### Một số hình ảnh tại sự kiện

![piture_1](/images/event3/4b3fbcde6db6e2e8bba7.jpg)
![piture_2](/images/event3/7f60cdb81cd0938ecac1.jpg)
![piture_3](/images/event3/f6f9ae1d7f75f02ba964.jpg)


> Tóm lại, sự kiện này giúp mình có cái nhìn toàn cảnh về nghề DevOps. Quan trọng hơn là học được mấy cái "bí kíp" (best practice) để áp dụng CI/CD và giám sát hệ thống sao cho chuẩn bài.