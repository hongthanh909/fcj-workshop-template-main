---
title: "Blog 3"
date: 2025-01-01
weight: 1
chapter: false
pre: " <b> 3.3. </b> "
---

# SMS Onboarding cho SaaS, ISVs và Ứng dụng Multi-Tenant với AWS End User Messaging

**Tác giả:** Tyler Holmes

**Ngày xuất bản:** 13 tháng 5, 2025

**Nguồn:** AWS End User Messaging, Messaging, SaaS

---

## Giới thiệu

SMS messaging vẫn là một trong những kênh giao tiếp đáng tin cậy và hiệu quả nhất. Tuy nhiên, đối với các công ty software-as-a-service (SaaS), independent software vendors (ISVs), và các nhà cung cấp giải pháp multi-tenant muốn tích hợp khả năng SMS vào sản phẩm của họ, hành trình có thể trở nên phức tạp và đầy thách thức.

Hướng dẫn này được thiết kế đặc biệt cho các nhà cung cấp công nghệ—dù bạn là công ty SaaS, ISV, hay bất kỳ nền tảng nào cho phép khách hàng của bạn gửi tin nhắn SMS đến người dùng cuối. Trong suốt bài viết này, chúng tôi sử dụng các thuật ngữ sau:

* **Provider (Nhà cung cấp):** Tổ chức cung cấp khả năng SMS như một phần của sản phẩm hoặc dịch vụ của họ.
* **Customer (Khách hàng):** Các thực thể sử dụng công nghệ của Provider để gửi tin nhắn SMS.
* **End User (Người dùng cuối):** Những người nhận đã đồng ý nhận tin nhắn SMS từ Customer.

Triển khai SMS có thể phức tạp, với các quy định khác nhau theo từng quốc gia, quy trình đăng ký có thể mất hàng tuần hoặc thậm chí hàng tháng, nhiều loại originator (long codes, short codes, sender IDs, v.v.) với các khả năng khác nhau, và nhu cầu đa dạng giữa Customers và End Users. Những thách thức này còn rõ rệt hơn khi bạn, với tư cách là Provider, đang cung cấp SMS như một dịch vụ cho Customers của riêng bạn, những người lại phục vụ End Users.

Đến cuối hướng dẫn này, bạn sẽ hiểu:

* Cách opt-in flows ảnh hưởng đến kiến trúc của bạn.
* Các tùy chọn để cấu trúc dịch vụ SMS cho Customers.
* Các chiến lược để giảm độ phức tạp trong quá trình triển khai SMS.

Hãy bắt đầu.

---

## Vấn đề đăng ký: Ai sở hữu mối quan hệ?

Một trong những quyết định quan trọng nhất khi đăng ký SMS originator là xác định thông tin của ai sẽ được sử dụng trong đăng ký. Sai lầm lớn nhất mà AWS thường thấy các Providers mắc phải là không hiểu cách mối quan hệ của họ với Customers và End Users của Customers sẽ ảnh hưởng đến kiến trúc hệ thống và cách họ hoàn thành các bước đăng ký bắt buộc.

Các mobile network operators muốn biết ai sẽ gửi tin nhắn SMS đến subscribers của họ, thực thể đó thu thập opt-ins như thế nào, và nội dung nào sẽ được gửi. Khi đăng ký originator—đặc biệt tại Hoa Kỳ—bạn phải giải thích rõ ràng cách End Users sẽ opt in và xác nhận rằng dữ liệu của họ sẽ không được chia sẻ với bất kỳ bên thứ ba nào. Kiến trúc của bạn phải đảm bảo:

* Quy trình opt-in rõ ràng với thực thể chịu trách nhiệm được xác định rõ.
* Chính sách bảo mật phản ánh chính xác cách dữ liệu được chia sẻ.
* Tuân thủ các quy định chia sẻ dữ liệu bên thứ ba.

AWS thường thấy các Providers đăng ký chính họ là originator mặc dù họ không có mối quan hệ trực tiếp với End Users của Customer. Quyết định về thông tin của ai để sử dụng trong đăng ký chủ yếu phụ thuộc vào một câu hỏi cơ bản:

> **End User tin rằng họ đang hình thành mối quan hệ với ai khi họ cung cấp số điện thoại của mình?**

Các kịch bản phổ biến nhất được mô tả dưới đây.

---

## Kịch bản 1: End Users chỉ tương tác với thương hiệu của Customer

Trong hầu hết các trường hợp, End Users hoàn toàn không biết đến sự tồn tại của bạn với tư cách là Provider. Họ tin rằng họ đang opt in để nhận tin nhắn trực tiếp từ Customer của bạn. Trong kịch bản này:

