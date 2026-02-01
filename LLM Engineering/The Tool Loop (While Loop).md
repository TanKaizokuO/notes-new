---
tags:
  - extension
created: 2026-02-01
---
---

Sometimes, the result of one tool might require the LLM to call *another* tool (or the same one again). A simple `if` statement only handles one round of tools. A `while` loop enables multi-step reasoning (Agentic behavior).

---

# 🧠 Why this LOOP matters

```text
LLM → tools → LLM → tools → LLM → final answer
```

---
## The Pattern

```python
def chat(message, history):
    # ... setup messages ...
    
    # Initial call
    response = openai.chat.completions.create(model=MODEL, messages=messages, tools=tools)
    
    # LOOP: Keep running as long as the LLM wants to call tools
    while response.choices[0].finish_reason == "tool_calls":
        message = response.choices[0].message
        
        # Execute tools
        tool_responses = handle_tool_calls(message)
        
        # Update history
        messages.append(message)
        messages.extend(tool_responses)
        
        # Call LLM again to see if it's done or needs more tools
        response = openai.chat.completions.create(model=MODEL, messages=messages, tools=tools)
        
    return response.choices[0].message.content
````

> [!WARNING] Infinite Loops
> 
> In production, you should add a `max_iterations` counter to break this loop if the model gets stuck calling tools repeatedly.

---

# Step by Step Explaination

Sure — let’s explain this **loop-based tool orchestration** using a concrete sample run.

---

# 📥 Sample input

```text
User: london
```

---

# 🔹 Step 1 — Initial LLM call

```python
response = openai.chat.completions.create(model=MODEL, messages=messages, tools=tools)
```

The model sees:

```text
system: FlightAI rules
user: london
```

Instead of answering directly, it returns:

```text
finish_reason = "tool_calls"
tool_calls = [
  get_ticket_price({"destination_city":"London"})
]
```

Meaning:

👉 _“I need backend data before replying.”_

---

# 🔹 Step 2 — Enter the WHILE loop

```python
while response.choices[0].finish_reason == "tool_calls":
```

Since it requested tools, we enter the loop.

---

## Inside loop — iteration #1

### ✅ Extract tool request

```python
message = response.choices[0].message
```

This contains:

```text
assistant → call get_ticket_price("London")
```

---

### ✅ Execute tools

```python
tool_responses = handle_tool_calls(message)
```

Your Python runs:

```text
get_ticket_price("London")
→ "The price of a ticket to London is $799"
```

Returned as:

```text
[
  {
    role: "tool",
    content: "The price of a ticket to London is $799"
  }
]
```

---

### ✅ Update conversation

```python
messages.append(message)
messages.extend(tool_responses)
```

Now the conversation becomes:

```text
system: FlightAI rules
user: london
assistant: (call get_ticket_price)
tool: The price of a ticket to London is $799
```

---

### ✅ Call LLM again

```python
response = openai.chat.completions.create(model=MODEL, messages=messages, tools=tools)
```

Now the model has the price.

It replies:

```text
finish_reason = "stop"
content = "The price of a ticket to London is $799."
```

---

# 🔹 Step 3 — Exit loop

Because:

```text
finish_reason != "tool_calls"
```

the `while` loop stops.

---

# ✅ Final return

```python
return response.choices[0].message.content
```

User receives:

```text
The price of a ticket to London is $799.
```

---
