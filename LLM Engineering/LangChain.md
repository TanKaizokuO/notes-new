---
tags:
  - extension
created: 2026-02-05
---
---
## RAG Implementation: Ingestion, Chunking, and Vectors

## 1. LangChain: Pros & Cons
Before diving into the code, it is helpful to understand the trade-offs of using [[LangChain]] for orchestration.

### Pros
* **Abstraction:** Simplifies complex logic (like switching between different LLM providers or Vector Stores) with unified APIs.
* **Ecosystem:** Massive library of integrations (loaders for PDFs, Notion, SQL, etc.).
* **Pre-built Chains:** Ready-to-use architectures for RAG, Conversational Retrieval, etc.
* **Component Modularity:** Easy to swap out the embedding model or vector store without rewriting the whole application.

### Cons
* **Complexity/Bloat:** Can feel like "over-engineering" for simple scripts; the abstraction layers can make debugging difficult ("Abstraction Leaks").
* **Learning Curve:** The syntax changes frequently (e.g., v0.1 vs v0.2).
* **Latency:** The abstraction layers can introduce slight overhead compared to raw API calls.

---

## PART A: Document Loading & Chunking

### Approach 1: Raw String Loading (Manual)
*Useful for quick analysis of total corpus size, but loses file-specific metadata.*

```python
import glob

knowledge_base_path = "knowledge-base/**/*.md"
files = glob.glob(knowledge_base_path, recursive=True)
print(f"Found {len(files)} files in the knowledge base")

entire_knowledge_base = ""
for file_path in files:
    with open(file_path, 'r', encoding='utf-8') as f:
        entire_knowledge_base += f.read()
    entire_knowledge_base += "\n\n"
  
print(f"Total characters in knowledge base: {len(entire_knowledge_base):,}")
````

### Approach 2: LangChain Loaders (Structured)

_Best practice for RAG. Preserves metadata (source file, path) which is crucial for citations later._

```Python
import os
from langchain_community.document_loaders import DirectoryLoader, TextLoader

# Load in everything in the knowledgebase using LangChain's loaders
folders = glob.glob("knowledge-base/*") 

documents = []

for folder in folders:
    doc_type = os.path.basename(folder)
    # Using TextLoader specifically for markdown files
    loader = DirectoryLoader(folder, glob="**/*.md", loader_cls=TextLoader, loader_kwargs={'encoding': 'utf-8'})
    folder_docs = loader.load()
    
    # Injecting custom metadata to track where the doc came from
    for doc in folder_docs:
        doc.metadata["doc_type"] = doc_type
        documents.append(doc)

print(f"Loaded {len(documents)} documents")
```

### Approach 3: Text Splitting

_We use `RecursiveCharacterTextSplitter` to respect sentence/paragraph boundaries._

- **Chunk Size:** 1000 tokens/chars (approx 2-3 paragraphs).
- **Overlap:** 200 tokens/chars (ensures context isn't lost between cuts).

```Python
from langchain_text_splitters import RecursiveCharacterTextSplitter

# Divide into chunks
text_splitter = RecursiveCharacterTextSplitter(chunk_size=1000, chunk_overlap=200)
chunks = text_splitter.split_documents(documents) 

print(f"Divided into {len(chunks)} chunks")
print(f"First chunk:\n\n{chunks[0]}")
```

---

## PART B: Vectors & Storage (ChromaDB)

This step converts text chunks into mathematical vectors and stores them.

> [!NOTE] Model Choice
> 
> - **HuggingFace (all-MiniLM-L6-v2):** Free, runs locally, fast, lower dimension (384).
>     
> - **OpenAI (text-embedding-3-large):** Paid API, higher accuracy, higher dimension (3072).
>     

```Python
from langchain_huggingface import HuggingFaceEmbeddings
# from langchain_openai import OpenAIEmbeddings
from langchain_chroma import Chroma
import os

db_name = "chroma_db"

# 1. Pick an embedding model  
embeddings = HuggingFaceEmbeddings(model_name="all-MiniLM-L6-v2")
# embeddings = OpenAIEmbeddings(model="text-embedding-3-large")

# 2. Reset DB if exists (Fresh Start)
if os.path.exists(db_name):
    # Note: Ensure you have the right client setup if running strictly local
    try:
        Chroma(persist_directory=db_name, embedding_function=embeddings).delete_collection()
    except Exception as e:
        print("Collection might not exist yet, continuing...")

# 3. Create Vector Store
vectorstore = Chroma.from_documents(
    documents=chunks, 
    embedding=embeddings, 
    persist_directory=db_name
)

print(f"Vectorstore created with {vectorstore._collection.count()} documents")

# 4. Investigate the vectors (Sanity Check)
collection = vectorstore._collection
count = collection.count()
# Fetch one embedding to check dimensions
sample_embedding = collection.get(limit=1, include=["embeddings"])["embeddings"][0]
dimensions = len(sample_embedding)

print(f"There are {count:,} vectors with {dimensions:,} dimensions in the vector store")
```

---
Related : [[Retrieval Augmented Generation|RAG]]

