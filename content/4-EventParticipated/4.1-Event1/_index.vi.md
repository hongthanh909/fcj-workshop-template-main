---
title: "Sự kiện 1"
date: 2025-01-01
weight: 1
chapter: false
pre: " <b> 4.1. </b> "
---
# Báo cáo tổng kết: "AI/ML, GenAI & Amazon Bedrock trên AWS"

### Mục tiêu sự kiện

- Hiểu về hệ sinh thái AI/ML/GenAI trên AWS
- Tìm hiểu cách Bedrock, RAG, Agent Core, và Browser Tool hoạt động
- Áp dụng các kỹ thuật Prompt Engineering để tương tác hiệu quả với LLMs
- Khám phá các dịch vụ AI như Rekognition, Textract, Transcribe, Translate
- Kết nối khả năng GenAI với phát triển ứng dụng hiện đại

### Diễn giả

- **Lam Tuan Kiet** – *Generative AI & Prompt Engineering Techniques*
- **Dinh Le Hoang Anh** – *AI/ML Services & AWS AI Stack*
- **Danh Hoang Hieu Nghi** – *Amazon Bedrock Agent Core & Browser Tool*

---

# Nội dung nổi bật

## Tổng quan về AI/ML/GenAI trên AWS

Sự kiện cung cấp cái nhìn sâu về AI stack hiện đại trên AWS — từ machine learning truyền thống đến **Foundation Models**, kết hợp với các công cụ như **Titan Embeddings**, **RAG**, **Bedrock Agents**, và **Prompt Engineering**.

Các chủ đề chính được đề cập:

- Vai trò của embeddings trong semantic search và RAG
- Cách Bedrock Agents thực hiện planning và tool usage
- RAG pipeline và các use cases thực tế
- Các dịch vụ AI hỗ trợ cho vision, speech, và document processing

---

## Embeddings & RAG

### Embeddings

- Biểu diễn text/images dưới dạng numerical vectors
- Được sử dụng cho **search**, **similarity**, **clustering**, **face recognition**, và **RAG**
- Amazon **Titan Text Embedding** cung cấp embeddings 1536–3072 chiều được tối ưu hóa cho semantic retrieval

### RAG trong thực tế

1. Người dùng gửi một query
2. Query được chuyển đổi thành embedding
3. Vector DB được tìm kiếm (FAISS, OpenSearch)
4. Context liên quan được truy xuất
5. LLM tạo câu trả lời dựa trên context được truy xuất

RAG cải thiện **độ chính xác**, giảm **hallucination**, và tích hợp dữ liệu đặc thù của doanh nghiệp.

---

## Kỹ thuật Prompt Engineering

### Các kỹ thuật được giới thiệu:

- **Zero-shot prompting** – suy luận không cần ví dụ
- **Few-shot prompting** – hướng dẫn model với các ví dụ
- **Chain of Thought** – bắt buộc suy luận từng bước
- **RAG Prompting** – tăng cường LLM với context được truy xuất
- **Instruction-style prompting** – tối ưu hóa cho task compliance

Mục tiêu: **làm cho AI tạo ra chính xác những gì bạn muốn**.

---

## Amazon Bedrock Agents

### Agent Core

- Lớp orchestration thông minh của Bedrock Agents
- Thực hiện planning, reasoning, và task interpretation
- Tự động quyết định công cụ nào để sử dụng: RAG, API calls, Code Interpreter, Browser Tool…
- Thực thi multi-step workflows với minimal backend logic
- Hỗ trợ Memory, Runtime, và Observability

### Browser Tool

- Cho phép agents duyệt web và truy cập thông tin thời gian thực
- Fetch URLs, thực hiện web searches, extract website text
- Lý tưởng cho news, stock prices, public documents, online articles

Sự khác biệt chính:

- **Browser Tool = dữ liệu online bên ngoài**
- **RAG = kiến thức doanh nghiệp nội bộ**

---

# Tổng quan các dịch vụ AWS AI

Sự kiện cũng làm rõ bối cảnh các dịch vụ AWS AI, nằm **ngoài Bedrock**, nhưng tích hợp cực kỳ tốt với GenAI:

### Rekognition

- Detect/compare faces
- Object detection
- Content moderation
- Được sử dụng cho security, FaceID, và filtering sensitive content

### Translate

- Real-time neural machine translation
- Hỗ trợ domain customization và S3 file translation

### Textract

- Enterprise-grade OCR
- Extract structured forms, tables
- Parse ID documents cho KYC workflows

### Transcribe

- Speech-to-text
- Speaker diarization
- Real-time transcription

### Polly

- Text-to-speech với natural neural voices

### Comprehend

- Classical NLP: sentiment, entity extraction, PII detection

### Kendra

- Enterprise semantic search
- FAQ matching & document ranking

### Personalize

- Real-time recommendation system được Amazon.com sử dụng

### Lookout Family

- Anomaly detection trên vision, equipment telemetry, và business metrics

---

# Bài học rút ra

## Tư duy AI/GenAI

- Luôn bắt đầu với **business problem**, không phải technology
- GenAI bổ sung cho traditional ML — không thay thế nó
- Embeddings + RAG là nền tảng cho các giải pháp GenAI doanh nghiệp
- Prompts tốt hơn = kết quả tốt hơn

## Kiến trúc kỹ thuật

- Bedrock Agents cho phép **automated AI workflows** mà không cần heavy backend code
- Browser Tool mở rộng kiến thức của agent với thông tin web thời gian thực
- Vector DB nằm ở core của retrieval-powered AI
- Các dịch vụ AWS AI (Rekognition, Textract…) nâng cao các hệ thống dựa trên GenAI

## Hiện đại hóa & Ứng dụng thực tế

Các ví dụ được highlight:

- Textract → RAG → Bedrock → Document chatbot
- Transcribe → Summary → Insight extraction
- Rekognition → Vision analysis với reasoning qua Bedrock
- Personalize → Recommendation + GenAI explanations

---

# Áp dụng vào công việc

- Xây dựng internal knowledge assistant sử dụng RAG + Titan Embeddings
- Tích hợp **Textract** vào automation workflows cho document processing
- Sử dụng **Bedrock Agent Core + Browser Tool** để power multi-step agents
- Áp dụng prompt engineering guidelines trên các teams
- Khám phá Rekognition/Textract/Transcribe cho operational automation

---

# Trải nghiệm sự kiện

Tham gia workshop **"AI/ML & GenAI trên AWS"** đã cho tôi hiểu biết toàn diện về cách AWS xây dựng một nền tảng AI mở, linh hoạt và hiện đại.

### Học hỏi từ các chuyên gia

- Các diễn giả giải thích rõ ràng cách áp dụng GenAI vào các kịch bản kinh doanh thực tế
- Hiểu rõ sự khác biệt giữa traditional ML, Foundation Models, và GenAI
- Hiểu được tiềm năng tương lai của Bedrock Agents

### Kiến thức thực hành

- Thực hành RAG pipelines và embedding generation
- Khám phá vector databases và semantic retrieval
- Test nhiều kỹ thuật prompt engineering

### Công cụ & Năng suất

- Thấy cách Amazon Q Developer tăng tốc toàn bộ SDLC
- Học cách các dịch vụ AI khác nhau phù hợp với modern workflows

### Bài học rút ra

- GenAI không phải "một model" — nó là một **kiến trúc hệ thống hoàn chỉnh**
- Thành công đòi hỏi data → embedding → RAG → agent → workflow design
- Sự hợp tác giữa business và technical teams là thiết yếu

### Hình ảnh sự kiện

<p align="center">
  <img src="../../images/event1.png" width="450" />
</p>