* Đăng ký nên được thực hiện sử dụng thông tin của Customer. Có nhiều cách bạn có thể hỗ trợ quy trình này, và sau trong bài viết này chúng tôi sẽ thảo luận một số phương pháp để giảm các điểm ma sát phổ biến.
* Tin nhắn phải xuất hiện như được gửi từ Customer, không phải từ Provider, và tên dịch vụ của bạn không nên xuất hiện trong nội dung tin nhắn.

---

## Kịch bản 2: End Users Opt In trực tiếp thông qua ứng dụng của Provider

Trong một số trường hợp, End Users hiểu rõ ràng rằng họ đang opt in để nhận tin nhắn thông qua nền tảng công nghệ của bạn, thay mặt cho Customers của bạn. Dữ liệu opt-in không được chia sẻ với Customer, và thương hiệu của bạn với tư cách là Provider là thực thể được đặt tên trong tất cả các tin nhắn SMS gửi đi.

Điều này có thể xảy ra theo nhiều cách:

* End Users có thể opt in sử dụng widget bạn xây dựng mà Customers cài đặt trên websites hoặc apps của họ.
* Một form giấy hoặc phone script bạn cung cấp, xác định rõ ràng bạn là Provider.

AWS thường thấy kịch bản này với các Providers cung cấp:

* Third-party payment processing
* Shipping và logistics support
* Customer service platforms
* One-time-password (OTP) capabilities

Trong kịch bản này, tên công ty của bạn thường sẽ xuất hiện trong tin nhắn, và đăng ký sẽ sử dụng thông tin công ty của bạn.

> **LƯU Ý:** Có những ngoại lệ cho hai kịch bản này và các triển khai có thể phức tạp. Nếu bạn là Provider và cảm thấy không phù hợp hoàn toàn với kịch bản nào, hãy liên hệ account manager của bạn, mở support case, hoặc nói chuyện với chuyên gia trước khi triển khai bất cứ điều gì.

---

## Các mô hình kiến trúc để triển khai SMS

Hãy khám phá các mô hình kiến trúc khác nhau để xây dựng dịch vụ SMS dựa trên nhu cầu kinh doanh và mối quan hệ của bạn với Customers. Mỗi mô hình có các đặc điểm riêng:

---

### 1. Mô hình "Bring Your Own AWS Account"

**Ai thực hiện đăng ký và cấu hình?**

Customer kết nối AWS account của riêng họ, vì vậy đăng ký và cấu hình xảy ra trong account của Customer. Thông tin được sử dụng trong đăng ký trong kịch bản này thường là của Customer, vì đó là account của họ.

**Trách nhiệm của Customer:**

* Thực hiện tất cả các bước đăng ký và cấu hình bắt buộc.
* Tích hợp account của họ với dịch vụ của Provider.
* Quản lý message sending, opt-out lists, v.v.
* Thanh toán hóa đơn AWS.

**Trách nhiệm của Provider:**

* Cung cấp giao diện thân thiện với người dùng gọi AWS End User Messaging Service APIs sử dụng credentials của Customer.
* Mức độ dịch vụ Provider cung cấp có thể thay đổi tùy theo nhu cầu.

**Phù hợp nhất cho:**

Customers kỹ thuật muốn kiểm soát hoàn toàn và đã quen thuộc với AWS; Providers muốn tránh sự phức tạp của đăng ký và cấu hình.

---

### 2. Mô hình "Provider Account – Manual Registration and Configuration"

**Ai thực hiện đăng ký và cấu hình?**

Provider sở hữu AWS account và không cho Customers cách nhập thông tin của riêng họ, vì vậy Provider nhập thay mặt họ.

* Thông tin Customer được thu thập thủ công.
* Provider xử lý sự phức tạp đăng ký và cấu hình qua console.

**Trách nhiệm của Customer:**

* Cung cấp thông tin cần thiết cho Provider để đăng ký.

**Trách nhiệm của Provider:**

* Thu thập thông tin đăng ký từ Customers thủ công.
* Quản lý sự phức tạp thay mặt Customer.

Mô hình này có thể được triển khai theo hai cách: sử dụng AWS account riêng cho mỗi Customer, hoặc sử dụng kiến trúc multi-tenant trong một account duy nhất.

**Phù hợp nhất cho:**

Providers với số lượng nhỏ Customers có giá trị cao cần hỗ trợ hands-on trong suốt quá trình SMS onboarding.

---

### 3. Mô hình "Semi-Automated – Customer Submits"

