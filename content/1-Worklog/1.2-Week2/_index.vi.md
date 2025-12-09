---
title: "Worklog Tuần 2"
date: "2025-09-09"
weight: 1
chapter: false
pre: " <b> 1.2. </b> "
---


### Mục tiêu tuần 2:

* Hiểu về EC2, CloudFormation, VPC peering, AWS Transit Gateway.
* Biết cách khởi chạy Amazon Linux Instance và Microsoft Windows Instance.

### Các công việc cần triển khai trong tuần này:
| Thứ | Công việc                                                                                                                                                                                                   | Ngày bắt đầu | Ngày hoàn thành | Nguồn tài liệu                            |
| --- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ | ------------ | --------------- | ----------------------------------------- |
| 2   | - **Thực hành:** <br>&emsp; + Tạo VPC cho Linux và Windows <br>&emsp; + Tạo Security Group (SC) cho Linux và Windows Instance <br>&emsp; + Khởi chạy Linux và Windows Instance <br>&emsp; + Kết nối tới Linux và Windows Instance  <br>&emsp; + Triển khai ứng dụng quản lý người dùng AWS trên Amazon EC2 Linux/Windows                                                           | 15/09/2025   | 15/09/2025      | <https://000004.awsstudygroup.com/>
| 3   | - Tìm hiểu về Hybrid DNS với Route 53: <br>&emsp; + Outbound Endpoints <br>&emsp; + Inbound Endpoints <br>&emsp; + Route 53 Resolver Rules <br> - **Thực hành:** <br>&emsp; + Tạo key pair <br>&emsp; + Tạo CloudFormation Template, SC <br>&emsp; + Kết nối tới RDGW                                          | 16/09/2025   | 16/09/2025      | <https://000010.awsstudygroup.com/> |
| 4   | - Tìm hiểu về VPC peering  <br> - **Thực hành:** <br>&emsp; + Tạo VPC peering  | 17/09/2025   | 17/09/2025      | <https://000019.awsstudygroup.com/> |
| 5   | - Tham gia sự kiện AWS Cloud Day                       | 18/09/2025   | 18/09/2025      |  |
| 6   | - Tìm hiểu về AWS Transit Gateway  <br> **Thực hành:** <br>&emsp; + Tạo AWS Transit Gateway                                                                                      | 19/09/2025   | 19/09/2025      | <https://000020.awsstudygroup.com/> |



### Kết quả đạt được tuần 2:

* Hiểu EC2 là gì và nắm vững các kiến thức cơ bản về EC2: 
  * Instance Type
  * AMI
  * Key Pair
  * Backup

* Khởi chạy thành công Instance từ custom AMI và truy cập vào Windows/Linux Instance khi mất key pair.

* Tạo thành công Hybrid DNS với Route 53 Resolver , kết nối tới RDGW và thiết lập DNS:
  * Outbound Endpoint
  * Inbound Endpoint
  * Route 53 Resolver Rules

* Hiểu về VPC Peering và thiết lập:
  * CloudFormation Template
  * Security Group
  * EC2 Instance
  * Network ACL
  * Route Tables
  * Cross-Peer DNS

* Hiểu về AWS Transit Gateway và biết cách tạo:
  * Transit Gateway
  * Transit Gateway Attachment
  * Transit Gateway Route Tables
  * Thêm các tuyến Transit Gateway vào VPC Route Tables