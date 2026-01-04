# Intelligent Complaint Analysis for Financial Services

This project focuses on developing an internal AI-powered system for CrediTrust Financial, a rapidly expanding digital finance company. The core solution is a Retrieval-Augmented Generation (RAG)–based chatbot designed to convert large volumes of unstructured customer complaint data into clear, actionable insights.

By allowing internal stakeholders—such as Product Managers, Customer Support teams, and Compliance officers—to ask plain-English questions and receive summarized, evidence-backed responses within seconds, the tool helps shift the organization from a reactive problem-solving approach to a proactive, insight-driven strategy.

## Business Context & Problem

CrediTrust Financial serves more than 500,000 users across East African markets through a mobile-first platform. Its services include Credit Cards, Personal Loans, Buy Now Pay Later (BNPL), Savings Accounts, and Money Transfers. Every month, the company receives thousands of customer complaints related to these products.

**Problem**
Internal teams are overwhelmed by the growing volume of complaint data. Product Managers, in particular, spend excessive time manually reviewing complaints to identify recurring issues and emerging trends, making it difficult to respond quickly and strategically.

### Key Performance Indicators (KPIs) for Success

- Decrease the time for a Product Manager to identify a major complaint trend from days to minutes.
- Empower non-technical teams (Support, Compliance) to get answers without needing a data analyst.
- Shift the company from reacting to problems to proactively identifying and fixing them based on real-time customer feedback.

## Motivation

CrediTrust’s internal teams face serious bottlenecks:

- Customer Support is overwhelmed by incoming complaint volume.
- Product Managers struggle to identify frequent or critical issues across products.
- Compliance & Risk teams are reactive rather than proactive.
- Executives lack visibility into emerging pain points due to scattered and hard-to-read complaint narratives.

The RAG agent is designed to:

- Allow internal users to ask plain-English questions (e.g., “Why are people unhappy with BNPL?”).
- Use semantic search (via a vector database) to retrieve the most relevant complaint narratives.
- Feed the retrieved narratives into a Language Model (LLM) that generates concise, insightful answers.
- Support multi-product querying, enabling filtering or comparison of issues across financial services.

## Data

This challenge uses complaint data from the Consumer Financial Protection Bureau (CFPB). The dataset contains real customer complaints across multiple financial products.

### Key Fields in Each Record

- **Consumer complaint narrative**: The core input for embedding and retrieval.
- **Product**: Financial product category.
- **Issue**: Specific issue reported by the consumer.
- **Sub-product**, **Company**, **Date received**, **Complaint ID**, and other metadata.

## Learning Outcomes

By completing this project, the following skills have been gained:

- Combining vector similarity search with language models to answer user questions based on unstructured data.
- Handling noisy, unstructured consumer complaint narratives and extracting meaningful insights.
- Creating and querying a vector database (ChromaDB) using embedding models to power semantic search.
- Developing a chatbot that uses real retrieved documents as context for generating intelligent, grounded answers using LLMs.
- Creating a system that can analyze and respond across multiple financial product categories.
- Building and testing a simple, interactive user interface.

## Project Structure

```
intelligent-complaint-answering-chatbot/
├── .git/
├── .github/
│   └── workflows/
│       └── ci.yml
├── data/
│   ├── processed/
│   │   └── filtered_complaints.csv
│   └── raw/
│       └── complaint.csv
├── config/
│   └── settings.yaml
├── models/
│   ├── sentence-transformers_all-MiniLM-L6-v2/
│   └── google_gemma-2b-it/
├── notebooks/
│   ├── 01_eda_preprocessing.ipynb
│   └── 02_rag_prototyping.ipynb
├── reports/
│   ├── interim_report.md
│   └── final_report.md
├── src/
│   ├── __init__.py
│   ├── data_processor.py
│   ├── vector_db_manager.py
│   ├── rag_pipeline.py
│   ├── app.py
│   └── utils/
│       ├── __init__.py
│       └── text_cleaners.py
├── tests/
│   ├── test_data_processor.py
│   ├── test_vector_db_manager.py
│   └── test_rag_pipeline.py
├── vector_store/
│   └── chroma.sqlite3
├── .gitignore
├── README.md
└── requirements.txt
```

## Setup and Installation

### Prerequisites

- Python 3.8+
- Git
- Git Large File Storage (Git LFS)

### Clone the Repository

```bash
clone the repo
cd RAG_hatbot
```

### Install Git LFS

```bash
git lfs install
git lfs track "vector_store/*.sqlite3"
git add .gitattributes
git commit -m "Configure Git LFS tracking for vector_store/*.sqlite3"
```
### Create and Activate Virtual Environment

```bash
python3 -m venv .venv
source .venv/bin/activate 
```

### Install Dependencies

```bash
pip install -r requirements.txt
```

## Project Progress & Deliverables

### Task 1: EDA & Preprocessing

- Loaded raw dataset with 9.6M rows.
- Removed ~69% of rows with missing complaint narratives.
- Filtered for 5 key financial products.
- Cleaned and saved to `data/processed/filtered_complaints.csv`.

### Task 2: Chunking, Embedding & Indexing

- Used `langchain.text_splitter.RecursiveCharacterTextSplitter`.
- `chunk_size = 1000`, `chunk_overlap = 200`
- Embedded with `sentence-transformers/all-MiniLM-L6-v2`.
- Indexed using ChromaDB.
- Saved to `vector_store/chroma.sqlite3`.

### Task 3: RAG Pipeline & Evaluation

- Retrieves top-5 relevant chunks using semantic similarity.
- Uses a structured prompt for context-grounded answers.
- Generates responses using `google/gemma-2b-it`.
- Evaluates outputs on 5–10 test queries.

### Task 4: Chat Interface

- Built using Gradio.
- Features:
  - Query input and answer output
  - Displays source chunks
  - Streams LLM response token by token
  - “Clear Chat” button

---

## How to Run

### Local PC

```bash
source .venv/bin/activate
python3 src/vector_db_manager.py
python3 src/app.py
```

Then visit the local URL shown in the terminal (e.g., `http://127.0.0.1:7860/`).

### Google Colab

1. Install dependencies:

```python
!pip install pandas langchain langchain-community sentence-transformers chromadb gradio transformers accelerate gdown
```
2. Paste and run Colab-modified versions of:
   - `src/vector_db_manager.py`
   - `src/rag_pipeline.py`
   - `src/app.py`

3. Click the Gradio public link to access the UI.

---

## Challenges Encountered & Solutions

### GitHub File Size Limit

- Used Git LFS to handle large vector store files.

### Git History Divergence

- Resolved with:

```bash
git pull origin main --allow-unrelated-histories
```