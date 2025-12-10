---
title: "Các bài blogs đã dịch"
date: 2025-01-01
weight: 3
chapter: false
pre: " <b> 3. </b> "
---

Phần này chứa các bài blog AWS mà tôi đã dịch từ tiếng Anh sang tiếng Việt trong chương trình thực tập.

---

### [Blog 1 - Cách SAN Boot môi trường Amazon EC2 doanh nghiệp từ Amazon FSx for NetApp ONTAP](3.1-Blog1/)

Blog này giải thích cách triển khai SAN boot cho các instance Amazon EC2 sử dụng Amazon FSx for NetApp ONTAP. Bạn sẽ tìm hiểu về các lợi ích của boot-from-SAN bao gồm quản lý tập trung, thin cloning với golden images, giảm chi phí thông qua snapshot-based cloning, và đơn giản hóa quy trình HA/DR. Bài viết đề cập đến quy trình boot kỹ thuật sử dụng iPXE chain-loading và các best practices cho triển khai production.

---

### [Blog 2 - Truy vấn Amazon S3 Tables từ Open-Source Trino sử dụng Apache Iceberg REST Endpoint](3.2-Blog2/)

Blog này trình bày cách tích hợp Trino với Amazon S3 Tables sử dụng Iceberg REST endpoint. Bạn sẽ học cách triển khai môi trường Trino bằng CloudFormation, cấu hình Iceberg REST connector, tạo schemas và tables, thực hiện các thao tác đọc/ghi, và sử dụng các tính năng nâng cao như time travel và schema evolution. Giải pháp cung cấp một nền tảng phân tích mạnh mẽ kết hợp distributed query engine của Trino với các tối ưu hóa tích hợp của S3 Tables.

---

### [Blog 3 - SMS Onboarding cho SaaS, ISVs và Ứng dụng Multi-Tenant với AWS End User Messaging](3.3-Blog3/)

Blog này cung cấp hướng dẫn toàn diện cho các nhà cung cấp công nghệ tích hợp khả năng SMS vào sản phẩm của họ. Bạn sẽ tìm hiểu về opt-in flows, các mô hình kiến trúc để triển khai dịch vụ SMS (từ "Bring Your Own AWS Account" đến "Fully Managed Program"), chiến lược định giá, các cân nhắc về địa lý, và best practices để giảm ma sát trong quá trình triển khai. Bài viết giúp các công ty SaaS, ISVs và nhà cung cấp giải pháp multi-tenant điều hướng trong bối cảnh SMS phức tạp.
