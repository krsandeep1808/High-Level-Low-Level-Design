## 🧠 High-Level Design (HLD)

### 🎯 **Objective**

Enable users to upload documents (e.g., PDF), and ask natural language questions that are answered based on the document content using NLP.

---

### 📐 **Architecture Diagram**

```
              ┌───────────────────┐
              │    Frontend UI    │
              │ (HTML + JS + CSS) │
              └────────▲──────────┘
                       │ REST API Calls
                       ▼
              ┌───────────────────┐
              │     FastAPI       │
              │  API Endpoints    │
              └────┬────────▲─────┘
                   │        │
         ┌─────────┘        └────────────┐
         ▼                               ▼
┌──────────────────┐           ┌───────────────────────┐
│ PDF Processor    │           │ NLP Processor         │
│ (PyMuPDF)        │           │ (spaCy / custom logic)│
└──────────────────┘           └───────────────────────┘
         ▼                               ▼
         └─────▶ Vectorized Content (e.g., text chunks)
                          │
                          ▼
                 ┌────────────────┐
                 │ RAG Agent / QA │
                 │  Logic Engine  │
                 └────────────────┘
                          │
                          ▼
                Return answer to frontend
```

---

### ✅ **Components Involved**

1. **Frontend**: Static HTML + JS + TailwindCSS UI to upload PDFs and submit queries.
2. **Backend (FastAPI)**:

   * **Upload API** → stores and parses documents.
   * **Query API** → sends query to NLP processor and returns answer.
3. **PDF Processor**: Extracts clean text content from PDF using `PyMuPDF`.
4. **NLP Processor**: Tokenizes, vectorizes, and processes text using `spaCy`.
5. **Query Answering Logic (RAG)**:

   * Embeds document content.
   * Matches user query with relevant content chunk.
   * Returns the most appropriate snippet or response.

---

## 🔍 Low-Level Design (LLD)

### 📁 Backend Modules (FastAPI)

#### `run.py`

* Entry point for starting the FastAPI server.

#### `app/api/endpoints.py`

* Defines API routes: `/upload`, `/query`.

#### `app/services/pdf_processor.py`

* Uses PyMuPDF (`fitz`) to extract readable text from uploaded PDF files.

#### `app/services/nlp_processor.py`

* Loads and uses spaCy NLP pipeline.
* Tokenizes document content.
* Searches for best match using simple scoring logic (can be extended to vector search).

#### `app/db/models.py` *(optional persistence)*

* Currently stubbed or basic — could store parsed documents, user queries, etc.

---

### 🔁 Data Flow (End-to-End)

1. User uploads a PDF.
2. PDF is parsed → clean text extracted.
3. Document text stored in memory (or optional DB).
4. User enters a query.
5. NLP engine checks which part of the document best answers the query.
6. Matched content is returned to user as the answer.

---

### 🔒 Security & Optimization Notes

* CORS enabled for frontend-backend communication.
* No document storage or authentication yet (can be added).
* Potential optimizations:

  * Use a vector database (e.g., FAISS, Pinecone) for semantic search.
  * Stream answers if using an LLM (e.g., OpenAI API).

---

Would you like this HLD/LLD included in your documentation file as a separate section or exported as a PDF?
