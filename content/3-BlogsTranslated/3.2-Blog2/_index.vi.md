---
title: "Blog 2"
date: 2025-01-01
weight: 1
chapter: false
pre: " <b> 3.2. </b> "
---

# Truy vấn Amazon S3 Tables từ Open-Source Trino sử dụng Apache Iceberg REST Endpoint

**Tác giả:** Aneesh Chandra PN, Aritra Gupta, Ananta Khanal, và Madhu Nagaraj

**Ngày xuất bản:** 13 tháng 6, 2025

**Nguồn:** Advanced (300), Amazon EC2, Amazon S3 Tables, Amazon Simple Storage Service (S3), Amazon VPC, AWS CLI, AWS CloudFormation, AWS Identity and Access Management (IAM), Technical How-to

---

Các tổ chức ngày càng tập trung vào việc giải quyết thách thức ngày càng tăng trong việc quản lý và phân tích khối lượng dữ liệu khổng lồ, đồng thời đảm bảo các đội ngũ dữ liệu có quyền truy cập kịp thời để họ có thể nhanh chóng tạo ra insights và đưa ra quyết định. Các analysts và data scientists cần khả năng self-service analytics để xây dựng và duy trì các data products, thường liên quan đến các transformations phức tạp và cập nhật thường xuyên. Tuy nhiên, điều này tạo ra overhead vận hành đáng kể—từ việc quản lý small files và delete markers, đến xử lý metadata ngày càng mở rộng và chi phí lưu trữ tăng cho các phiên bản lịch sử. Nếu không được giải quyết, những thách thức này có thể ảnh hưởng đến hiệu suất truy vấn và tăng chi phí hạ tầng.

Các kiến trúc dữ liệu hiện đại đã phát triển để giải quyết những thách thức này bằng cách sử dụng các công nghệ open-source mạnh mẽ như **Trino** và **Apache Iceberg**, kết hợp với các managed services như **Amazon S3 Tables**. Trino xuất sắc trong việc distributed querying trên nhiều nguồn dữ liệu khác nhau, trong khi Apache Iceberg cung cấp một table format mạnh mẽ với ACID transactions, schema evolution, và time travel. S3 Tables bổ sung cho stack này bằng cách cung cấp automatic optimization và maintenance cho Iceberg tables, mang lại hiệu suất nhất quán mà không cần can thiệp thủ công.

Trong bài viết này, chúng tôi trình bày cách tích hợp Trino với S3 Tables để tạo một nền tảng analytics mạnh mẽ kết hợp những điểm tốt nhất của cả hai công nghệ. Chúng tôi hướng dẫn quy trình thiết lập sử dụng S3 Tables table-management APIs tương thích với tiêu chuẩn Apache Iceberg REST catalog, minh họa cách cấu hình tích hợp với Trino và kích hoạt các lợi ích chính như automatic data compaction và snapshot management. Đến cuối bài viết, bạn sẽ thấy cách sử dụng sự kết hợp mạnh mẽ này để xây dựng và duy trì các data products hiệu suất cao ở quy mô lớn.

Bài viết này tập trung vào cấu hình cho S3 Tables Iceberg REST endpoint. Bạn cũng có thể sử dụng S3 Tables với AWS Glue Iceberg REST endpoint. Để có unified governance trên tất cả tabular data, fine-grained access control, và centralized data management, chúng tôi khuyến nghị sử dụng **Amazon SageMaker Lakehouse**, hỗ trợ S3 Tables như một linked catalog.

---

## Tổng quan giải pháp

S3 Tables Iceberg REST endpoint cung cấp một API dựa trên tiêu chuẩn cho phép Trino giao tiếp trực tiếp với S3 Tables mà không cần bất kỳ proprietary connector hoặc middleware bổ sung nào. Điều này có nghĩa là các triển khai Trino của bạn có thể ngay lập tức tận dụng các tối ưu hóa tích hợp của S3 Tables cho Iceberg workloads.

Hướng dẫn này sẽ chỉ cho bạn cách:

* Triển khai môi trường Trino single-node sử dụng **AWS CloudFormation** template
* Cấu hình Iceberg REST connector để giao tiếp với S3 Tables
* Tạo schemas và tables trực tiếp trong S3 Tables từ Trino
* Thực hiện các thao tác Iceberg read/write đầy đủ
* Sử dụng các tính năng nâng cao như time travel và schema evolution

**Kiến trúc triển khai:** Mặc dù Trino thường được triển khai như một kiến trúc distributed multi-node cho production workloads để tận dụng parallel processing, bài viết này sử dụng một **Amazon Elastic Compute Cloud (Amazon EC2)** instance duy nhất để bạn có thể nhanh chóng khám phá tích hợp S3 Tables–Trino. Cấu hình Iceberg catalog sẽ giống nhau khi bạn triển khai trên Trino cluster của riêng mình cho các use cases production.

---

## Điều kiện tiên quyết

Bạn cần những điều sau để hoàn thành giải pháp này:

* Một AWS account với quyền tạo resources như EC2 instances, AWS Identity and Access Management (IAM) roles, và S3 table buckets
* Một Amazon Virtual Private Cloud (Amazon VPC) với public subnet (hoặc private subnet với VPN access)
* Một Amazon EC2 key pair cho SSH access
* AWS Command Line Interface (AWS CLI) đã cài đặt và cấu hình (tùy chọn, dùng để tạo S3 table bucket thủ công)

---

## Hướng dẫn từng bước

Các phần sau hướng dẫn bạn triển khai giải pháp này.

---

## Phần A: Triển khai Trino với S3 Tables Integration

Phần này cung cấp hướng dẫn từng bước để launching một môi trường Trino sử dụng CloudFormation template được cung cấp. Nó cũng mô tả các resources cần thiết để xây dựng giải pháp này. Để đơn giản hóa tích hợp giữa Trino và S3 Tables, chúng tôi đã tạo một CloudFormation template tự động hóa toàn bộ quy trình triển khai.

### 1. Tổng quan CloudFormation Template

CloudFormation template provisions tất cả các components cần thiết cho một môi trường Trino hoàn chỉnh có thể đọc và ghi dữ liệu vào S3 Tables thông qua Iceberg REST endpoint:

* **S3 table bucket:** Tạo một bucket chuyên dụng để lưu trữ dữ liệu của bạn
* **EC2 instance:** Triển khai một Trino server single-node sử dụng version 475
* **Security group:** Cấu hình network access cho SSH và Trino web UI
* **IAM instance profile:** Cấp cho EC2 instance các quyền phù hợp

### 2. Quy trình triển khai

1. Tải CloudFormation template.
2. Trong CloudFormation console, chọn **Create stack > With new resources (standard)**.
3. Upload template file và chọn **Next**.
4. Cung cấp giá trị cho các template parameters:
   * **Stack name:** Tên cho CloudFormation stack của bạn
   * **KeyName:** EC2 key pair hiện có cho SSH access
   * **VpcId:** VPC để triển khai EC2 instance
   * **SubnetId:** Public subnet trong VPC của bạn
   * **S3TablesBucketName:** Tên cho S3 table bucket của bạn
   * **AwsRegion:** AWS Region cho S3 Tables và AWS Glue
   * **TrinoInstanceType:** EC2 instance size (mặc định: `t3.xlarge`)
5. Chọn **Next** trên trang stack options.
6. Review chi tiết, xác nhận CloudFormation có thể tạo IAM resources, sau đó chọn **Create stack**.
7. Đợi stack creation hoàn thành (khoảng 10–15 phút).

Khi stack hoàn thành, bạn có thể tìm thông tin hữu ích trong tab **Outputs**:

* **PublicDNS:** Public DNS name của Trino instance
* **SSHCommand:** SSH command bạn có thể sử dụng để kết nối đến instance
* **TrinoURL:** URL cho Trino web UI
* **TableBucketName:** Tên của S3 table bucket

### 3. Deployment thực hiện những gì

CloudFormation template thực hiện một số tác vụ chính:

* **Infrastructure provisioning:** Thiết lập EC2 instance, security group, và S3 table bucket
* **Software installation:** Cài đặt Amazon Corretto Java 23 và Trino 475
* **Configuration:** Tạo các Trino configuration files cần thiết
* **Integration setup:** Cấu hình Iceberg REST connector cho S3 Tables

---

## Phần B: Kết nối Trino với Amazon S3 Tables sử dụng Iceberg REST Endpoint

CloudFormation template tự động cấu hình S3 Tables catalog trong Trino. Trong phần này, chúng ta sẽ xem xét cấu hình cho phép tích hợp này.

### 1. Chi tiết cấu hình Catalog

Một catalog trong Trino là một cấu hình cho phép truy cập vào một data source cụ thể. Một Trino cluster duy nhất có thể được cấu hình với nhiều catalogs, cung cấp truy cập đồng thời vào các data sources khác nhau.

Trong thiết lập này, CloudFormation template tạo một catalog properties file tại:

`/home/ec2-user/trino-server-475/etc/catalog/s3tables_irc.properties`

với cấu hình sau:

```properties
connector.name=iceberg
iceberg.catalog.type=rest
iceberg.rest-catalog.uri=https://s3tables.${AwsRegion}.amazonaws.com/iceberg
iceberg.rest-catalog.warehouse=arn:aws:s3tables:${AwsRegion}:${AWS::AccountId}:bucket/${S3TablesBucketName}
iceberg.rest-catalog.sigv4-enabled=true
iceberg.rest-catalog.signing-name=s3tables
iceberg.rest-catalog.view-endpoints-enabled=false
fs.hadoop.enabled=false
fs.native-s3.enabled=true
s3.iam-role=<ARN of the IAM ROLE with permissions to S3 Tables>
s3.region=${AwsRegion}
```

### 2. Các thuộc tính cấu hình S3 Tables Iceberg REST Endpoint

Bảng dưới đây liệt kê các thuộc tính chính trong cấu hình Trino catalog:

| Thuộc tính | Mô tả |
| --- | --- |
| `iceberg.rest-catalog.uri` | REST server API endpoint URI (**bắt buộc**) |
| `iceberg.rest-catalog.warehouse` | Warehouse identifier/location cho catalog (**bắt buộc**). Đối với S3 Tables, đây là ARN của S3 table bucket |
| `iceberg.rest-catalog.sigv4-enabled` | Phải đặt thành `true` (**bắt buộc**) |
| `iceberg.rest-catalog.signing-name` | Phải đặt thành `s3tables` (**bắt buộc**) |
| `iceberg.rest-catalog.view-endpoints-enabled` | Phải đặt thành `false` (**bắt buộc**) |
| `fs.hadoop.enabled` | Phải đặt thành `false` |
| `fs.native-s3.enabled` | Phải đặt thành `true` |
| `s3.iam-role` | ARN của IAM role với quyền truy cập S3 Tables |
| `s3.region` | AWS Region, ví dụ `us-east-1` |

---

## 3. Làm việc với S3 Tables trong Trino

Sau khi bạn đã thiết lập và cấu hình Trino để làm việc với S3 Tables, bạn có thể bắt đầu khám phá tích hợp.

### 3.1. Kết nối đến Trino

1. Thêm IP hoặc subnet mong muốn vào inbound rules của Trino security group để cho phép SSH access (port 22).
2. Kết nối đến EC2 instance sử dụng SSH command được cung cấp trong CloudFormation stack outputs:

   ```bash
   ssh -i your-key.pem ec2-user@your-instance-public-dns
   ```
3. Sau khi kết nối, sử dụng Trino CLI, được CloudFormation template cài đặt tự động:

   ```bash
   cd /home/ec2-user
   ./trino-cli --catalog s3tables_irc
   ```

### 3.2. Ví dụ: Tạo và Truy vấn Table

#### 3.2.1. Tạo Namespace

Đầu tiên, tạo một namespace (schema) trong S3 Tables. Namespace trong S3 Tables là một logical container hoặc organizational unit nhóm các tables và objects liên quan.

