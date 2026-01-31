---
tags:
  - main
created: 2026-01-31
---
---

## Prompt Caching Comparison

Caching allows models to reuse processed tokens from previous requests, significantly reducing latency and cost for repetitive tasks.

### 1. OpenAI
**Mechanism**: Automatic / Implicit
* **Requirement**: Exact prefix matches.
* **Strategy**: Structure prompts with **static content first** (instructions, examples, tools) and **variable content last** (user-specific info).
* **Images/Tools**: Must be identical between requests to trigger a cache hit.



**Pricing**:
* Cached input tokens are **4x cheaper** than uncached input.

### 2. Anthropic (Claude)
**Mechanism**: Explicit
* You must strictly define *what* blocks of the prompt to cache.

**Pricing Structure**:
* **Writing (Priming)**: You pay **25% MORE** to write to the cache initially.
* **Reading (Reuse)**: You pay **10x LESS** when reusing cached content.

### 3. Google Gemini
**Mechanism**: Hybrid
* Supports both **implicit** (automatic) and **explicit** caching methods.

---

## Conversational Structure (Chat History)

LLM conversations are stateless by default. To create a continuous interaction (or an adversarial loop between bots), we structure the prompt as a list of message objects.

### The Structure
The standard format uses a list of dictionaries to maintain context:

```json
[
    {"role": "system", "content": "You are a helpful assistant."},
    {"role": "user", "content": "Explain quantum physics."},
    {"role": "assistant", "content": "Quantum physics deals with..."},
    {"role": "user", "content": "Expand on entanglement."}
]
```

```python
gpt_system = "You are a smart chatbot."

gpt_messages = ["Hi there"]
claude_messages = ["Hi"]

def call_gpt():

	messages = [{"role": "system", "content": gpt_system}]
	print(messages)
	for gpt, claude in zip(gpt_messages, claude_messages):
		messages.append({"role": "assistant", "content": gpt})
		print('in Loop',messages)
		messages.append({"role": "user", "content": claude})
		print('in Loop',messages)
	print(messages)
	response = openai.chat.completions.create(model=gpt_model, messages=messages)
	print(response.choices[0].message.content)
	return response.choices[0].message.content
	
call_gpt()
```

```json
[{'role': 'system', 'content': 'You are a smart chatbot.'}]

in Loop [{'role': 'system', 'content': 'You are a smart chatbot.'}, {'role': 'assistant', 'content': 'Hi there'}]

in Loop [{'role': 'system', 'content': 'You are a smart chatbot.'}, {'role': 'assistant', 'content': 'Hi there'}, {'role': 'user', 'content': 'Hi'}]

[{'role': 'system', 'content': 'You are a smart chatbot.'}, {'role': 'assistant', 'content': 'Hi there'}, {'role': 'user', 'content': 'Hi'}]

Hey there! 😊 How can I help you today?

```

---

# More advanced exercises


Try creating a 3-way, perhaps bringing Gemini into the conversation! One student has completed this - see the implementation in the community-contributions folder.

The most reliable way to do this involves thinking a bit differently about your prompts: just 1 system prompt and 1 user prompt each time, and in the user prompt list the full conversation so far.

Something like:
```python

system_prompt = """

You are Alex, a chatbot who is very argumentative; you disagree with anything in the conversation and you challenge everything, in a snarky way.

You are in a conversation with Blake and Charlie.

"""

  

user_prompt = f"""

You are Alex, in conversation with Blake and Charlie.

The conversation so far is as follows:

{conversation}

Now with this, respond with what you would like to say next, as Alex.

"""

```

Try doing this yourself before you look at the solutions. It's easiest to use the OpenAI python client to access the Gemini model (see the 2nd Gemini example above).