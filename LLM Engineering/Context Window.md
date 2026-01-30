---
tags:
  - extension
created: 2026-01-18
---
---
# LLM Memory

  > [!NOTE] LLM Memory is a myth
> Every call to an LLM is completely STATELESS. It's a totally new call, every single time. As AI  engineers, it's OUR JOB to devise techniques to give the impression that the LLM has a "memory".

1. Every call to an LLM is stateless
2. We pass in the entire conversation so far in the input prompt, every time
3. This gives the illusion that the LLM has memory - it apparently keeps the context of the conversation
4. But this is a trick; it's a by-product of providing the entire conversation, every time
5. An LLM just predicts the most likely next tokens in the sequence; if that sequence contains "My name is Ed" and later "What's my name?" then it will predict.. Ed!

The ChatGPT product uses exactly this trick - every time you send a message, it's the entire conversation that gets passed in.

---
# Context Window
Max number of tokens that the model can consider when generating the next token includes the original input prompt subsequent conversation the latest input prompt and almost all output prompt.

It governs how well the model can remember references, content and context.
particularly important for multi-shot prompting where the prompt includes examples or for long conversations.

---

