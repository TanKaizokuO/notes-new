---
tags:
  - main
created: 2026-02-02
---
---
# Anatomy of a Transformer Model 
When inspecting the `model` object in PyTorch, you see the internal layers of the Transformer [[Archiectures|archietecture]] (invented by Google in 2017). 
## Key Components 
### 1. Embeddings 
* **Function**: Takes tokens (integers) and turns them into dense vectors (e.g., 4,096 dimensions for Llama). 
* *Note*: This maps discrete words to a continuous vector space. 
### 2. Decoder Layers 
* Llama 3.1 consists of 32 "Decoder Layers". Each layer typically contains: *
* **Self-Attention**: Allows the model to relate different parts of the sequence to each other. 
* **MLP (Multi-Layer Perceptron)**: A feed-forward network processing information locally at each position. 
* **Batch/Layer Norm**: Normalizes values to stabilize training. 

### 3. LM Head (Language Modeling Head) 
* **Function**: The final layer that projects the hidden states back to the vocabulary size to predict the next token. 
### Inspecting Source Code 
* You can view the specific implementation of these layers in the Hugging Face library: * **Repo**: [Hugging Face Transformers](https://github.com/huggingface/transformers) * **Llama Code**: src/transformers/models/llama/modeling_llama.py`
---
