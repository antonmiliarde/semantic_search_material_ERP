# 1C/SAP AI-Powered Semantic Search Microservice

A production-ready, high-performance REST API microservice designed for semantic item matching and intelligent nomenclature processing inside corporate ERP systems (1C:Enterprise, SAP). 

The service solves the problem of manual data entry errors, typos, abbreviations, and word order mismatches when managers search for items in vast corporate databases.

## 🚀 Key Features

* **True Semantic Search**: Utilizes deep learning Transformer architecture (`deepvk/USER-bge-m3`) to convert text into dense 1024-dimensional embeddings, understanding synonyms and technical abbreviations (e.g., matching slang "шурик" with "дрель-шуруповерт").
* **Incremental Cache Updates**: Vector embeddings are mapped directly to unique 1C/SAP product IDs (UIDs). The system point-calculates only new or modified entries, ensuring instant system startups without re-indexing the entire database.
* **Hybrid Numerical Filtering**: Implements deterministic regex validation over probabilistic AI outputs. If critical numbers (volts, dimensions, amperes) in the query do not match the database item, a rigid penalty is applied to keep results precise.
* **100% Offline & On-Premise**: The model operates entirely locally inside the container, sending zero requests to external networks, which satisfies strict enterprise data privacy and security requirements.

## 🛠️ Technology Stack

* **Backend Framework**: FastAPI (Asynchronous Python REST API)
* **ASGI Server**: Uvicorn (High-performance C-based server execution)
* **Deep Learning & Math**: PyTorch, SentenceTransformers
* **Data Validation**: Pydantic v2
* **Containerization**: Docker

## 📦 Project Structure

```text
├── by_self.py          # FastAPI web server and core AI search logic
├── items.csv           # Demo 1C database export (Item_Code;Item_Name)
├── Dockerfile          # Production multi-stage Docker deployment configuration
└── requirements.txt    # Strict dependency list (optimized for CPU execution)
```

## 🔧 Installation & Local Setup

1. **Clone the repository and navigate to the project directory:**
   ```bash
   cd 1_search_material_by_name
   ```

2. **Install requirements (Optimized for CPU-only server environments):**
   ```bash
   pip install -r requirements.txt
   ```

3. **Run the development server with hot-reload:**
   ```bash
   uvicorn by_self:app --reload
   ```

4. **Access Interactive API Documentation**:
   Open your browser and navigate to `http://127.0.0` to open the built-in Swagger UI panel to test individual queries.

## 🐳 Docker Deployment (Production)

To package and run the application as an isolated container in a secure closed corporate network layout:

1. **Build the Docker image:**
   ```bash
   docker build -t ai-nomenclature-search .
   ```

2. **Run the container in detached background mode:**
   ```bash
   docker run -d -p 8000:8000 --name ai-search-service ai-nomenclature-search
   ```

The API endpoint will be exposed internally at `http://<server-ip>:8000/search` ready to ingest JSON payloads from your ERP system HTTP clients.
