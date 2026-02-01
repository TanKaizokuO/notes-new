---
tags:
  - main
created: 2026-02-01
---
---

This note demonstrates how to upgrade a standard text-based chatbot into a **multimodal** assistant that can generate images (via DALL-E 3) and speak responses (via OpenAI TTS), all while handling function calls.

---
## 1. Image Generation (DALL-E 3)
We define an `artist` function that takes a city name and generates a visual representation using DALL-E 3.

```python
import base64
from io import BytesIO
from PIL import Image
import openai

def artist(city):
    """Generates a pop-art style image of the city using DALL-E 3."""
    image_response = openai.images.generate(
        model="dall-e-3",
        prompt=f"An image representing a vacation in {city}, showing tourist spots and everything unique about {city}, in a vibrant pop-art style",
        size="1024x1024",
        n=1,
        response_format="b64_json",
    )
    # Decode base64 image data
    image_base64 = image_response.data[0].b64_json
    image_data = base64.b64decode(image_base64)
    return Image.open(BytesIO(image_data))

# Example usage
# image = artist("New York City")
# display(image)
````

---

## 2. Text-to-Speech (TTS)

We define a `talker` function that converts the AI's text response into audio.

```python
def talker(message):
    """Converts text to speech using OpenAI's TTS model."""
    response = openai.audio.speech.create(
        model="tts-1",  # or "tts-1-hd" for higher quality
        voice="onyx",   # Options: alloy, echo, fable, onyx, nova, shimmer
        input=message
    )
    return response.content # Returns binary audio data
```

---

## 3. The Multimodal Chat Logic

The `chat` function now orchestrates three tasks:

1. **Reasoning & Tools**: Calls tools (like `get_ticket_price`) if needed.
2. **Audio Generation**: Converts the final text reply to speech.
3. **Image Generation**: If a specific tool (price lookup) was called, it triggers an image generation for that city.

```Python
def chat(history):
    # 1. Format history for OpenAI
    history = [{"role": h["role"], "content": h["content"]} for h in history]
    messages = [{"role": "system", "content": system_message}] + history
    
    # 2. Initial Call
    response = openai.chat.completions.create(model=MODEL, messages=messages, tools=tools)
    
    cities = []
    image = None

    # 3. Handle Tool Calls Loop
    while response.choices[0].finish_reason == "tool_calls":
        message = response.choices[0].message
        
        # Execute tools and capture specific data (e.g., city names)
        responses, new_cities = handle_tool_calls_and_return_cities(message)
        cities.extend(new_cities)
        
        # Update conversation
        messages.append(message)
        messages.extend(responses)
        
        # Follow-up Call
        response = openai.chat.completions.create(model=MODEL, messages=messages, tools=tools)

    # 4. Final Text Response
    reply = response.choices[0].message.content
    history += [{"role": "assistant", "content": reply}]

    # 5. Generate Audio (Voice)
    voice = talker(reply)

    # 6. Generate Image (Visual) - triggered if a city was queried
    if cities:
        image = artist(cities[0]) # Generate image for the first city mentioned
    
    return history, voice, image
```

---

## 4. Helper: Handling Tools & Extracting Context

This helper function executes the tools and **extracts metadata** (like the `destination_city`) needed for the side-effects (image generation).

```Python
def handle_tool_calls_and_return_cities(message):
    responses = []
    cities = []
    
    for tool_call in message.tool_calls:
        if tool_call.function.name == "get_ticket_price":
            # Parse arguments
            arguments = json.loads(tool_call.function.arguments)
            city = arguments.get('destination_city')
            cities.append(city)
            
            # Execute logic
            price_details = get_ticket_price(city)
            
            responses.append({
                "role": "tool",
                "content": price_details,
                "tool_call_id": tool_call.id
            })
            
    return responses, cities
```

---
