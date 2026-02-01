---
tags:
  - main
created: 2026-02-01
---
---

Our code prompts LLM and LLM sends a response that we need to call a tool then our code calls the tool(code), takes its answer and sends it back to LLM in the conversation history.
## The Concept
Standard LLMs cannot access real-time data or perform actions (like booking tickets or querying a database). **Function Calling** (or Tool Use) bridges this gap.

### The Workflow
1.  **Define**: You describe a function to the LLM (name, description, arguments) using a specific JSON schema.
2.  **Prompt**: You ask the LLM a question (e.g., "How much is a ticket to London?").
3.  **Decision**: The LLM *decides* not to answer directly, but to issue a **Tool Call** request (e.g., "Call `get_ticket_price` with city='London'").
4.  **Execute**: Your code executes the Python function using the arguments provided by the LLM.
5.  **Return**: You feed the function's output back to the LLM.
6.  **Answer**: The LLM uses that data to generate the final natural language response.

## Defining a Tool (JSON Schema)
The API requires a specific structure to understand your function.

```python
price_function = {
    "name": "get_ticket_price",
    "description": "Get the price of a return ticket to the destination city.",
    "parameters": {
        "type": "object",
        "properties": {
            "destination_city": {
                "type": "string",
                "description": "The city that the customer wants to travel to",
            },
        },
        "required": ["destination_city"],
        "additionalProperties": False
    }
}

# Wrap it in the tools list
tools = [{"type": "function", "function": price_function}]
```

---
# Related

1. [[Handling a Single Tool Call]]
2. [[Handling Multiple Tool Calls]]
3. [[The Tool Loop (While Loop)]]
4. [[Connecting Tools to Databases]]

---
# Next Step
[[Multimodal Chatbot]]

---
