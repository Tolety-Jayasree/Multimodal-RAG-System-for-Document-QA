# 🚀 Multimodal RAG System for Document Q&A

## 📌 Overview
This project is a **Multimodal Retrieval-Augmented Generation (RAG) system** that enables question answering over **PDF documents containing both text and images**.

It uses a unified embedding space (CLIP) to support **cross-modal search**, allowing users to query:
- 📄 Text content
- 🖼️ Images inside PDFs

The system retrieves relevant multimodal context using FAISS and generates responses using **GPT-4 (vision-capable model)**.

---

## ⚙️ Architecture

### 1️⃣ PDF Parsing (PyMuPDF)
- Loads PDF using `fitz (PyMuPDF)`
- Extracts:
  - Text per page
  - Embedded images

---

### 2️⃣ Text Processing
- Text is cleaned and split using:
  - `RecursiveCharacterTextSplitter`
- Each chunk is stored as a LangChain `Document`

---

### 3️⃣ Image Processing
For each extracted image:
- Converted to PIL format
- Encoded to **base64 (for GPT-4V input)**
- Embedded using **CLIP vision encoder**
- Stored with metadata (`page`, `image_id`)

---

### 4️⃣ Multimodal Embeddings (CLIP)
A unified embedding model is used:

- 📄 Text → CLIP text encoder  
- 🖼️ Images → CLIP vision encoder  
- 🔗 Both mapped into same vector space (512-dim)

---

### 5️⃣ Vector Store (FAISS)
- All embeddings (text + images) are stored in a **single FAISS index**
- Enables **cross-modal similarity search**

---

### 6️⃣ Retrieval
- User query is embedded using CLIP text encoder
- FAISS retrieves top-k relevant:
  - Text chunks
  - Images

---

### 7️⃣ Multimodal Prompt Construction
Retrieved results are formatted into a GPT-4 message:
- Text context is included as structured excerpts
- Images are passed as **base64 image inputs**
- Query + context are combined into a single prompt

---

### 8️⃣ Answer Generation (GPT-4 Vision)
- `gpt-4.1` is used via LangChain
- Model reasons over:
  - Text + images together
- Produces final grounded response

---

## ✨ Key Features

- 📄 Multi-PDF document support  
- 🖼️ Image + text extraction from PDFs  
- 🧠 Unified CLIP embeddings (multimodal space)  
- 🔍 Cross-modal semantic search using FAISS  
- 🤖 GPT-4 Vision-based reasoning  
- ⚡ End-to-end RAG pipeline  

---

## 🧠 Tech Stack

- Python  
- PyMuPDF (fitz)  
- OpenAI CLIP (`clip-vit-base-patch32`)  
- FAISS (vector search)  
- LangChain  
- GPT-4.1 (Vision-capable LLM)  
- PyTorch  
- PIL (image processing)  
- scikit-learn  

---

## 📂 Project Workflow
PDF → Text + Images
↓
CLIP Embeddings
↓
FAISS Vector Store
↓
User Query → CLIP Embedding
↓
Similarity Search (Text + Images)
↓
Multimodal Prompt Construction
↓
GPT-4 Vision Response

---

## ▶️ How to Run

### 1. Install dependencies
```bash
pip install -r requirements.txt
2. Add API key
Create a .env file:
OPENAI_API_KEY=your_secret_api_key
3. Run the application
python app.py

Built by Jaya Sree Tolety
AI/ML Engineer | LLMs | RAG Systems | Agentic AI
