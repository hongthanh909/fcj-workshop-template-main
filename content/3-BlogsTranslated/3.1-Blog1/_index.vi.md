---
title: "Blog 1"
date: 2025-06-13
weight: 1
chapter: false
pre: " <b> 3.1. </b> "
---
# Cách SAN Boot môi trường Amazon EC2 doanh nghiệp từ Amazon FSx for NetApp ONTAP

**Tác giả:** Randy Seamans
**Ngày xuất bản:** 13 tháng 6, 2025
**Nguồn:** Advanced (300), Amazon EC2, Amazon Elastic Block Store (Amazon EBS), Amazon FSx for NetApp ONTAP, AWS Partner Network, Migration, Storage, Thought Leadership

---

## Giới thiệu

Theo truyền thống, nhiều doanh nghiệp và tổ chức sử dụng hạ tầng on-premises đã triển khai boot-from-SAN (Storage Area Network) thay vì sử dụng bộ nhớ gắn cục bộ. Boot từ SAN cung cấp quản lý tập trung và sao lưu các boot volumes, hỗ trợ tính sẵn sàng cao thông qua multipathing, và mang lại sự linh hoạt hơn bằng cách cho phép các hệ thống boot từ các OS images được cấu hình sẵn lưu trữ trên các storage arrays dùng chung để giảm chi phí.

Amazon FSx for NetApp ONTAP mang những lợi ích này lên cloud. Là một dịch vụ được quản lý hoàn toàn bởi Amazon Web Services (AWS), FSx for ONTAP cung cấp một virtual storage array cấp doanh nghiệp hỗ trợ các tính năng như high-throughput I/O, deduplication, compression, compaction, replication, và block-level access thông qua iSCSI và NVMe/TCP.

Quan trọng nhất cho SAN boot là khả năng thin cloning. FSx for ONTAP cho phép một LUN được provisioned mỏng hoạt động như một "golden image" cơ sở cho hệ điều hành (OS). Các snapshot clones read-write của LUN này có thể được provisioned nhanh chóng và phân phối đến hàng trăm servers như các boot volumes riêng biệt. Mỗi clone chỉ lưu trữ những khác biệt tối thiểu để nhận dạng từng server, giảm đáng kể tổng yêu cầu dung lượng lưu trữ.

Hơn nữa, FSx for ONTAP nhận biết các vùng dữ liệu dùng chung, cho phép các blocks được truy cập thường xuyên được cache trong bộ nhớ một lần và phục vụ cho tất cả các clones, mở rộng hiệu quả cache và cải thiện hiệu suất tổng thể. Vì FSx for ONTAP cũng cung cấp tính sẵn sàng cao (HA) và khôi phục thảm họa (DR) thông qua các cơ chế replication tiên tiến, các boot volumes có thể được tích hợp vào các quy trình HA/DR. Điều này đảm bảo trạng thái OS vẫn nhất quán trên các môi trường mà không cần can thiệp thủ công.

---

## Bối cảnh về Boot Devices trong AWS

Các AWS instances thường boot từ Amazon Elastic Block Store (Amazon EBS) volumes, được tích hợp chặt chẽ với Amazon Elastic Compute Cloud (Amazon EC2). Sự tích hợp này cho phép thời gian boot nhanh và ổn định thông qua các tính năng như EBS Fast Snapshot Restore và EBS Provisioned IOPS cho việc khởi tạo volume.

Amazon EBS cũng cung cấp bảo mật nâng cao với customer-managed key (CMK) encryption, độ tin cậy cao thông qua các boot volumes hoạt động độc lập, và time-based AMI copies để phân phối hiệu quả và nhất quán trên các Regions. Được thiết kế cho cả workloads đa mục đích và hiệu suất cao, Amazon EBS là boot device mặc định cho Amazon EC2.

Trong bài viết này, tôi sẽ trình bày cách bạn có thể boot từ iSCSI LUNs được lưu trữ trên FSx for ONTAP file systems trong cấu hình Single-Availability Zone (AZ) hoặc Multi-AZ. Các LUNs này có thể được thin provisioned, tiết kiệm không gian, và replicated giữa các AZs hoặc các AWS Regions khác nhau.

Khi được cấu hình đúng cách, SAN booting từ FSx for ONTAP có thể giúp giảm chi phí lưu trữ ở quy mô lớn trong khi đơn giản hóa các hoạt động HA/DR.

