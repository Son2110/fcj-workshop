---
title: "Worklog Tuần 2"
date: "2025-09-09"
weight: 1
chapter: false
pre: " <b> 1.2. </b> "
---


### Mục tiêu tuần 2:

* Hiểu dịch vụ EC2, CloudFormation, VPC peering, AWS Transit Gateway
* Biết cách khởi tạo instance Linux và Windows

### Các công việc cần triển khai trong tuần này:

| Thứ | Công việc                                                                                                                                                                                                   | Ngày bắt đầu | Ngày hoàn thành | Nguồn tài liệu                            |
| --- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ | ------------ | --------------- | ----------------------------------------- |
| 2   | - Học về EC2 <br>&emsp; + Instance Type <br>&emsp; + User data <br>&emsp; + Meta data <br>&emsp; + EC2 auto scaling - EFS/FSx - Lightsail - MGN <br> - **Practice:** <br>&emsp; + Tạo Linux và Windows VPC <br>&emsp; + Tạo Security Group cho Linux và Windows Instance <br>&emsp; + Khởi tạo Linux và Windows Instance <br>&emsp; + Kết nối với Linux và Windows Instance <br>&emsp; + Triển khai ứng dụng AWS User Management trên Amazon EC2 Linux/Windows                                                           | 09/15/2025 | 09/15/2025      | <https://000004.awsstudygroup.com/>
| 3   | - Học về Hybrid DNS với Route 53 <br>&emsp; + Outbound Endpoints <br>&emsp; + Inbound Endpoints <br>&emsp; + Route 53 Resolver Rules <br> - **Practice:** <br>&emsp; + Tạo key pair <br>&emsp; + Tạo CloudFormation Template, Security Group <br>&emsp; + Kết nối với RDGW                                          | 09/16/2025 | 09/16/2025      | <https://000010.awsstudygroup.com/> |
| 4   | - Học về VPC peering <br> - **Practice:** <br>&emsp; + Tạo VPC peering  | 09/17/2025 | 09/17/2025      | <https://000019.awsstudygroup.com/> |
| 5   | - Tham gia sự kiện AWS Cloud Day                       | 09/18/2025 | 09/18/2025      |  |
| 6   | - Học về AWS Transit Gateway <br> - **Practice:** <br>&emsp; + Tạo AWS Transit Gateway                                                                                      | 09/19/2025 | 09/19/2025      | <https://000020.awsstudygroup.com/> |



### Kết quả đạt được tuần 2:

* Hiểu được EC là gì và nắm được các kiến thức cơ bản về EC2:
  * Instance Type
  * AMI
  * Key Pair 
  * Backup
  
* Thành công tạo Instance từ custim AMI and truy cập WIndows/Linux Instance khi mất key pair

* Tạo thành công Hybrid DNS with Route 53 Resolver, kêt nối với RDGW and thiết lập  DNS:
  * Outbound Endpoint
  * Inbound Endpoint
  * Route 53 Resovler Rules

* Hiểu về VPC Peering và thiết lập:
  * CloudFormation Template
  * Security Group
  * EC2 Instance
  * Network ACL
  * Route Tables
  * Cross-Peer DNS

* Hiểu về AWS Transite Gateway and know to create:
  * Transite Gateway
  * Transite Gateway Attachment
  * Transite Gateway Route Tables
  * Add Transite Gateway Routes to VPC Route Tables



