# RAG Document Q&A with Groq

A local Retrieval-Augmented Generation (RAG) application that lets you chat with your PDF documents using Groq LLM inference. Ask questions in plain English and get accurate, context-grounded answers.

---

## How It Works

```
PDF Files -> Text Chunks -> HuggingFace Embeddings -> FAISS Vector DB
                                                            |
User Question -> Similarity Search -> Relevant Chunks -> Groq LLM -> Answer
```

1. **PDF Loading** - Reads all PDFs from the `research_papers/` folder
2. **Chunking** - Splits text into 1000-character chunks (200 overlap)
3. **Embedding** - Converts chunks into vectors using `all-MiniLM-L6-v2` (runs locally, free)
4. **Vector Store** - Stores vectors in a FAISS in-memory database
5. **Q&A** - Your question is matched against the vector store; top chunks + question are sent to Groq LLM

---

## Tech Stack

| Component     | Technology                              |
|---------------|-----------------------------------------|
| Frontend      | Streamlit                               |
| LLM           | Groq API (qwen/qwen3.8-27b)             |
| Embeddings    | HuggingFace all-MiniLM-L6-v2 (local)   |
| Vector Store  | FAISS (in-memory, no account needed)    |
| PDF Loader    | LangChain PyPDFDirectoryLoader          |
| Orchestration | LangChain                               |

---

## Setup

### 1. Place your PDFs
Put your PDF files inside the `research_papers/` folder.

### 2. Install dependencies

```bash
pip install streamlit langchain langchain-groq langchain-huggingface langchain-community langchain-classic langchain-text-splitters langchain-core faiss-cpu pypdf python-dotenv sentence-transformers
```

### 3. Configure API Keys

Create a `.env` file in the project root:

```env
GROQ_API_KEY=your_groq_api_key_here
```

Get your free Groq API key at https://console.groq.com

---

## Running the App

```bash
streamlit run app_huggingfaceembedding.py
```

Then open http://localhost:8501 in your browser.

---

## Usage

1. Click **"Document Embedding"** - processes your PDFs and builds the vector database
2. Wait for the **"Vector Database is ready!"** message
3. Type your question in the text box and press Enter
4. Expand **"Document Similarity Search"** to see which source chunks were used

---

## Project Structure

```
4-RAG Document Q&A/
|-- app_huggingfaceembedding.py   # Main app (uses local HuggingFace embeddings)
|-- main.py                       # Alternative app (requires OpenAI API key)
|-- research_papers/              # Put your PDF files here
|-- .env                          # API keys (do not commit to git)
|-- README.md                     # This file
```

---

## Available Groq Models

Update `model_name` in `app_huggingfaceembedding.py` to switch models:

| Model ID             | Notes                    |
|----------------------|--------------------------|
| qwen/qwen3.8-27b     | Current default          |
| openai/gpt-oss-20b   | Alternative              |
| openai/gpt-oss-120b  | Larger, more powerful    |

---

## Notes

- The vector database is in-memory only - it resets when you restart the app.
- Only the first 50 document chunks are used by default.
- An OpenAI API key is NOT required when using app_huggingfaceembedding.py.
