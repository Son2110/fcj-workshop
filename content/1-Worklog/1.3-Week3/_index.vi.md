---
title: "Worklog Tuần 3"
date: "2025-09-09"
weight: 1
chapter: false
pre: " <b> 1.3. </b> "
---

### Mục tiêu tuần 3:

* Hiểu về các dịch vụ EC2 và S3. 
* Có thể import (nhập) máy ảo vào AWS.
* Có thể export (xuất) máy ảo từ EC2 instance và AMI.
* Có thể mount (gắn) file sharing lên máy on-premise (máy chủ tại chỗ).

### Các công việc cần triển khai trong tuần này:
| Thứ | Công việc                                                                                                                                                                                                   | Ngày bắt đầu | Ngày hoàn thành | Nguồn tài liệu                            |
| --- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ | ------------ | --------------- | ----------------------------------------- |
| 2   | - Tìm hiểu về EC2 cơ bản: <br>&emsp; + Loại Instance (Instance Type) <br>&emsp; + User data <br>&emsp; + Meta data <br>&emsp; + EC2 Auto Scaling  <br>&emsp; + EFS/FSx <br>&emsp; + Lightsail <br>&emsp; + MGN                                                                                                     | 22/09/2025   | 22/09/2025      | <https://www.youtube.com/watch?v=e7XeKdOVq40&list=PLahN4TLWtox2a3vElknwzU_urND8hLn1i&index=73>|
| 3   | - **Thực hành:** <br>&emsp; + Tạo S3 Bucket <br>&emsp; + Tải dữ liệu lên (Load data) <br>&emsp; + Triển khai web sử dụng S3  <br>&emsp; + Tăng tốc Static Web với CloudFront                                         | 23/09/2025   | 23/09/2025      | <https://000057.awsstudygroup.com/> |
| 4   | - **Thực hành:** <br>&emsp; + Tạo Storage Gateway <br>&emsp; + Tạo File Shares <br>&emsp; + Mount File Shares trên máy on-premises <br>&emsp; + Tạo backup plan (kế hoạch sao lưu) <br>&emsp; + Thiết lập thông báo (notifications) | 24/09/2025   | 24/09/2025      | <https://000024.awsstudygroup.com/> <br> <https://000013.awsstudygroup.com/> |
| 5   | - Tìm hiểu về S3: <br>&emsp; + Access Point <br>&emsp; + Storage class  <br>&emsp; + Static Website & CORS <br>&emsp; + Glacier <br>&emsp; + Snow Family - Storage Gateway - Backup                             | 25/09/2025   | 25/09/2025      | <https://www.youtube.com/watch?v=hsCfP0IxoaM&list=PLahN4TLWtox2a3vElknwzU_urND8hLn1i&index=103> |
| 6   | - **Thực hành:** <br>&emsp; + Import máy ảo vào AWS  <br>&emsp; + Triển khai Instance từ AMI  <br>&emsp;   + Export máy ảo từ EC2 instance và từ AMI thông qua S3 bucket                                                                                 | 26/09/2025   | 26/09/2025      | <https://000014.awsstudygroup.com/> |


### Kết quả đạt được tuần 3:

* Đã hiểu về EC2 và nắm vững nhóm dịch vụ cơ bản:
  * User/Meta data
  * EC2 Auto Scaling
  * EFS/FSx 
  * Lightsail
  * MGN

* Đã hiểu S3 là gì và nắm vững:
  * Access point
  * Storage class
  * Static Website & CORS
  * Glacier

* Đã tạo và cấu hình thành công Static Web bằng cách sử dụng S3.

* Đã import thành công máy ảo vào AWS.

* Đã export thành công EC2 Instance từ AWS/AMI.

* Đã mount thành công file sharing lên máy on-premise.