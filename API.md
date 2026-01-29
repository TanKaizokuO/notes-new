---
tags:
  - main
---
---
## 🔗 What is an HTTP GET or POST?

- **HTTP** is the protocol that powers the web.
- **GET**: Retrieves data from a server. Think of visiting a website or fetching data.
    - Example: `GET https://api.example.com/users`
- **POST**: Sends data to a server, often creating or modifying a resource.
    - Example: `POST https://api.example.com/messages` with a message in the body

> Tip: GET is like reading. POST is like writing.

---
## 🌐 What is an Endpoint?

An **endpoint** is a specific URL that represents a resource or action in an API.

- For example:
    `https://api.openai.com/v1/chat/completions`
    This endpoint lets you create a chat completion using OpenAI's API.

> Think of an endpoint as a “doorway” into a specific function of a service.

---
## 📦 Python Client Libraries

### What is a Client Library?

A **client library** is a Python package that wraps HTTP calls for an API.

Instead of this:

```python
import requests 
requests.post(     
	 "https://api.sendgrid.com/v3/mail/send",
     headers={...},
     json={...} 
)
```


You do this:
```python
import sendgrid 
sg = sendgrid.SendGridAPIClient(api_key) 
sg.send(message)
```
### Why Use Them?

- They **abstract away** raw HTTP
- They handle **auth headers**, **request formats**, **errors**
- They’re more **readable** and often safer

---