---

## Hai Use Cases chính cho SAN Boot với FSx for ONTAP

### 1. Giảm chi phí Boot Volume

Trong môi trường on-premises, SAN boot thường được sử dụng để giảm chi phí khi triển khai hàng trăm servers với các boot volumes gần như giống hệt nhau. Nguyên tắc này cũng áp dụng trong cloud khi sử dụng iSCSI boot với FSx for ONTAP.

Bằng cách tận dụng thin provisioning và snapshot-based cloning, yêu cầu lưu trữ cho 100 đến 200 boot volumes chỉ cao hơn một chút so với một boot volume duy nhất. Mỗi server chỉ tiêu thụ dung lượng cho những khác biệt độc đáo của nó so với golden image, giảm đáng kể tổng mức sử dụng lưu trữ.

Hơn nữa, bằng cách tuân theo các best practices được đề cập sau trong bài viết này, bạn có thể tránh việc provisioning IOPS chuyên dụng cho các boot volumes nhờ khả năng performance pooling của FSx for ONTAP. Kết quả là tiết kiệm chi phí đáng kể trong khi duy trì hiệu suất nhất quán.

### 2. Đơn giản hóa HA/DR và OS Lifecycle Management

Cập nhật OS và thay đổi cấu hình là các yêu cầu thường xuyên trong enterprise workloads. Sử dụng SAN boot tối ưu hóa các quy trình HA/DR bằng cách replicate các boot volumes trên nhiều Availability Zones (AZs) và các AWS Regions từ xa.

FSx for ONTAP hỗ trợ cả multi-AZ replication và long-distance replication, vì vậy bất kỳ thay đổi nào đối với OS hoặc boot volume đều được đồng bộ hóa tự động và luôn ở trạng thái sẵn sàng cao. Điều này giảm đáng kể các bước thủ công cần thiết để khôi phục trong các sự cố trong khi hạn chế rủi ro lỗi con người, giúp dễ dàng đáp ứng các mục tiêu thời gian khôi phục (RTO) nghiêm ngặt.

Ngoài ra, các bản cập nhật có thể được triển khai trước trên một clone của golden image, được kiểm tra kỹ lưỡng, và chỉ được đưa vào production sau khi được xác nhận, hợp lý hóa quy trình cập nhật OS và giảm thiểu gián đoạn.

---

## Cách SAN Boot từ FSx for ONTAP Volumes hoạt động

Để thực hiện SAN boot từ FSx for ONTAP, chúng ta sử dụng khái niệm network-based chain-loader boot device, đôi khi được gọi là "jumpboot."

Ban đầu, EC2 instance nhanh chóng boot một OS nhỏ gọn, bị khóa từ một EBS volume 1 GB chứa Preboot eXecution Environment (iPXE). iPXE sau đó chain-boots đến một volume lưu trữ Linux hoặc Windows OS thực tế nằm trên FSx for ONTAP.

Người dùng có thể compile iPXE Amazon Machine Image (AMI) của riêng họ hoặc sử dụng AWS certified iPXE AMI có sẵn trong mọi AWS Region như một community AMI. Cơ chế chain-loading này vẫn cho phép tích hợp với Amazon EC2 console cho các hoạt động như launch, start, stop, hoặc sử dụng serial console.

iPXE biết FSx for ONTAP và iSCSI volume nào để boot từ đâu? Khi launching một EC2 instance với iPXE AMI, chúng ta truyền thông tin đó trong user data script, và iPXE sau đó chain-boots OS mới được lưu trữ trên block volume được chỉ định.

---

## Các cân nhắc thực tế và Best Practices

Boot từ SAN sử dụng FSx for ONTAP trong AWS mang đến một số cân nhắc cho việc lập kế hoạch và vận hành, tương tự như môi trường SAN on-premises truyền thống, với một số best practices đặc thù cho cloud.

### OS Licensing

Các boot volumes thường được clone, vì vậy mỗi instance phải tuân thủ đầy đủ các yêu cầu licensing tương ứng, đặc biệt đối với các hệ điều hành thương mại như Microsoft Windows.

### Storage Placement

Trừ khi có các yêu cầu cụ thể khác, tốt nhất là đặt cả boot volumes và data volumes cho một EC2 instance trên cùng một FSx for ONTAP system. Điều này đảm bảo data locality tối ưu và duy trì hiệu suất nhất quán.

