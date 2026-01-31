---
tags:
  - main
---
---

# What is the OpenAI package?

- It's known as a Python Client Library. 
- It's nothing more than a wrapper around making this exact call to the http endpoint.
- It just allows you to work with nice Python code instead of messing around with janky json objects.
- But that's it. It's open-source and lightweight. Some people think it contains OpenAI model code - it doesn't!

```python
# Create OpenAI client
from openai import OpenAI

openai = OpenAI()

response = openai.chat.completions.create(model="gpt-5-nano", messages=[{"role": "user", "content": "Tell me a fun fact"}])

response.choices[0].message.content
```

OpenAI's Chat Completions API was so popular, that the other model providers created endpoints that are identical.  They are known as the "OpenAI Compatible Endpoints". And OpenAI decided to be kind: they said, hey, you can just use the same client library that we made for GPT. We'll allow you to specify a different endpoint URL and a different key, to use another provider. 

```python
gemini = OpenAI(base_url="https://generativelanguage.googleapis.com/v1beta/openai/", api_key="AIz....")

gemini.chat.completions.create(...)
```
---

## Ollama also gives an OpenAI compatible endpoint

```python
OLLAMA_BASE_URL = "http://localhost:11434/v1"
ollama = OpenAI(base_url=OLLAMA_BASE_URL, api_key='ollama')
response = ollama.chat.completions.create(model="llama3.2", messages=[{"role": "user", "content": "Tell me a fun fact"}])
response.choices[0].message.content
```

```
"Here's one: Did you know that honey never spoils? Archaeologists have found pots of honey in ancient Egyptian tombs that are over 3,000 years old and still perfectly edible! Honey's unique combination of acidity and water content creates an environment that is hostile to bacteria and other microorganisms, making it virtually immune to spoilage. Isn't that sweet?"
```
---
# chat.completions.create Parameters
```python
response = openai.chat.completions.create(model=MODEL, messages=easy_puzzle, reasoning_effort="none")
```
 - supported values of reasoning_effort are **none, minimal, low, medium, high** and **xhigh**.
 - [Docs](https://platform.openai.com/docs/api-reference/chat/create)
---
## Routers and Abtraction Layers

- Starting with the wonderful OpenRouter.ai - it can connect to all the models above!
- Visit openrouter.ai and browse the models.
- Here's one we haven't seen yet: GLM 4.5 from Chinese startup z.ai

```python
openrouter = OpenAI(base_url=OPENROUTER_URL, api_key=OPENROUTER_KEY)

response = openrouter.chat.completions.create(model="z-ai/glm-4.5", messages=tell_a_joke)

display(Markdown(response.choices[0].message.content))
```
---
