---
tags:
  - extension
created: 2026-02-02
---
---
## 1. Basic Generation & Cleanup
Generating text returns input IDs, which must be decoded back to text.

```python
# Generate
outputs = model.generate(inputs, max_new_tokens=80)

# Decode
print(tokenizer.decode(outputs[0]))

# Memory Cleanup (Crucial for GPU)
import gc
del model, inputs, tokenizer, outputs
gc.collect()
torch.cuda.empty_cache()

```
## 2. Streaming Responses

Instead of waiting for the full response, `TextStreamer` prints tokens as they are generated.

```Python
from transformers import TextStreamer

streamer = TextStreamer(tokenizer)
outputs = model.generate(inputs, max_new_tokens=80, streamer=streamer)
```
## 3. Generation Prompts

When using `apply_chat_template`, the argument `add_generation_prompt=True` is critical for chat models (like Phi-3).

- **True**: Tells the model "It is now your turn to speak," prompting an answer.
- **False**: The model might just continue the user's sentence instead of answering.

## 4. Universal Generation Function

A wrapper to handle different models (Phi, Gemma, Qwen, DeepSeek) with optional quantization.

```Python
def generate(model_id, messages, quant=True, max_new_tokens=80):
    tokenizer = AutoTokenizer.from_pretrained(model_id)
    tokenizer.pad_token = tokenizer.eos_token
    
    # Enable generation prompt
    input_ids = tokenizer.apply_chat_template(
        messages, 
        return_tensors="pt", 
        add_generation_prompt=True
    ).to("cuda")
    
    attention_mask = torch.ones_like(input_ids, dtype=torch.long, device="cuda")
    streamer = TextStreamer(tokenizer)
    
    # Conditionally Load Quantized vs Full
    if quant:
        model = AutoModelForCausalLM.from_pretrained(
            model_id, 
            quantization_config=quant_config
        ).to("cuda")
    else:
        model = AutoModelForCausalLM.from_pretrained(model_id).to("cuda")
        
    outputs = model.generate(
        input_ids=input_ids, 
        attention_mask=attention_mask, 
        max_new_tokens=max_new_tokens, 
        streamer=streamer
    )
```
---
