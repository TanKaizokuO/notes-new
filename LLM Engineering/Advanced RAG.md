---
created: 2026-05-02
tags:
  - main
---
---
# Advanced [[Retrieval Augmented Generation|RAG]]: Semantic Chunking, Visualization & Reranking

Moving beyond "Basic RAG" (simple character splitting), this note covers **Advanced RAG** patterns. The core philosophy here is **"Intelligence over Speed"**—using LLMs during the ingestion and retrieval stages to improve quality.

## 1. Intelligent Ingestion (No Abstractions)

Instead of using generic LangChain loaders, we build a custom pipeline to control exactly how data is processed.

### The "Smart" Chunking Strategy
Standard chunking (e.g., `RecursiveCharacterTextSplitter`) splits text blindly by character count. **Semantic Chunking** uses an LLM to read the document and split it logically.

**Key Innovation:** We don't just store the text. We generate:
1.  **Headline:** A generated title for searchability.
2.  **Summary:** A dense vector-friendly summary.
3.  **Original Text:** The ground truth.

#### Data Model (Pydantic)
We use `Pydantic` to enforce structured output from the LLM.

```python
from pydantic import BaseModel, Field

class Chunk(BaseModel):
    headline: str = Field(..., description="High-level heading for retrieval")
    summary: str = Field(..., description="Dense summary of the content")
    original_text: str = Field(..., description="The raw text from the document")

    def as_result(self, document):
        # We concatenate headline + summary + text for the vector embedding
        # This gives the embedding model more "hooks" to catch relevant queries
        return Result(
            page_content=self.headline + "\n\n" + self.summary + "\n\n" + self.original_text,
            metadata={"source": document["source"], "type": document["type"]}
        )
````

> [!TIP] Why do this?
> 
> A raw document might say "It costs $5."
> 
> An LLM-enriched chunk might say **"Headline: Premium Pricing. Summary: The monthly cost of the Gold Plan. Text: It costs $5."**
> 
> The enriched version is much easier to retrieve when a user asks "How much is the Gold Plan?"

---

## 2. Visualization (Observability)

You cannot improve what you cannot see. We use **t-SNE** (t-Distributed Stochastic Neighbor Embedding) to reduce our high-dimensional vectors (e.g., 1536 dims) down to 2D or 3D for plotting.

### The Logic

- **Goal:** Verify that semantically similar documents form physical "clusters" in space.    
- **Expectation:** "Employee" documents should group together, far away from "Product" documents.

```Python
from sklearn.manifold import TSNE
import plotly.graph_objects as go

# 1. Reduce dimensions
tsne = TSNE(n_components=3, random_state=42)
reduced_vectors = tsne.fit_transform(vectors)

# 2. Plot (Code snippet for 3D visualization)
fig = go.Figure(data=[go.Scatter3d(
    x=reduced_vectors[:, 0],
    y=reduced_vectors[:, 1],
    z=reduced_vectors[:, 2],
    mode='markers',
    marker=dict(size=5, color=colors, opacity=0.8),
    text=[f"Type: {t}<br>{d[:100]}..." for t, d in zip(doc_types, documents)]
)])
fig.show()
```

---

## 3. Retrieval Pipeline 2.0

In advanced RAG, retrieval is a multi-step process, not a single lookup.

### Step A: Query Rewriting

Users ask bad questions (e.g., "Who is he?"). We use an LLM to rewrite the query based on conversation history **before** searching the database.

- **Input:** "Who is he?" + History (User asked about Avery previously)    
- **Rewritten Query:** "Who is Avery Lancaster and what is his role at InsureLLM?"

```Python
def rewrite_query(question, history):
    system_prompt = """
    Respond only with a single, refined question that you will use to search the Knowledge Base.
    Focus on specific details. Don't mention the company name unless general.
    """
    # ... API Call to LLM ...
    return refined_question
```

### Step B: Reranking (The Precision Layer)

Vector Search (Cosine Similarity) is great at **Recall** (finding everything vaguely relevant) but bad at **Precision** (ranking the best answer #1).

**The Two-Stage Pattern:**

1. **Fetch Top K (Broad):** Retrieve 10-20 chunks using fast vector search.
2. **Rerank (Precise):** Use an LLM (or Cross-Encoder) to read all 20 chunks and strictly rank them by relevance to the question.
3. **Cutoff:** Take only the Top 5 from the reranked list.

```Python
class RankOrder(BaseModel):
    order: list[int] = Field(description="Order of chunks by relevance")

def rerank(question, chunks):
    # We ask the LLM to output a JSON list of indices [2, 0, 9, 1...]
    # representing the best order.
    user_prompt = f"Order these chunks by relevance to: {question}..."
    # ... API Call ...
    return [chunks[i] for i in order]
```

### Step C: The "Chain"

```Python
def answer_question(question, history):
    # 1. Rewrite the user's bad query
    refined_query = rewrite_query(question, history)
    
    # 2. Fetch broadly (Top 10)
    broad_chunks = fetch_context_unranked(refined_query)
    
    # 3. Rerank intelligently (Top 5)
    best_chunks = rerank(refined_query, broad_chunks)
    
    # 4. Generate Answer
    return generate_answer(question, best_chunks)
```

---

## Summary of Upgrades

|**Feature**|**Basic RAG**|**Advanced RAG**|**Benefit**|
|---|---|---|---|
|**Ingestion**|Character Splitter|LLM Semantic Splitter|Chunks are complete thoughts, not fragments.|
|**Metadata**|File path only|Headlines + Summaries|"Enriched" context improves retrieval hits.|
|**Query**|Raw user input|Rewritten Query|Resolves ambiguity ("it", "he") using history.|
|**Retrieval**|Top-K Vectors|Top-K -> Rerank|Fixes "Lost in the Middle" phenomenon.|

***

### Relevant Video
For a deeper visual understanding of how embeddings work and how `t-SNE` helps us visualize them (as implemented in your notebook), this video is excellent:
[Visualising embeddings with t-SNE](https://www.youtube.com/watch?v=MgayYUdI4is)

It walks through the exact concept of reducing high-dimensional vector data into the 2D/3D clusters you created in Plotly.
http://googleusercontent.com/youtube_content/0
