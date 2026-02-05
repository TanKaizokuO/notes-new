---
tags:
  - main
created: 2026-02-02
---

---
# LLaMA Architecture Breakdown (Quantized)

This note breaks down the module tree of a LLaMA-style causal language model (specifically optimized with 4-bit quantization).

---

## 🌳 Module Tree Overview

```text
LlamaForCausalLM
 ├─ model: LlamaModel
 │   ├─ embed_tokens
 │   ├─ layers (32 × LlamaDecoderLayer)
 │   ├─ norm
 │   └─ rotary_emb
 └─ lm_head
````

**Key Stats**:

- **Architecture**: Decoder-only Transformer.
- **Layers**: 32.
- **Hidden Size**: 4096.
- **Vocabulary**: 128,256.
- **Optimization**: 4-bit quantized linear layers (`Linear4bit`).

---

## 📦 Top-Level: `LlamaForCausalLM`

This module wraps the base transformer model and adds the language modeling head required for generation.

### `lm_head`

- **Type**: `Linear(in_features=4096, out_features=128256, bias=False)`
- **Function**: Projects final hidden states → vocabulary logits.
- **Shape**: `4096 → 128,256`
- **Usage**: Used to predict the probability distribution of the next token.

---

## 🏗 Core Model: `LlamaModel`

### 1. Token Embeddings (`embed_tokens`)

- **Type**: `Embedding(128256, 4096)`
- **Function**: Maps discrete Token IDs → Continuous Vectors.
- **Result**: Each token becomes a 4096-dimensional vector.

### 2. Decoder Stack (`layers`)

- **Structure**: 32 identical `LlamaDecoderLayer` blocks.

### 3. Rotary Positional Embeddings (`rotary_emb`)

- **Type**: `LlamaRotaryEmbedding`
- **Function**: Injects position information directly into Q/K vectors using rotation matrices (RoPE).
- **Benefit**: Enables better extrapolation to longer context windows compared to learned embeddings.

### 4. Final Normalization (`norm`)

- **Type**: `LlamaRMSNorm(4096)`
- **Function**: Applied after all 32 layers, just before the `lm_head`.

---

## 🔁 Inside a Single `LlamaDecoderLayer`

Each of the 32 layers contains two main sub-blocks: Attention and MLP.

### 🔹 Self Attention (`LlamaAttention`)

- **Components** (All `Linear4bit`):
    - `q_proj`: `4096 → 4096`
    - `k_proj`: `4096 → 1024`
    - `v_proj`: `4096 → 1024`
    - `o_proj`: `4096 → 4096`
- **Grouped Query Attention (GQA)**:
    - **Q (Query)** keeps full dimension (4096).
    - **K/V (Key/Value)** are reduced to 1024.
    - **Ratio**: 4 Query heads share 1 Key/Value head.
    - **Benefit**: Faster inference and significantly lower memory usage (KV-cache) with minimal quality loss.

### 🔹 MLP Block (`LlamaMLP`)

- **Components**:
    - `gate_proj`: `4096 → 14336` (Expansion ~3.5x)
    - `up_proj`: `4096 → 14336`
    - `down_proj`: `14336 → 4096`
    - `act_fn`: `SiLU`
- **Architecture**: **SwiGLU** (Swish-Gated Linear Unit).
- **Formula**: `down_proj( SiLU(gate_proj(x)) * up_proj(x) )`

### 🔹 Layer Normalization

- **Type**: RMSNorm (Root Mean Square Normalization).
- **Locations**:
    
    1. `input_layernorm`: Before Attention.
    2. `post_attention_layernorm`: Before MLP.

---

## 📐 Summary Table

|**Component**|**Purpose**|**Shape**|
|---|---|---|
|**Token Embedding**|Token → vector|`128,256 × 4,096`|
|**Layers**|Transformer blocks|`32`|
|**Attention Q**|Query projection|`4,096 → 4,096`|
|**Attention K/V**|Key/Value (GQA)|`4,096 → 1,024`|
|**MLP expand**|Feedforward up|`4,096 → 14,336`|
|**MLP contract**|Feedforward down|`14,336 → 4,096`|
|**Final norm**|Stabilization|`4,096`|
|**LM Head**|Hidden → vocab|`4,096 → 128,256`|

---

## 🚀 Key Architectural Observations

1. **Decoder-only**: Standard GPT-style architecture.
2. **GQA (Grouped Query Attention)**: Smaller K/V heads for efficiency.
3. **SwiGLU MLP**: Uses 3 matrices instead of the traditional 2, offering higher expressivity.
4. **RoPE**: Rotary embeddings for better position handling.
5. **RMSNorm**: Used everywhere instead of standard LayerNorm for stability.
6. **4-bit Quantization**: Applied to all major linear layers to reduce VRAM footprint.