**Ai thực hiện đăng ký và cấu hình?**

Provider xây dựng cơ chế cho Customers submit thông tin đăng ký, và sau đó programmatically chuyển tiếp dữ liệu này đến carriers/regulators.

**Trách nhiệm của Customer:**

* Nền tảng của bạn quản lý cấu hình kỹ thuật và khả năng gửi, nhưng Customers vẫn chịu trách nhiệm về compliance.

**Trách nhiệm của Provider:**

* Cung cấp các cách thuận tiện cho Customers submit thông tin (webhooks, forms, APIs).
* Tự động gửi dữ liệu đăng ký đến regulators.
* Quản lý cấu hình kỹ thuật và khả năng gửi.

**Phù hợp nhất cho:**

Providers với khả năng kỹ thuật vừa phải muốn giảm ma sát trong khi duy trì sự tách biệt rõ ràng về trách nhiệm pháp lý.

---

### 4. Mô hình "Fully Automated – Provider Sends"

**Ai thực hiện đăng ký và cấu hình?**

Thông tin Customer được sử dụng trong đăng ký, nhưng Provider tự động hóa hoàn toàn quy trình đăng ký.

**Trách nhiệm của Customer:**

* Chỉ tập trung vào message-level compliance; tất cả các khía cạnh kỹ thuật được Provider xử lý.

**Trách nhiệm của Provider:**

* Cung cấp terms of service và privacy policies sẵn sàng sử dụng, có thể tùy chỉnh.
* Cung cấp các kênh opt-in tuân thủ (web forms, phone scripts, v.v.).
* Xử lý tất cả các khía cạnh kỹ thuật của đăng ký.

**Phù hợp nhất cho:**

Providers lớn hơn phục vụ nhiều Customers với các mức độ kỹ thuật khác nhau.

---

### 5. Mô hình "Fully Automated Messaging Constrained by Templates"

**Ai thực hiện đăng ký và cấu hình?**

Thông tin Customer được sử dụng cho đăng ký, nhưng Provider xử lý tự động.

**Trách nhiệm của Customer:**

* Centralized compliance: họ chỉ được phép cá nhân hóa các trường trong message templates đã được phê duyệt trước.

**Trách nhiệm của Provider:**

* Cung cấp bộ message templates đã được phê duyệt trước.
* Quản lý compliance tập trung.
* Đơn giản hóa đăng ký vì nội dung được kiểm soát chặt chẽ.

**Phù hợp nhất cho:**

Các kịch bản messaging có thể dự đoán như appointment reminders, delivery notifications, hoặc OTP messages.

---

### 6. Mô hình "Fully Managed Program"

**Ai thực hiện đăng ký và cấu hình?**

Customers ủy quyền cho Provider gửi tin nhắn thay mặt họ, có nghĩa là Provider sở hữu mối quan hệ với End User.

**Trách nhiệm của Customer:**

* Chỉ cung cấp thông tin cần thiết để cá nhân hóa tin nhắn (ví dụ, tracking number).

**Trách nhiệm của Provider:**

* Quản lý toàn bộ mối quan hệ với End Users.
* Kiểm soát toàn bộ messaging experience, bao gồm thu thập opt-ins.

**Ví dụ:** Một dịch vụ delivery notification có thể gửi:

> "ShipTrack: Đơn hàng của bạn từ ACME Corp sẽ đến vào ngày mai. Theo dõi tại [link]."

**Phù hợp nhất cho:**

Các kịch bản chuyên biệt nơi nền tảng của bạn mang lại giá trị đáng kể như một intermediary được xác định rõ ràng.

---

## Định hình dịch vụ SMS của bạn: Các yếu tố chiến lược cần xem xét

### Chiến lược định giá

Khi tích hợp SMS vào sản phẩm của bạn, một trong những yếu tố đầu tiên cần xem xét là cách cấu trúc pricing. Không giống như nhiều dịch vụ digital với chi phí có thể dự đoán, SMS pricing thay đổi đáng kể theo destination country, originator type, và message volume.

AWS End User Messaging Service tính phí dựa trên volume tin nhắn gửi đến mỗi quốc gia, với rate khác nhau cho mỗi quốc gia. Pricing được xác định bởi country code của thiết bị người nhận, không phải vị trí vật lý của họ.

Khi thiết kế pricing model, hãy xem xét các phương pháp volume-based phổ biến này:

