---
title: "Event 1"
date: 2025-01-01
weight: 1
chapter: false
pre: " <b> 4.1. </b> "
---
# Summary Report: "AI/ML, GenAI & Amazon Bedrock on AWS"

### Event Objectives

- Understand the AI/ML/GenAI ecosystem on AWS
- Learn how Bedrock, RAG, Agent Core, and Browser Tool work
- Apply Prompt Engineering techniques to interact effectively with LLMs
- Explore AI services such as Rekognition, Textract, Transcribe, Translate
- Connect GenAI capabilities to modern application development

### Speakers

- **Lam Tuan Kiet** – *Generative AI & Prompt Engineering Techniques*
- **Dinh Le Hoang Anh** – *AI/ML Services & AWS AI Stack*
- **Danh Hoang Hieu Nghi** – *Amazon Bedrock Agent Core & Browser Tool*

---

# Key Highlights

## Overview of AI/ML/GenAI on AWS

The event provided a deep dive into the modern AI stack on AWS — from traditional machine learning to **Foundation Models**, combined with tools such as **Titan Embeddings**, **RAG**, **Bedrock Agents**, and **Prompt Engineering**.

Key topics covered:

- The role of embeddings in semantic search and RAG
- How Bedrock Agents perform planning and tool usage
- The RAG pipeline and real-world use cases
- Supporting AI services for vision, speech, and document processing

---

## Embeddings & RAG

### Embeddings

- Represent text/images as numerical vectors
- Used for **search**, **similarity**, **clustering**, **face recognition**, and **RAG**
- Amazon **Titan Text Embedding** provides 1536–3072-dimensional embeddings optimized for semantic retrieval

### RAG in Action

1. User submits a query
2. Query is converted into an embedding
3. Vector DB is searched (FAISS, OpenSearch)
4. Relevant context is retrieved
5. LLM generates an answer based on the retrieved context

RAG improves **accuracy**, reduces **hallucination**, and integrates enterprise-specific data.

---

## Prompt Engineering Techniques

### Techniques introduced:

- **Zero-shot prompting** – reasoning without examples
- **Few-shot prompting** – guiding the model with examples
- **Chain of Thought** – enforcing step-by-step reasoning
- **RAG Prompting** – augmenting LLM with retrieved context
- **Instruction-style prompting** – optimizing for task compliance

Goal: **make AI produce exactly what you intend**.

---

## Amazon Bedrock Agents

### Agent Core

- The intelligent orchestration layer of Bedrock Agents
- Performs planning, reasoning, and task interpretation
- Automatically decides which tools to use: RAG, API calls, Code Interpreter, Browser Tool…
- Executes multi-step workflows with minimal backend logic
- Supports Memory, Runtime, and Observability

### Browser Tool

- Allows agents to browse the web and access real-time information
- Fetch URLs, perform web searches, extract website text
- Ideal for news, stock prices, public documents, online articles

Key distinction:

- **Browser Tool = external online data**
- **RAG = internal enterprise knowledge**

---

# AWS AI Services Overview

The event also clarified the landscape of AWS AI services, which are **outside Bedrock**, but integrate extremely well with GenAI:

### Rekognition

- Detect/compare faces
- Object detection
- Content moderation
- Used for security, FaceID, and filtering sensitive content

### Translate

- Real-time neural machine translation
- Supports domain customization and S3 file translation

### Textract

- Enterprise-grade OCR
- Extract structured forms, tables
- Parse ID documents for KYC workflows

### Transcribe

- Speech-to-text
- Speaker diarization
- Real-time transcription

### Polly

- Text-to-speech with natural neural voices

### Comprehend

- Classical NLP: sentiment, entity extraction, PII detection

### Kendra

- Enterprise semantic search
- FAQ matching & document ranking

### Personalize

- Real-time recommendation system used by Amazon.com

### Lookout Family

- Anomaly detection across vision, equipment telemetry, and business metrics

---

# Key Takeaways

## AI/GenAI Mindset

- Always start with the **business problem**, not the technology
- GenAI complements traditional ML — it doesn’t replace it
- Embeddings + RAG is the foundation for enterprise GenAI solutions
- Better prompts = better outcomes

## Technical Architecture

- Bedrock Agents enable **automated AI workflows** without heavy backend code
- Browser Tool extends an agent’s knowledge with real-time web information
- Vector DB sits at the core of retrieval-powered AI
- AWS AI services (Rekognition, Textract…) enhance GenAI-based systems

## Modernization & Real Applications

Examples highlighted:

- Textract → RAG → Bedrock → Document chatbot
- Transcribe → Summary → Insight extraction
- Rekognition → Vision analysis with reasoning via Bedrock
- Personalize → Recommendation + GenAI explanations

---

# Applying to Work

- Build an internal knowledge assistant using RAG + Titan Embeddings
- Integrate **Textract** into automation workflows for document processing
- Use **Bedrock Agent Core + Browser Tool** to power multi-step agents
- Adopt prompt engineering guidelines across teams
- Explore Rekognition/Textract/Transcribe for operational automation

---

# Event Experience

Participating in the **“AI/ML & GenAI on AWS”** workshop gave me a comprehensive understanding of how AWS builds an open, flexible, and modern AI platform.

### Learning from Experts

- Speakers clearly explained how to apply GenAI to real business scenarios
- Gained clarity on the differences between traditional ML, Foundation Models, and GenAI
- Understood the future potential of Bedrock Agents

### Hands-On Knowledge

- Practiced RAG pipelines and embedding generation
- Explored vector databases and semantic retrieval
- Tested multiple prompt engineering techniques

### Tools & Productivity

- Saw how Amazon Q Developer accelerates the entire SDLC
- Learned how various AI services fit into modern workflows

### Lessons Learned

- GenAI is not “one model” — it is a **full system architecture**
- Success requires data → embedding → RAG → agent → workflow design
- Collaboration between business and technical teams is essential

### Event Photos

<p align="center">
  <img src="../../images/event1.png" width="450" />
</p>
