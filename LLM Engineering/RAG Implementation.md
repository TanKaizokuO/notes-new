---
created: 2026-02-05
tags:
  - main
---
---
### RAG Implementation

This note covers the construction of an [[Retrieval Augmented Generation|RAG]] based "Expert Question Answerer" using **LangChain 1.0**, **ChromaDB**, and **Gradio**.

## 1. The Stack
* **Orchestration:** LangChain (The "glue" logic).
* **Vector Store:** Chroma (Persisted locally in `vector_db`).
* **Embeddings:** HuggingFace (`all-MiniLM-L6-v2`) - *Selected for high speed and low memory usage.*
* **LLM:** `gpt-oss:20b` (via ChatOpenAI interface) - *Likely a local open-weights model being served via an OpenAI-compatible API endpoint.*
* **Interface:** Gradio - *For rapid UI prototyping.*

---

## 2. Key Concept: Temperature

> [!INFO] What is Temperature?
> Temperature controls the **diversity** and **randomness** of the LLM's output. It is often mistaken for "creativity," but it strictly controls the **probability distribution** of the next token prediction.

* **Low Temperature (e.g., 0):** The model becomes "greedy," almost always selecting the token with the highest probability. The output is deterministic, focused, and conservative.
* **High Temperature (e.g., 0.7 - 1.0):** The model flattens the probability curve, giving lower-probability tokens a chance to be picked. This results in variety but increases the risk of hallucinations.

> [!TIP] RAG & Temperature
> In RAG applications, we almost always set `temperature=0`. 
> **Why?** We want the model to be a **faithful translator** of the retrieved documents, not a creative writer. We want it to use facts from the context, not invent new ones.

---

## 3. Implementation Steps

### Step A: Setup & Connection
Connect to the existing Chroma vector database.

> [!CRITICAL] The Embedding Model Rule
> You **MUST** use the exact same embedding model for **retrieval** that you used for **ingestion**.
> If you used `all-MiniLM-L6-v2` to save the data, but try to search using `openai-text-embedding-3`, the vectors will exist in different mathematical spaces, and your search results will be gibberish.

```python
from langchain_chroma import Chroma
from langchain_huggingface import HuggingFaceEmbeddings
from langchain_openai import ChatOpenAI

# 1. Define Constants
DB_NAME = "vector_db"
MODEL = "gpt-oss:20b"

# 2. Load Embeddings & Connect to DB
embeddings = HuggingFaceEmbeddings(model_name="all-MiniLM-L6-v2")

# We don't need to re-ingest data. We just point to the existing directory.
vectorstore = Chroma(persist_directory=DB_NAME, embedding_function=embeddings)
````

### Step B: The Retriever & The LLM

The `retriever` fetches relevant chunks, and the `llm` generates the answer.

> [!NOTE] What is `as_retriever()`?
> 
> This converts the Vector Store into a Runnable interface.
> 
> By default, it performs a **Similarity Search** (Cosine Similarity) and returns the top **k=4** documents.
> 
> You can customize this: `vectorstore.as_retriever(search_kwargs={"k": 3})`

Python

```
# Create Retriever interface
retriever = vectorstore.as_retriever()

# Initialize LLM with specific Temperature
# We use ChatOpenAI, but pointing to our local/custom model name.
llm = ChatOpenAI(temperature=0, model_name=MODEL)
```

### Step C: The RAG Loop (The "Glue")

This function combines retrieval with generation. It injects the retrieved documents into the system prompt before asking the LLM for an answer.

> [!abstract] The "Context Window" Strategy
> 
> 1. **Retrieve:** We find the 4 most relevant text chunks from our database based on the user's question.
>     
> 2. **Stuff:** We "stuff" those chunks into the `{context}` slot of the System Prompt.
>     
> 3. **Grounding:** The prompt explicitly tells the LLM "If you don't know, say so." This is a **guardrail** to prevent the model from using its pre-training training data (which might be outdated or irrelevant) instead of our specific documents.
>     

```python
from langchain_core.messages import SystemMessage, HumanMessage

SYSTEM_PROMPT_TEMPLATE = """
You are a knowledgeable, friendly assistant representing the company Insurellm.
You are chatting with a user about Insurellm.
If relevant, use the given context to answer any question.
If you don't know the answer, say so.
Context:
{context}
"""

def answer_question(question: str, history):
    # 1. Retrieve relevant documents (The "R" in RAG)
    docs = retriever.invoke(question)
    
    # 2. Format context (The "A" in RAG)
    # We join the content of the retrieved docs with newlines to make a single block of text
    context = "\n\n".join(doc.page_content for doc in docs)
    
    # 3. Fill the prompt template
    system_prompt = SYSTEM_PROMPT_TEMPLATE.format(context=context)
    
    # 4. Generate Answer (The "G" in RAG)
    response = llm.invoke([
        SystemMessage(content=system_prompt), 
        HumanMessage(content=question)
    ])
    
    return response.content
```

### Step D: User Interface

Using `gradio` to create a chat interface wrapper around the function.

> [!example] Why Gradio?
> 
> Gradio automatically detects that our function accepts a string (`question`) and returns a string (`response`). It creates the text box, submit button, and chat history window automatically.


```python
import gradio as gr

# The 'history' argument in answer_question is required by Gradio's ChatInterface contract,
# even if we aren't using conversational memory yet.
gr.ChatInterface(answer_question).launch()
```