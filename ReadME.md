Chronicle-AI-RAG: Retrieval Augmented News Intelligence System

This is an end to end Retrieval Augmented Generation (RAG) system that automates content ingestion from RSS feeds, chunks and indexes data using semantic vector embeddings, and generates context grounded answers through Google's Gemini models. This project is a combination of web scraping, information retrieval, and LLM synthesis into a single workflow.

### Skills & Technologies Demonstrated

* **RAG Architecture:** Context-grounded synthesis using `gemini-2.5-flash`.


* **Vector Storage:** Dense vector retrieval using `FAISS` (L2 Flat Index).


* **Text Engineering:** Semantic chunking via LangChain’s `RecursiveCharacterTextSplitter`.


* **Embeddings Pipeline:** Vector generation via `sentence-transformers/all-MiniLM-L6-v2`.


* **Data Automation:** Automated ingestion via `feedparser` and headless `Playwright` scraping.


* **Context Compression:** Local token reduction using a Hugging Face `t5-small` pipeline.

---

## System Architecture

RSS Feeds → Article Extraction (feedparser + Playwright) → Chunking & Embeddings (all-MiniLM-L6-v2) → FAISS Vector Index → Semantic Retrieval (Top-K) → T5 Context Compression → Gemini Response Generation

---

## Technical Design & Architecture

### Cost-Conscious Context Compression (`main.py`)

To minimize external API token consumption, the system retrieves the top 5 relevant document chunks (`K=5`) and passes them through a local, hardware-efficient Hugging Face `t5-small` summarizer. This compresses raw text into compact factual summaries before sending the payload to Gemini, optimizing token budgets while maintaining strict source grounding.

### Core Component Breakdown

* **Dynamic Ingestion (`ingest.py`):** Uses headless Chromium via `Playwright` to extract content from raw `<article>` HTML tags, bypassing simple RSS abstract limits.


* **Text Processing (`process.py`):** Implements a `RecursiveCharacterTextSplitter` with a strict 600 character window and 100 character overlap to retain semantic boundaries.


* **Persistence & State Management:** Saves vector spaces (`data/vector.index`) and cross-referenced metadata JSON files directly to disk to preserve session tracking and application state.


---

## Setup & Execution

### Installation

Ensure Python 3.11+ is installed, then pull dependencies and the automated browser engine:

```bash
git clone https://github.com/Celsius273-web/Chronicle-AI-RAG
cd Chronicle-AI-RAG
pip install -r requirements_2.txt
playwright install chromium

```

### Configuration Details

1. Include the RSS streams inside `data/config.yaml`.

2. Create a local `.env` file containing your Gemini API access credentials:

MY_KEY=your_gemini_api_key_here

3. Start the program with: python main.py

* **`n`**: Fetches new data from RSS streams, rebuilds chunks, and updates the FAISS index.

* **`p`**: Opens the natural language prompt terminal loop to query your localized vector space.
