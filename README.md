
# 📖 Orchestrating a Retriever Pipeline with Azure AI Search

This project demonstrates how to **build and orchestrate a Retriever Pipeline** using **Azure AI Search, Azure Blob Storage, and Azure OpenAI**.  
The notebook walks through **document ingestion, embeddings, indexing, retrieval, and orchestration with an LLM**, along with an **evaluation step**.

---

## 📂 Project Outline

1. **Setting Up Services**
   - Azure Cognitive Search
   - Azure Blob Storage
   - Azure OpenAI

2. **Document Preprocessing**
   - Upload PDFs to Blob Storage
   - Prepare documents for ingestion

3. **Embedding & Indexing**
   - Generate embeddings using Azure OpenAI
   - Upload documents and embeddings to Azure AI Search

4. **Retrieval Pipeline**
   - Create search index
   - Configure **vector search (HNSW)** and **semantic search**
   - Query documents using embeddings

5. **LLM Orchestration**
   - Integrate retriever pipeline with Azure OpenAI Chat model
   - Perform **context-aware Q&A**

6. **Evaluation**
   - Calculate **Relevancy Scores**
   - Validate retrieval accuracy

---

## ⚙️ Setup & Installation

### 🔑 Prerequisites
- Python **3.9+**
- Active **Azure Subscription**
- Provisioned Azure services:
  - **Azure Cognitive Search**
  - **Azure Blob Storage**
  - **Azure OpenAI**

### 📦 Install Dependencies
```bash
pip install azure-search-documents azure-identity azure-core azure-storage-blob python-dotenv
```

### 🔧 Configure Environment Variables
Create a `.env` file in the project root with the following values:

```ini
AZURE_SEARCH_ENDPOINT=your-search-endpoint
AZURE_SEARCH_KEY=your-search-key
AZURE_SEARCH_INDEX=your-index-name

AZURE_BLOB_CONN_STR=your-blob-connection-string
AZURE_BLOB_CONTAINER=your-container-name

AZURE_OPENAI_ENDPOINT=your-openai-endpoint
AZURE_OPENAI_KEY=your-openai-key
AZURE_OPENAI_DEPLOYMENT=your-embedding-deployment
AZURE_OPENAI_CHAT_DEPLOYMENT=your-chat-deployment
AZURE_OPENAI_MODEL_DIMENSIONS=1024
```

---

## 🛠️ How It Works

### 1. Upload Documents to Blob Storage
```python
upload_sample_documents(
    blob_connection_string=blob_connection_string,
    blob_container_name=blob_container_name,
    documents_directory="Documents/*.pdf"
)
```

### 2. Create Data Source in Azure AI Search
- Connects your **Blob container** as the data source.
- Configures **soft-delete policy** for updates/removals.

### 3. Define Search Index
- Fields: `title`, `chunk`, `vector`, `chunk_id`, etc.
- Enables:
  - **Vector Search (HNSW algorithm)**
  - **Semantic Search**

### 4. Generate Embeddings & Upload
- Uses **Azure OpenAI Embedding Model**.
- Stores embeddings and metadata in the index.

### 5. Retrieval & Orchestration
```python
query = "What are the eligibility criteria defined in Affine’s Sabbatical Leave Policy?"
answer, sources = answer_with_citations(query)

print("Answer:", answer)
for s in sources:
    print(s["citation"], ":", s["content"][:200])
```

✅ Returns a **grounded, citation-based answer** with semantic scoring.

### 6. Evaluation
- Retrieves the `@search.score` from results.
- Measures retrieval relevancy.

---

## 📊 Example Workflow
1. Upload PDFs → Blob Storage  
2. Index data → Azure AI Search  
3. Generate embeddings → Azure OpenAI  
4. Run semantic/vector search queries  
5. Get contextual answers with citations  

---

## 📁 Repository Structure

```
├── work.ipynb          # Jupyter notebook with the complete pipeline
├── Documents/          # Sample PDFs (not included in repo)
├── .env.example        # Example environment variables file
├── README.md           # Project documentation
```

---

## 🚀 Future Enhancements
- Add **RAG evaluation metrics** (precision, recall, MRR, nDCG).  
- Deploy as a **Streamlit / FastAPI app**.  
- Integrate with **LangChain / LlamaIndex** for advanced orchestration.  
- Support for **multi-modal document search**.  

---

## 📝 License
This project is licensed under the **MIT License**.  
