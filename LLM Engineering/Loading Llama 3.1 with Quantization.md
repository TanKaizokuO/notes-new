---
tags:
  - main
created: 2026-02-02
---
---

Running modern Large Language Models (LLMs) like Llama 3.1 often requires optimizing memory usage. We use **Quantization** to load the model in 4-bit precision instead of 16-bit or 32-bit.
## 1. Quantization Configuration
We use `BitsAndBytesConfig` to define how the model should be compressed.

```python
from transformers import BitsAndBytesConfig
import torch

quant_config = BitsAndBytesConfig(
    load_in_4bit=True,
    bnb_4bit_use_double_quant=True,
    bnb_4bit_compute_dtype=torch.bfloat16,
    bnb_4bit_quant_type="nf4"
)
````

## 2. Tokenizer Setup

The tokenizer converts text into numbers (tokens) the model can understand.

```Python
from transformers import AutoTokenizer

tokenizer = AutoTokenizer.from_pretrained(LLAMA)
tokenizer.pad_token = tokenizer.eos_token

# Prepare inputs
inputs = tokenizer.apply_chat_template(messages, return_tensors="pt").to("cuda")
```

## 3. Loading the Model

We pass the `quantization_config` to the `from_pretrained` method.


```Python
from transformers import AutoModelForCausalLM

model = AutoModelForCausalLM.from_pretrained(
    LLAMA, 
    device_map="auto", 
    quantization_config=quant_config
)

# Check Memory Footprint
memory = model.get_memory_footprint() / 1e6
print(f"Memory footprint: {memory:,.1f} MB")
```

---
 Next : [[Streaming, Generation & Memory Management]]