### Tránh Boot Storms

Một best practice quan trọng khác là tránh tập trung quá nhiều boot volumes trên một FSx for ONTAP system duy nhất. Trong các kịch bản khôi phục quy mô lớn, thường được gọi là "boot storms," điều này có thể dẫn đến độ trễ thời gian boot.

May mắn thay, không giống như các hệ thống on-premises truyền thống, trong AWS hầu như không có sự khác biệt chi phí đáng kể khi phân phối cùng một dung lượng lưu trữ trên nhiều FSx for ONTAP systems. Điều này cho phép bạn mở rộng theo chiều ngang mà không phát sinh chi phí bổ sung lớn, đảm bảo tránh được boot storms.

Ví dụ, một FSx for ONTAP system với 50 TB SSD capacity, theo mặc định và không cần provisioning IOPS chuyên dụng, có thể đạt tới 150,000 IOPS. Nếu hệ thống này hỗ trợ SAN boot cho 200 servers, trong giai đoạn boot mỗi server sẽ trung bình 750 IOPS mỗi giây—nhanh hơn gấp sáu lần so với HDD thông thường. Vì applications và boot được đặt cùng vị trí, không có application IO cạnh tranh với boot process trong quá trình khởi động.

### Cấu hình Multipathing

Để ngăn ngừa gián đoạn, đảm bảo multipathing được cấu hình và xác nhận đúng cách cho tất cả các boot volumes kết nối qua iSCSI. Các cơ chế path failover đáng tin cậy rất quan trọng để duy trì cả hiệu suất và fault tolerance.

### Kiểm tra HA và Failover

Cuối cùng, việc kiểm tra các cấu hình HA và failover trước khi đưa vào production là cực kỳ quan trọng. Bạn có thể mô phỏng một sự kiện failover bằng cách tạm thời tăng throughput capacity của FSx for ONTAP system. Điều này kích hoạt một non-disruptive controller failover, cho phép bạn xác minh multipath handling và OS stability. Sau khi xác nhận thành công, bạn có thể giảm throughput trở lại mức yêu cầu.

---

## Bắt đầu

Có khá nhiều bước liên quan đến việc thiết lập SAN boot OS trong AWS với FSx for ONTAP. Cách tốt nhất để bắt đầu là khám phá trang web iPXE dành riêng cho AWS, và/hoặc liên hệ trực tiếp với AWS nếu tổ chức của bạn cần triển khai SAN boot.

Đối với các dự án migration quy mô lớn sang SAN boot từ môi trường on-premises hoặc từ môi trường AWS hiện có, Cirrus Data—một AWS Partner—cung cấp các giải pháp tự động hóa toàn bộ quy trình, bao gồm iSCSI, cấu hình multipathing, provisioning, và LUN mapping, tất cả đều là các tác vụ phức tạp ở quy mô lớn.

---

## SAN Boot từ FSx for ONTAP có phù hợp với bạn không?

Boot từ SAN sử dụng FSx for ONTAP block storage không phải là lựa chọn cho hầu hết các môi trường AWS, nhưng đối với các tổ chức đã quen với việc sử dụng SAN boot để đơn giản hóa các quy trình DR hoặc điều phối hạ tầng quy mô lớn với các OS images đồng nhất, khả năng này hiện đã có sẵn trên AWS.

Nếu bạn đang quản lý một fleet lớn các EC2 instances yêu cầu khả năng HA, failover giữa các AZs, hoặc cross-Region replication, FSx for ONTAP sẽ giúp bạn giảm đáng kể chi phí boot volume trong khi tối ưu hóa các quy trình failover và DR.

Tóm lại, bạn hoàn toàn có thể áp dụng các chiến lược SAN boot đã được chứng minh trong môi trường cloud với quy mô và độ bền của AWS.

---

## Tags

Amazon EC2, Amazon FSx for NetApp ONTAP, AWS Partner Network, AWS Storage, Data Migration

---

## Về tác giả

**Randy Seamans** là một chuyên gia storage industry kỳ cựu và Principal Storage Specialist & Advocate cho AWS, chuyên về High Performance Storage (HPC/AI), Enterprise Storage, và Disaster Recovery.

Để biết thêm insights và thông tin thú vị về Storage, hãy theo dõi anh ấy tại: https://www.linkedin.com/in/storageperformance
