---
tags:
  - main
aliases:
created: 2026-01-30
---

---
# Tokens and Tokenizers in LLMs (e.g., GPT)

## 1. What is a Token?
A **token** is the fundamental unit of text that an LLM processes. While humans read text as characters or words, models read a stream of numerical integers (Token IDs).

> [!NOTE] Rule of Thumb
> For English text, **1,000 tokens $\approx$ 750 words**.
> A token is roughly 4 characters of text.

### The Spectrum of Granularity
* **Character-level:** "a", "p", "p", "l", "e" (Too long sequences, small vocab).
* **Word-level:** "apple" (Huge vocab, struggles with unknown words).
* **Subword-level (Current Standard):** "app", "le" (Balances sequence length and vocabulary size).

---

## 2. The Tokenizer
The **Tokenizer** is a separate, pre-trained component that sits *before* the LLM. It translates raw text into integers (encoding) and integers back into text (decoding).

### The Pipeline
1.  **Normalization:** Unicode normalization (NFD), lowercasing (optional).
2.  **Pre-tokenization:** Splitting inputs by boundaries (whitespace, punctuation).
3.  **Model:** The core algorithm (e.g., BPE) that applies the merge rules.
4.  **Post-processing:** Adding special tokens like `[CLS]`, `[SEP]`, or `<|endoftext|>`.
5. Most modern LLMs (GPT-3, GPT-4, Llama) use Byte-Pair Encoding.

---

## 3. Impact on Model Behavior
Tokenization explains many "weird" behaviors of LLMs.

* **Math & Arithmetic:** Models struggle with math because numbers are often tokenized inconsistently.
    * *Input:* `1234` might be one token.
    * *Input:* `1235` might be two tokens (`12`, `35`).
    * *Result:* The model cannot easily learn column-wise arithmetic because it doesn't see the digits, it sees unique IDs.
* **Coding:** Whitespace is treated as tokens. Indentation in Python costs tokens.
* **Non-English Languages:** If the tokenizer was trained primarily on English, other languages will be fragmented into many small tokens (more "expensive" to process and shorter context memory).

> [!WARNING] The "SolidGoldMagikarp" Glitch
> Some strings appear in the tokenizer vocabulary (often from Reddit data) but were never actually seen during the model's training. If you prompt a model with these "glitch tokens," it can cause undefined behavior or hallucinations.

---

## 4. Vocabulary & Efficiency

| Feature | Large Vocabulary | Small Vocabulary |
| :--- | :--- | :--- |
| **Sequence Length** | **Shorter** (More info per token) | **Longer** (Requires more compute) |
| **Embedding Matrix** | **Larger** (More parameters) | **Smaller** (Fewer parameters) |
| **Handling Rare Words** | Better (if in vocab) | Worse (fragmented) |

---

## 5. Implementation (Python/Tiktoken)
OpenAI uses `tiktoken` for their BPE tokenization.

```python
import tiktoken

# Load the encoding for GPT-4
enc = tiktoken.encoding_for_model("gpt-4")

text = "Tokenization is fascinating"
tokens = enc.encode(text)

print(tokens) 
# Output: [5963, 2235, 374, 18274]
# "Token" "ization" " is" " fascinating"

print(enc.decode(tokens))
# Output: "Tokenization is fascinating"
````

---
## 7. Relevant Links
- [[Context Window]]
- [OpenAI Tokenizer Tool](https://platform.openai.com/tokenizer)