```sql
CREATE SCHEMA blog_namespace;
USE blog_namespace;
```

#### 3.2.2. Tạo Table

Tạo một table với nhiều data types khác nhau:

```sql
CREATE TABLE IF NOT EXISTS customers (
  customer_sk INT,
  customer_id VARCHAR,
  salutation VARCHAR,
  first_name VARCHAR,
  last_name VARCHAR,
  preferred_cust_flag VARCHAR,
  birth_day INT,
  birth_month INT,
  birth_year INT,
  birth_country VARCHAR,
  login VARCHAR
) WITH (
  format = 'PARQUET',
  sorted_by = ARRAY['customer_id']
);

SHOW TABLES;
DESCRIBE customers;
```

#### 3.2.3. Insert Data

```sql
INSERT INTO customers
VALUES
    (1, 'AAAAA', 'Mrs', 'Amanda',     'Olson',  'Y', 8,  4, 1984, 'US', 'aolson' ),
    (2, 'AAAAB', 'Mr',  'Leonard',    'Eads',   'N', 22, 6, 2001, 'US', 'leads' ),
    (3, 'BAAAA', 'Mr',  'David',      'White',  'Y', 16, 2, 1999, 'US', 'dwhite' );
```

#### 3.2.4. Truy vấn Data

```sql
SELECT * FROM customers LIMIT 10;
```

---

### 3.4. Các tính năng Iceberg nâng cao

#### 3.4.1. Truy vấn Table Metadata

```sql
-- Xem thông tin snapshot
SELECT * FROM "trino_customers$snapshots";

-- Xem thông tin data file
SELECT * FROM "trino_customers$files";
```

#### 3.4.2. Time Travel

**Time travel** là một tính năng mạnh mẽ của Apache Iceberg table format cho phép bạn truy vấn dữ liệu tại một thời điểm cụ thể trong quá khứ.

```sql
-- Time travel sử dụng snapshot ID
SELECT * FROM trino_customers FOR VERSION AS OF <snapshot ID>;

-- Time travel sử dụng timestamp
SELECT * FROM trino_customers FOR TIMESTAMP AS OF TIMESTAMP '2025-03-13 08:00:00.000 UTC';
```

#### 3.4.3. Xem History và Rollback

```sql
-- Xóa một record
DELETE FROM trino_customers WHERE customer_sk = 8;

-- Xem table history
SELECT * FROM "trino_customers$history";

-- Rollback về snapshot trước đó
ALTER TABLE trino_customers EXECUTE rollback_to_snapshot(<snapshot ID>);
```

---

## Cleanup

Để dọn dẹp resources:

1. **Xóa data và tables** (bạn có thể làm trực tiếp từ Trino CLI).
2. Trong AWS console, vào **CloudFormation** và xóa stack bạn đã tạo.

---

## Kết luận

Trong bài viết này, chúng tôi đã trình bày tích hợp liền mạch giữa Trino và Amazon S3 Tables sử dụng Iceberg REST endpoint. Sự kết hợp mạnh mẽ này cho phép bạn sử dụng distributed query engine của Trino để thực hiện interactive analytics trên dữ liệu được lưu trữ trong S3 Tables, đồng thời hưởng lợi từ các tính năng Iceberg như transactional support, schema evolution, và time travel—cùng với các tối ưu hóa Iceberg tích hợp được cung cấp bởi S3 Tables.

Giải pháp này mang lại một nền tảng linh hoạt, hiệu suất cao cho modern data analytics trên AWS.

---

### Về các tác giả

**Aneesh Chandra PN** là Principal Analytics Solutions Architect tại AWS làm việc với Strategic customers.

**Aritra Gupta** là Senior Technical Product Manager trong team Amazon S3 tại AWS.

**Ananta Khanal** là Senior Solutions Architect tại AWS tập trung vào data và cloud-computing solutions.

**Madhu Nagaraj** là Senior Technical Account Manager (TAM) tại Amazon Web Services (AWS) với hơn 20 năm kinh nghiệm trong IT.
