---
tags:
  - extension
created: 2026-02-01
---

---

LLMs often try to be efficient by calling multiple tools at once (e.g., "Price for Paris and London?"). The code must handle a **list** of tool calls, not just one.

## Updated Handler Logic

```python
def handle_tool_calls(message):
    responses = []
    
    # Iterate over ALL tool calls requested by the model
    for tool_call in message.tool_calls:
        
        if tool_call.function.name == "get_ticket_price":
            arguments = json.loads(tool_call.function.arguments)
            city = arguments.get('destination_city')
            
            # Execute function
            price_details = get_ticket_price(city)
            
            # Append to list of responses
            responses.append({
                "role": "tool",
                "content": price_details,
                "tool_call_id": tool_call.id
            })
            
    return responses
````

### Integration in Chat Loop

When adding to `messages`, use `.extend()` because we now have a list of tool responses.

```python
if response.choices[0].finish_reason == "tool_calls":
    message = response.choices[0].message
    responses = handle_tool_calls(message)
    
    messages.append(message)      # The "intent" to call tools
    messages.extend(responses)    # The results of ALL tools
    
    response = openai.chat.completions.create(model=MODEL, messages=messages)
```
---



# Step By Step Explaination

```text
User: london
```

### 🤖 Model output (internally)

The LLM decides it needs pricing data, so it returns:

```text
finish_reason = "tool_calls"

tool_calls = [
  get_ticket_price({"destination_city":"London"})
]
```

No natural language yet — just **intent to call tools**.

---

## 🔹 Step 1 — `handle_tool_calls(message)`

Now this runs:

```python
for tool_call in message.tool_calls:
```

Instead of assuming _one_ tool, you loop over **all** requested tools.

In this sample:

```text
tool_call.function.name = "get_ticket_price"
arguments = {"destination_city":"London"}
```

---

### Parse arguments

```python
arguments = json.loads(tool_call.function.arguments)
city = "London"
```

---

### Execute your backend function

```python
price_details = get_ticket_price("London")
```

Your Python returns:

```text
"The price of a ticket to London is $799"
```

---

### Wrap into TOOL message

You append:

```json
{
  "role": "tool",
  "content": "The price of a ticket to London is $799",
  "tool_call_id": tool_call.id
}
```

Because you’re using a list, `responses` becomes:

```json
[
  {
    role: "tool",
    content: "The price of a ticket to London is $799"
  }
]
```

Then:

```python
return responses
```

---

## 🔹 Step 2 — Integration in chat loop

Back in your main chat logic:

```python
message = response.choices[0].message
responses = handle_tool_calls(message)
```

Now:

- `message` = assistant requesting tools
- `responses` = list of tool outputs

---

### Add everything to conversation

```python
messages.append(message)
messages.extend(responses)
```

Important difference:

- `.append(message)` → adds the assistant’s tool request
- `.extend(responses)` → adds **ALL tool results**

So the conversation now looks like:

```
system: FlightAI rules
user: london
assistant: (call get_ticket_price)
tool: The price of a ticket to London is $799
```

---

## 🔹 Step 3 — Second OpenAI call

```python
response = openai.chat.completions.create(model=MODEL, messages=messages)
```

Now the LLM sees the tool output and produces final text:

```text
"The price of a ticket to London is $799."
```

---

## ✅ Final user-facing result

```text
The price of a ticket to London is $799.
```

---

# 🧠 Why this version matters

Your earlier code assumed **one tool**.

This version supports:

✅ multiple tool calls in one turn  
✅ parallel backend execution  
✅ scalable agent workflows

---
