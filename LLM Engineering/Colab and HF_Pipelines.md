---
tags:
  - extension
created: 2026-02-01
---
---

## Before we start: 2 important pro-tips for using Colab:

**Pro-tip 1:**  Data Science code often gives warnings and messages. They can mostly be safely ignored! Glance over them, and if something goes wrong later, perhaps they can give you a clue.

**Pro-tip 2:** In the middle of running a Colab, you might get an error like this:

> Runtime error: CUDA is required but not available for bitsandbytes. Please consider installing [...]

This is a super-misleading error message! Please don't try changing versions of packages... 

This actually happens because Google has switched out your Colab runtime, perhaps because Google Colab was too busy. The solution is:

1. Kernel menu >> Disconnect and delete runtime
2. Reload the colab from fresh and Edit menu >> Clear All Outputs
3. Connect to a new T4 using the button at the top right
4. Select "View resources" from the menu on the top right to confirm you have a GPU
5. Rerun the cells in the colab, from the top down, starting with the pip installs  

---
```bash
pip install -q --upgrade datasets==3.6.0
```

## Breakdown

🔹pip install : Uses Python’s package manager to install libraries.
🔹 -q  (quiet) : Suppresses most install logs for cleaner notebook output.
🔹 --upgrade : Forces reinstall if the package already exists, ensuring the requested version.
🔹 datasets == 3.6.0 : Pins the install to **exactly version 3.6.0** of Hugging Face’s `datasets`.

Version pinning guarantees:

- Reproducibility
- Consistent APIs
- No breaking changes from newer releases

---

### ✅ In one sentence:

> It quietly installs (or updates) the Hugging Face `datasets` package to version **3.6.0** in your notebook environment.

This is done to ensure **version consistency**, so everyone running the Colab gets the _same behavior_ and avoids breaking changes.

---

They are two parts in the [[Hugging Face]] : **Tokenizer + Model** and **Pipeline**.

| Feature              | Tokenizer + Model | Pipeline            |
| -------------------- | ----------------- | ------------------- |
| Setup                | Manual            | Automatic           |
| Flexibility          | Maximum           | Limited             |
| Control              | Full              | Minimal             |
| Speed tuning         | Yes               | Harder              |
| Production readiness | Yes               | Usually no          |
| Learning curve       | Higher            | Very low            |
| Best for             | Custom systems    | Demos / prototyping |

---
# Pipelines
Pipelines are for simple out-of-the-box inference tasks. 
- Sentiment analysis
- classifiers
- named entity recognition
- question answering
- summarising
- translation

They can be used to generate text, image and Audio.

---
# Docs 
[Transformer-Pipelines](https://huggingface.co/docs/transformers/main_classes/pipelines)
[Diffuser-Pipelines](https://huggingface.co/docs/diffusers/en/api/pipelines/overview)

---
