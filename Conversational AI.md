---
tags:
  - main
created: 2026-01-31
---
---

# Building Context-Aware Chatbots with Gradio & OpenAI

This note covers the progression of building a chatbot using [[Gradio]]'s `ChatInterface` and the OpenAI API, moving from a static response to a dynamically prompted AI assistant.

---

## 1. The Basic Interface ("Hello World")

The simplest implementation of a Gradio chatbot requires a function that accepts a `message` and `history`.

```python
import gradio as gr

def chat(message, history):
    # Simplest possible response
    return "bananas"

# type="messages" ensures history is passed as a list of dictionaries 
# (OpenAI format) rather than a list of lists.
gr.ChatInterface(fn=chat, type="messages").launch()
````

> [!NOTE] Key Concepts
> 
> - **`fn`**: The function to call when the user sends a message.
>     
> - **`message`**: The current string input from the user.
>     
> - **`history`**: The conversation history.
>     

---

## 2. Connecting to OpenAI (Streaming)

To make the bot intelligent, we connect it to an LLM. We use `yield` instead of `return` to create a streaming effect (token-by-token generation), which feels faster to the user.

```Python
import openai

MODEL = "gpt-4o-mini" # or "gpt-3.5-turbo"

def chat(message, history):
    # 1. format history explicitly for safety/consistency
    history = [{"role": h["role"], "content": h["content"]} for h in history]
    
    # 2. Construct the message payload
    # Note: We will define system_message in the next section
    messages = [{"role": "system", "content": system_message}] + history + [{"role": "user", "content": message}]
    
    # 3. Call API with stream=True
    stream = openai.chat.completions.create(model=MODEL, messages=messages, stream=True)
    
    # 4. Accumulate and Yield
    response = ""
    for chunk in stream:
        response += chunk.choices[0].delta.content or ''
        yield response # Updates the UI incrementally

gr.ChatInterface(fn=chat, type="messages").launch()
```

---

## 3. Defining the Persona (System Prompt)

The **System Message** defines the AI's behavior. We can define this globally.

```python
system_message = "You are a helpful assistant in a clothes store. You should try to gently encourage \
the customer to try items that are on sale. Hats are 60% off, and most other items are 50% off. \
For example, if the customer says 'I'm looking to buy a hat', \
you could reply something like, 'Wonderful - we have lots of hats - including several that are part of our sales event.'\
Encourage the customer to buy hats if they are unsure what to get."
```

### updating the System Prompt

You can append to the system string to add more rules without rewriting the whole prompt.

```python
# Adding logic for shoes
system_message += "\nIf the customer asks for shoes, you should respond that shoes are not on sale today, \
but remind the customer to look at hats!"
```

---

## 4. Dynamic Context Injection (RAG-lite)

Instead of a massive static system prompt, we can inject specific instructions **conditionally** based on what the user types. This is a primitive form of [[RAG]] (Retrieval Augmented Generation).

**Scenario:** The prompt changes _only_ if the user mentions "belts".

```Python
def chat(message, history):
    # Standard history formatting
    history = [{"role": h["role"], "content": h["content"]} for h in history]
    
    # Start with the global system message
    relevant_system_message = system_message
    
    # --- DYNAMIC INJECTION LOGIC ---
    # Check if the keyword 'belt' is in the user's message
    if 'belt' in message.lower():
        relevant_system_message += " The store does not sell belts; if you are asked for belts, be sure to point out other items on sale."
    # -------------------------------

    # Use 'relevant_system_message' instead of the global 'system_message'
    messages = [{"role": "system", "content": relevant_system_message}] + history + [{"role": "user", "content": message}]
    
    stream = openai.chat.completions.create(model=MODEL, messages=messages, stream=True)
    
    response = ""
    for chunk in stream:
        response += chunk.choices[0].delta.content or ''
        yield response

gr.ChatInterface(fn=chat, type="messages").launch()
```

> [!TIP] Why use `relevant_system_message`?
> 
> We assign the modified prompt to a temporary variable (`relevant_system_message`) rather than appending to the global `system_message`.
> 
> If we appended to the global variable, the system prompt would grow indefinitely with every message, potentially confusing the AI or hitting token limits.