* **SMS Credits:** Tạo hệ thống credit tiêu chuẩn hóa nơi Customers mua credits bất kể destination country.
* **Dollar-Based Allocation:** Cung cấp cho Customers ngân sách tiền tệ được tiêu thụ dựa trên chi phí thực tế của mỗi tin nhắn.
* **Country-Tiered Pricing:** Nhóm các quốc gia thành tiers và tính giá khác nhau cho mỗi tier.
* **Bundled Messaging:** Bao gồm một số lượng tin nhắn nhất định trong gói cơ bản, với phí bổ sung cho overage.

---

### Các cân nhắc về địa lý

Các quốc gia khác nhau có các yêu cầu quy định khác nhau cho SMS messaging, bao gồm:

* **Originator support:** Không phải tất cả các quốc gia đều hỗ trợ tất cả originator types.
* **Originator selection:** Khi nhiều originator types được hỗ trợ, bạn sẽ giúp Customers chọn đúng loại cho mỗi use case như thế nào?
* **Registration:** Ngày càng nhiều quốc gia yêu cầu đăng ký trước khi bạn được phép gửi.
* **Quiet hours:** Nhiều quốc gia hạn chế thời gian promotional messages có thể được gửi.
* **Content restrictions:** Một số loại nội dung (gambling, alcohol, adult content, v.v.) có thể bị cấm hoặc được quy định chặt chẽ.
* **Template requirements:** Một số jurisdictions yêu cầu phê duyệt trước message templates.
* **Sender ID regulations:** Các quy tắc về ai có thể sử dụng alphanumeric Sender IDs khác nhau rộng rãi.

---

## Các chiến lược để giảm ma sát trong quá trình triển khai

Triển khai SMS có thể phức tạp cho Customers của bạn. Dưới đây là một số chiến lược có thể đơn giản hóa và/hoặc hợp lý hóa quy trình:

### Provider-Hosted Privacy Policy và/hoặc Terms & Conditions

Tạo các templates tuân thủ, có thể tùy chỉnh cho privacy policies và terms & conditions mà Customers của bạn có thể sử dụng.

### Online Registration Forms và Flows

Phát triển các online forms thân thiện với người dùng thu thập tất cả thông tin đăng ký bắt buộc trong một quy trình có hướng dẫn.

### Pre-Approved Opt-In Widgets

Tạo các widgets có thể nhúng mà Customers của bạn có thể thêm vào websites hoặc apps của họ để triển khai opt-in flows tuân thủ.

### Template Library

Cung cấp thư viện message templates đã được phê duyệt trước cho các use cases phổ biến.

### Test Environments

Tạo sandbox environments nơi Customers có thể test các triển khai SMS của họ trước khi go live.

### Documentation và Training

Phát triển documentation và training resources rõ ràng cụ thể cho mỗi originator type và use case.

---

## Kết luận

Tích hợp khả năng SMS vào nền tảng của bạn có thể tăng đáng kể engagement với Customers của bạn, nhưng hành trình có thể phức tạp. Hướng dẫn này đã khám phá các yếu tố chính sẽ giúp bạn điều hướng thành công.

Chúng tôi đã xem xét nhiều mô hình kiến trúc, mỗi mô hình có các trade-offs riêng giữa trách nhiệm của Customer và Provider. Chúng tôi cũng đã xem xét các yếu tố chiến lược như pricing, geographic regulations, và originator types cần được xem xét cẩn thận. Cuối cùng, chúng tôi đã thảo luận các chiến lược thực tế để giảm ma sát cho Customers của bạn—như hosted compliance materials, streamlined registration flows, và pre-approved templates—để đơn giản hóa tích hợp.

Bước đầu tiên quan trọng nhất là hiểu rõ mối quan hệ giữa bạn (với tư cách là Provider), Customers của bạn, và End Users của họ. Điều này xác định thông tin của ai được sử dụng cho originator registration và, đến lượt nó, định hình SMS experience.

Cuối cùng, một giải pháp SMS thành công đòi hỏi cân bằng các cân nhắc kỹ thuật, yêu cầu quy định, và tập trung vào khách hàng. Bằng cách tận dụng hướng dẫn này, bạn có thể thiết kế và triển khai một dịch vụ mang lại trải nghiệm tốt nhất có thể cho cả Customers và End Users của họ.

---

### Về tác giả

**Tyler Holmes** là Senior Specialist Solutions Architect. Anh có kinh nghiệm sâu rộng trong communications với tư cách là consultant, solutions architect, practitioner, và leader ở mọi cấp độ từ startups đến các công ty Fortune 500. Anh có hơn 14 năm kinh nghiệm trong sales, marketing, và service operations, làm việc cho vendors, consultancies, và brands, xây dựng teams và tăng trưởng doanh thu.
