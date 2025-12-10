---
title: "Worklog Tuần 8"
date: 2025-10-23
weight: 8
chapter: false
pre: " <b> 1.8. </b> "
---
### Mục tiêu Tuần 8:

* Hiểu database fundamentals: Primary Key, Foreign Key, và Normalization.
* Học Indexing, Partitioning, và Execution Plans cho query optimization.
* So sánh RDBMS vs NoSQL, và khái niệm OLTP vs OLAP/Data Warehouse.

### Các công việc trong tuần:

| Ngày | Công việc                                         | Ngày bắt đầu | Ngày hoàn thành | Tài liệu tham khảo |
| ---- | ------------------------------------------------- | ------------ | --------------- | ------------------ |
| 2    | Khái niệm database cơ bản trên AWS & managed services | 10/23/2025   | 10/23/2025      | tiếp tục           |
| 3    | Primary Key, Foreign Key & Normalization          | 10/24/2025   | 10/24/2025      | tiếp tục           |
| 4    | Indexing, Partitioning & Execution Plan           | 10/25/2025   | 10/25/2025      | tiếp tục           |
| 5    | Database Logs, Buffer Cache & Transactions        | 10/26/2025   | 10/26/2025      | tiếp tục           |
| 6    | RDBMS vs NoSQL, OLTP vs OLAP / Data Warehouse     | 10/27/2025   | 10/27/2025      | tiếp tục           |

### Kết quả đạt được Tuần 8:

#### Primary Key, Foreign Key & Normalization

**Primary Key (PK)**
* Xác định duy nhất mỗi record trong table.
* Không thể trùng lặp hoặc null.

**Foreign Key (FK)**
* Một column tham chiếu đến Primary Key của table khác.
* Tạo relationships giữa các tables.

**Normalization**
* Tổ chức data để **tránh trùng lặp** và **giảm storage usage**.
* Các dạng phổ biến: 1NF, 2NF, 3NF.

#### Indexing, Partitioning & Execution Plan

**Indexing**
* Tăng tốc data retrieval bằng cách tránh full table scans.
* Yêu cầu extra storage và tăng write cost.

**Partitioning**
* Chia large tables thành các phần nhỏ hơn cho queries nhanh hơn.
* Queries chỉ scan relevant partitions thay vì whole table.

**Execution Plan**
* Chiến lược step-by-step của database để chạy query.
* Thiết yếu cho query optimization.

#### RDBMS vs NoSQL, OLTP vs OLAP / Data Warehouse

**RDBMS (Relational Databases)**
* Structured tables với fixed schema.
* Strong consistency, hỗ trợ complex queries (SQL).
* Scales vertically.

**NoSQL**
* Flexible hoặc schema-less data model.
* High scalability, thường horizontal scaling.
* Phù hợp cho unstructured/semi-structured data.

**OLTP (Online Transaction Processing)**
* Xử lý real-time, frequent read/write operations.
* Dùng cho transactions như orders, payments, banking.

**OLAP (Online Analytical Processing) / Data Warehouse**
* Lưu trữ lượng lớn historical data cho analysis.
* Hỗ trợ complex queries cho reporting và business insights.
