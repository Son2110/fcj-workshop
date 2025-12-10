---
title: "Event 4"
date: "2025-09-09"
weight: 4
chapter: false
pre: " <b> 4.4. </b> "
---


# Báo cáo tổng hợp: "AWS Cloud Mastery Series #3: Theo AWS Well-Architected Security Pillar"

### Mục đích của sự kiện

- Nắm nền tảng về Bảo mật (Security Foundation).
- Quản lý xem ai được vào và được làm gì (Identity & Access Management).
- Phát hiện kẻ xấu (Detection).
- Bảo vệ hạ tầng và dữ liệu (Infrastructure & Data Protection).
- Làm gì khi "có biến" (Incident Response).

### Danh sách diễn giả

- **Le Vu Xuan An**
- **Tran Duc Anh**
- **Tran Doan Cong Ly**
- **Danh Hoang Hieu Nghi**
- **Thinh Lam**
- **Viet Nguyen**
- **Mendel Branski (Long)**

### Nội dung nổi bật

#### Giới thiệu về Cloud Club

- Biết thêm về các CLB Cloud tại trường đại học như UTE, SGU. Rất vui khi thấy cộng đồng sinh viên hoạt động sôi nổi.

#### Nền tảng bảo mật

- **SCPs**: Luật chơi chung cho toàn bộ tổ chức, không ai được phá.
- **Permission Boundaries**: Đặt giới hạn để không ai có quyền lực quá lớn.
- **MFA**: Cái này bắt buộc phải có để bảo vệ tài khoản.

#### Phát hiện và Giám sát

- **Bảo mật nhiều lớp**: Kiểm tra ở mọi ngóc ngách chứ không chỉ ở cửa ra vào.
- **Cảnh báo (Alerting)**: Dùng EventBridge để hệ thống tự "la làng" khi có gì đó đáng ngờ.
- **Detection-as-Code**: Dùng code để tự động phát hiện mối đe dọa (nghe rất ngầu và tiện).

#### Ứng phó sự cố (Incident Response)

- **Phòng ngừa**: Cách tốt nhất để xử lý sự cố là đừng để nó xảy ra.
- **"Ngủ ngon hơn"**: Thiết lập hệ thống tốt để đêm không bị dựng dậy sửa lỗi.
- Quy trình từng bước để xử lý khi hệ thống bị tấn công hoặc sập.

### Bài học rút ra

#### Service Control Policies (SCPs)

- Coi cái này như "Hiến pháp" của tài khoản AWS vậy.
- Nó kiểm soát quyền hạn *tối đa*. Dù bạn là Admin, nhưng SCP bảo "Cấm" thì là **Cấm**.
- Nó không bao giờ cấp quyền, nó chỉ dùng để chặn/lọc quyền thôi.

#### Permission Boundaries

- Một cách để ngăn chặn người dùng (hoặc Role) tự cấp quyền quá cao cho bản thân. Nó tạo ra một cái "trần" quyền hạn không thể vượt qua.

#### Multi-Factor Authentication (MFA) 

- **TOTP**: Mã 6 số trên Google Authenticator. Miễn phí, dễ dùng, sao lưu linh hoạt.
- **FIDO2**: Khóa vật lý (USB) chạm vào là xong. Bảo mật cao hơn, khó bị hack hơn nhưng tốn tiền mua khóa.

#### Detection-as-Code

- Dùng các công cụ như CloudFormation/Terraform để tự động bật **GuardDuty** (trình phát hiện mối đe dọa) trên toàn bộ hệ thống.
- **Quản lý phiên bản**: Lưu các luật bảo mật trên Git để theo dõi thay đổi y như code phần mềm.

#### Ứng phó sự cố

- **Chuẩn bị**: Phải có tool tự động *trước khi* sự cố xảy ra.
- **Rút kinh nghiệm**: Sau khi sửa xong, luôn phải họp lại để xem tại sao bị lỗi để lần sau không bị nữa.

### Ứng dụng vào công việc

- **Nguyên tắc đặc quyền tối thiểu**: Rà soát lại dự án, chỉ cấp quyền vừa đủ dùng, không cấp Admin lung tung nữa.
- **Bật MFA**: Bắt buộc tất cả các tài khoản phải có xác thực 2 bước.
- **Dự đoán**: Luôn tự hỏi "Lỡ cái này hỏng thì sao?" và chuẩn bị sẵn phương án.

### Trải nghiệm tại sự kiện

Tham gia buổi workshop **“AWS Well-Architected Security Pillar”** giúp mình nhận ra bảo mật không chỉ là cài firewall, mà là cả một tư duy hệ thống.

#### Học hỏi từ các diễn giả giàu chuyên môn 
- Nghe các anh chia sẻ về cách xử lý khi "có biến" (incident) rất thực tế. Các anh nhấn mạnh nhiều vào việc "học được gì sau sự cố" thay vì chỉ lo sửa cho xong.

#### Giao lưu với Cloud Club
- Được biết thêm về các hoạt động của Cloud Club, nơi kết nối những người học AWS từ khắp nơi.

#### Cảnh báo & Tự động hóa
- Hiểu rằng mình không thể trực màn hình 24/7. Phải biết dùng CloudTrail, EventBridge để hệ thống tự canh gác và báo động ngay lập tức khi có vấn đề.


> Tóm lại, sự kiện này là cơ hội để mình mở rộng kiến thức về Tự động hóa và Bảo mật. Mình hiểu ra bảo mật không chỉ là khóa cửa, mà là có hệ thống báo động thông minh và biết phải làm gì khi chuông báo động reo.