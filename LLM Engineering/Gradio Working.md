---
tags:
  - extension
created: 2025-02-01
---
---
## 1. Architecture & Lifecycle
Gradio acts as a bridge between Python functions and a modern web interface.
### How it Works Under the Hood
1.  **Frontend Generation**:
    * Gradio constructs a frontend app based on your Python UI description.
    * *Note*: While often described loosely as "React-like", modern Gradio components are primarily built using **Svelte**.
2.  **Server Launch**:
    * It starts a local web server using **Starlette** (a lightweight ASGI framework/toolkit, similar to FastAPI).
    * This server listens on a free port (defaulting to `7860`) to serve the frontend app.
3.  **Backend Routing**:
    * Gradio creates API routes for your **callbacks** (e.g., your `chat()` function).
    * When a user interacts with the UI (clicks "Submit"), the frontend calls these specific backend routes to execute your Python logic.

---

## 2. Gradio UI Types
Gradio offers three distinct levels of abstraction for building interfaces.

| Type | Best For | Usage |
| :--- | :--- | :--- |
| **`gr.Interface`** | **Standard, simple UIs** | You provide a function, input widgets, and output widgets. Gradio handles the layout entirely. Great for quick prototypes. |
| **`gr.ChatInterface`** | **Standard Chatbot UIs** | A high-level abstraction specifically for LLMs. Automatically handles chat history state, input box, and submit logic. |
| **`gr.Blocks`** | **Custom UIs** | You control the exact placement of components (Rows, Columns) and the wiring of events. Essential for complex apps or multi-step workflows. |

---

## 3. Building Custom Chatbots with `gr.Blocks`
When using `gr.Blocks`, you must manually handle the layout and the event flow.

### Step A: The Callbacks
You often need helper functions to manage the UI state (e.g., showing the user's message immediately) before calling the heavy LLM function.

```python
# Helper to immediately show the user's message in the chat window
def put_message_in_chatbot(message, history):
    return "", history + [{"role": "user", "content": message}]
````

### Step B: The UI Definition

Use `with` blocks to define the hierarchy (Rows and Columns).

```Python
import gradio as gr

with gr.Blocks() as ui:
    # Top Row: Chatbot + Image
    with gr.Row():
        chatbot = gr.Chatbot(height=500, type="messages")
        image_output = gr.Image(height=500, interactive=False)

    # Middle Row: Audio
    with gr.Row():
        audio_output = gr.Audio(autoplay=True)

    # Bottom Row: Input
    with gr.Row():
        message = gr.Textbox(label="Chat with our AI Assistant:")
```

### Step C: Event Chaining (`.then`)

This is the critical piece for responsiveness. We chain events to update the UI in steps.

1. **`message.submit(...)`**: First, calls `put_message_in_chatbot`.
    - _Result_: Clears the textbox (`""`) and adds the user's message to the chat history immediately.
2. **`.then(...)`**: Once step 1 finishes, it calls the main `chat` function.
    - _Result_: Generates the AI response, audio, and image.

```Python
    # Wiring the events
    message.submit(
        put_message_in_chatbot, 
        inputs=[message, chatbot], 
        outputs=[message, chatbot]
    ).then(
        chat, # The main LLM logic function
        inputs=chatbot, 
        outputs=[chatbot, audio_output, image_output]
    )

    ui.launch(inbrowser=True, auth=("ed", "bananas"))
```