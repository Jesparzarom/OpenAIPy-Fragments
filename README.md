# OpenAIPy Fragments 🚀

[![Version](https://img.shields.io/visual-studio-marketplace/v/Jesparzarom.openaipy-fragments.svg?label=Version&logo=visual-studio-code)](https://marketplace.visualstudio.com/items?itemName=Jesparzarom.openaipy-fragments)
[![Installs](https://img.shields.io/visual-studio-marketplace/i/Jesparzarom.openaipy-fragments.svg?label=Installs&logo=visual-studio-code)](https://marketplace.visualstudio.com/items?itemName=Jesparzarom.openaipy-fragments)
[![License](https://img.shields.io/badge/license-MIT-blue.svg)](LICENSE)

**OpenAIPy Fragments** is a comprehensive collection of Python snippets for the **OpenAI Python SDK (v1.0+)**. Built for speed and 2026 coverage: latest GPT-5.x models, the new Responses API, structured outputs, vision, tool calling, and compatible third-party providers like **Groq, DeepSeek, xAI, and Ollama**.

> **v2.1.0 — Major update.** This version breaks completely from the v1 legacy API (`openai.ChatCompletion.create`, `openai.Completion.create`, verbose parameter snippets). Everything is now aligned with the current OpenAI SDK and model lineup (Mar 2026). See the [Migration Guide](#-migration-guide--changelog) for a full diff.

---

## ✨ What's new in v2.1

- **Structured Outputs** — Pydantic via `beta.chat.completions.parse` (`oai-structured`)
- **Vision / Multimodal** — análisis de imagen via URL y base64 (`oai-vision`, `oai-vision-b64`)
- **Tool Calling** — definición + call + manejo de respuesta (`oai-tools`)
- **Error handling** — try/except con errores tipados del SDK (`oai-try`)
- **Retry con backoff** — wrapper con reintentos automáticos (`oai-retry`)
- **dotenv boilerplate** — `.env` base y cliente con `load_dotenv()` (`oai-dotenv`, `oai-init-dotenv`)
- **Responses API** — nueva API recomendada por OpenAI con estado server-side (`oai-response`, `oai-response-thread`)
- **Reasoning effort** — parámetro `reasoning_effort` para `gpt-5.4` y `o3` (`oai-reasoning`)
- **Async client** — `AsyncOpenAI` + helper async (`oai-init-async`, `oai-helper-async`)
- **Modelos actualizados** — GPT-5.4, o3, o3-mini; lista recortada a los esenciales
- **xAI / Grok 3** — model ID y base URL

---

## 📦 Installation

Search **"OpenAIPy Fragments"** in the VS Code Extensions panel, or install via CLI:

```bash
code --install-extension Jesparzarom.openaipy-fragments
```

**Requirements:**

```bash
pip install openai>=1.0.0
```

---

## 📝 Snippets Reference

All snippets use the `oai-` prefix. Type it in any `.py` file to trigger IntelliSense.

---

### ◼ Client Initialization

| Trigger | Description |
| :--- | :--- |
| `oai-init` | Cliente OpenAI estándar con `OPENAI_API_KEY` desde env |
| `oai-init-dotenv` | Cliente OpenAI con `load_dotenv()` |
| `oai-init-compat` | Cliente para APIs compatibles — Groq, DeepSeek, xAI, Ollama |
| `oai-init-async` | Cliente `AsyncOpenAI` para contextos async/await |
| `oai-dotenv` | Boilerplate de archivo `.env` con las keys de los proveedores |

**`oai-init`**
```python
import os
from openai import OpenAI

client = OpenAI(api_key=os.environ.get('OPENAI_API_KEY'))
```

**`oai-init-dotenv`**
```python
import os
from openai import OpenAI
from dotenv import load_dotenv

load_dotenv()
client = OpenAI(api_key=os.environ.get('OPENAI_API_KEY'))
```

**`oai-init-compat`** — reemplaza `base_url` y `api_key` según el proveedor
```python
import os
from openai import OpenAI

client = OpenAI(
    base_url="https://api.groq.com/openai/v1",
    api_key=os.environ.get('GROQ_API_KEY')
)
```

**`oai-dotenv`** — para archivo `.env`
```
OPENAI_API_KEY=sk-your-key-here
# GROQ_API_KEY=
# DEEPSEEK_API_KEY=
# XAI_API_KEY=
```

---

### ◼ Chat Completions

| Trigger | Description |
| :--- | :--- |
| `oai-chat` | Completion básico con roles system / user |
| `oai-stream` | Streaming con for-loop y `flush=True` |
| `oai-json` | JSON mode con `response_format` + parse automático |
| `oai-reasoning` | Completion con `reasoning_effort` (none / low / medium / high / xhigh) |
| `oai-messages` | Array de mensajes reutilizable |

**`oai-chat`**
```python
response = client.chat.completions.create(
    model="gpt-5.4-mini",
    messages=[
        {"role": "system", "content": "You are a helpful assistant."},
        {"role": "user", "content": "Your prompt here"}
    ]
)
print(response.choices[0].message.content)
```

**`oai-stream`**
```python
stream = client.chat.completions.create(
    model="gpt-5.4-mini",
    messages=[{"role": "user", "content": "Your prompt here"}],
    stream=True,
)
for chunk in stream:
    if chunk.choices[0].delta.content is not None:
        print(chunk.choices[0].delta.content, end="", flush=True)
```

**`oai-json`**
```python
response = client.chat.completions.create(
    model="gpt-5.4-mini",
    response_format={"type": "json_object"},
    messages=[
        {"role": "system", "content": "You are a helpful assistant that responds only in JSON."},
        {"role": "user", "content": "Your prompt asking for JSON"}
    ]
)
import json
data = json.loads(response.choices[0].message.content)
print(data)
```

**`oai-reasoning`** — para `gpt-5.4`, `o3`, `o3-mini`
```python
response = client.chat.completions.create(
    model="gpt-5.4",
    reasoning_effort="high",   # none | low | medium | high | xhigh
    messages=[
        {"role": "user", "content": "Your prompt here"}
    ]
)
print(response.choices[0].message.content)
```

---

### ◼ Structured Outputs

Reemplaza JSON mode. La respuesta es un objeto Pydantic validado, no un string que hay que parsear.

| Trigger | Description |
| :--- | :--- |
| `oai-structured` | Output estructurado con validación Pydantic |

**`oai-structured`**
```python
from pydantic import BaseModel

class ResponseModel(BaseModel):
    field: str

completion = client.beta.chat.completions.parse(
    model="gpt-5.4-mini",
    messages=[
        {"role": "system", "content": "Extract the information."},
        {"role": "user", "content": "Input text"},
    ],
    response_format=ResponseModel,
)
result = completion.choices[0].message.parsed
print(result)
```

---

### ◼ Vision / Multimodal

| Trigger | Description |
| :--- | :--- |
| `oai-vision` | Análisis de imagen via URL |
| `oai-vision-b64` | Análisis de imagen local en base64 |

**`oai-vision`**
```python
response = client.chat.completions.create(
    model="gpt-5.4-mini",
    messages=[
        {
            "role": "user",
            "content": [
                {"type": "text", "text": "What do you see in this image?"},
                {
                    "type": "image_url",
                    "image_url": {"url": "https://example.com/image.jpg"},
                },
            ],
        }
    ],
)
print(response.choices[0].message.content)
```

**`oai-vision-b64`** — para archivos locales
```python
import base64

with open("image.jpg", "rb") as f:
    image_data = base64.b64encode(f.read()).decode("utf-8")

response = client.chat.completions.create(
    model="gpt-5.4-mini",
    messages=[
        {
            "role": "user",
            "content": [
                {"type": "text", "text": "What do you see in this image?"},
                {
                    "type": "image_url",
                    "image_url": {"url": f"data:image/jpeg;base64,{image_data}"},
                },
            ],
        }
    ],
)
print(response.choices[0].message.content)
```

---

### ◼ Tool Calling

| Trigger | Description |
| :--- | :--- |
| `oai-tools` | Definición de funciones + call + manejo de `tool_calls` |

**`oai-tools`**
```python
import json

tools = [
    {
        "type": "function",
        "function": {
            "name": "function_name",
            "description": "What this function does",
            "parameters": {
                "type": "object",
                "properties": {
                    "param": {
                        "type": "string",
                        "description": "Parameter description"
                    }
                },
                "required": ["param"],
            },
        },
    }
]

response = client.chat.completions.create(
    model="gpt-5.4-mini",
    messages=[{"role": "user", "content": "Prompt"}],
    tools=tools,
    tool_choice="auto",
)

# Handle tool call
message = response.choices[0].message
if message.tool_calls:
    tool_call = message.tool_calls[0]
    args = json.loads(tool_call.function.arguments)
    print(f"Tool: {tool_call.function.name}, Args: {args}")
```

---

### ◼ Error Handling

| Trigger | Description |
| :--- | :--- |
| `oai-try` | Try/except con los 5 errores tipados del SDK |
| `oai-retry` | Wrapper con retry automático y exponential backoff |

**`oai-try`**
```python
from openai import (
    APIConnectionError,
    APITimeoutError,
    AuthenticationError,
    BadRequestError,
    RateLimitError,
)

try:
    response = client.chat.completions.create(
        model="gpt-5.4-mini",
        messages=[{"role": "user", "content": "Your prompt"}]
    )
    print(response.choices[0].message.content)

except RateLimitError as e:
    print(f"Rate limit hit: {e}")
except AuthenticationError as e:
    print(f"Auth error — check your API key: {e}")
except APITimeoutError as e:
    print(f"Request timed out: {e}")
except APIConnectionError as e:
    print(f"Connection error: {e}")
except BadRequestError as e:
    print(f"Bad request: {e}")
```

**`oai-retry`** — exponential backoff: 1s → 2s → 4s
```python
import time
from openai import RateLimitError, APIConnectionError, APITimeoutError

def call_with_retry(prompt: str, model: str = "gpt-5.4-mini", max_retries: int = 3) -> str:
    for attempt in range(max_retries):
        try:
            response = client.chat.completions.create(
                model=model,
                messages=[{"role": "user", "content": prompt}]
            )
            return response.choices[0].message.content
        except RateLimitError:
            wait = 2 ** attempt
            print(f"Rate limit — retrying in {wait}s...")
            time.sleep(wait)
        except (APIConnectionError, APITimeoutError) as e:
            print(f"Network error on attempt {attempt + 1}: {e}")
            if attempt == max_retries - 1:
                raise
            time.sleep(1)
    raise RuntimeError("Max retries exceeded")
```

---

### ◼ Responses API

La nueva API recomendada por OpenAI. Reemplaza Assistants. Maneja el estado de conversación del lado del servidor vía `previous_response_id`.

| Trigger | Description |
| :--- | :--- |
| `oai-response` | Llamada básica a la Responses API |
| `oai-response-thread` | Conversación multi-turno con estado server-side |

**`oai-response`**
```python
response = client.responses.create(
    model="gpt-5.4-mini",
    input="Your prompt here",
)
print(response.output_text)
```

**`oai-response-thread`**
```python
# First turn
r1 = client.responses.create(
    model="gpt-5.4-mini",
    input="First message",
)

# Subsequent turns — state is managed server-side
r2 = client.responses.create(
    model="gpt-5.4-mini",
    input="Follow-up message",
    previous_response_id=r1.id,
)
print(r2.output_text)
```

---

### ◼ Helper Functions

| Trigger | Description |
| :--- | :--- |
| `oai-helper-completion` | Función `get_completion(prompt)` síncrona |
| `oai-helper-async` | Función `get_completion(prompt)` async |
| `oai-helper-system` | Función `chat(user, system)` con system message configurable |

**`oai-helper-completion`**
```python
def get_completion(prompt: str, model: str = "gpt-5.4-mini") -> str:
    response = client.chat.completions.create(
        model=model,
        messages=[{"role": "user", "content": prompt}]
    )
    return response.choices[0].message.content
```

**`oai-helper-system`**
```python
def chat(user_msg: str, system_msg: str = "You are a helpful assistant.", model: str = "gpt-5.4-mini") -> str:
    response = client.chat.completions.create(
        model=model,
        messages=[
            {"role": "system", "content": system_msg},
            {"role": "user", "content": user_msg},
        ]
    )
    return response.choices[0].message.content
```

---

### ◼ Embeddings, Imágenes y Audio

| Trigger | Description |
| :--- | :--- |
| `oai-embed` | Vector embeddings con `text-embedding-3-small/large` |
| `oai-image` | Generación de imagen con `gpt-image-1` |
| `oai-transcribe` | Transcripción de audio con `whisper-1` |
| `oai-tts` | Text-to-speech con `gpt-audio-1.5` |

**`oai-embed`**
```python
response = client.embeddings.create(
    input="Your text here",
    model="text-embedding-3-small"
)
vector = response.data[0].embedding
print(f"Dimensions: {len(vector)}")
```

**`oai-image`**
```python
response = client.images.generate(
    model="gpt-image-1",
    prompt="A futuristic city at sunset",
    size="1024x1024",    # 1024x1024 | 1792x1024 | 1024x1792
    quality="standard",  # standard | hd
    n=1,
)
image_url = response.data[0].url
print(image_url)
```

**`oai-transcribe`**
```python
with open("audio.mp3", "rb") as audio_file:
    transcript = client.audio.transcriptions.create(
        model="whisper-1",
        file=audio_file
    )
print(transcript.text)
```

**`oai-tts`**
```python
response = client.audio.speech.create(
    model="gpt-audio-1.5",
    voice="nova",   # alloy | echo | fable | onyx | nova | shimmer
    input="Text to convert to speech"
)
response.stream_to_file("output.mp3")
```

---

### ◼ Model Shortcuts — OpenAI

Todos los model shortcuts insertan únicamente el string del modelo.

| Trigger | Model ID | Notas |
| :--- | :--- | :--- |
| `oai-model-gpt54` | `"gpt-5.4"` | Flagship — razonamiento + coding |
| `oai-model-gpt54-mini` | `"gpt-5.4-mini"` | Rápido, bajo costo |
| `oai-model-o3` | `"o3"` | Razonamiento profundo |
| `oai-model-o3-mini` | `"o3-mini"` | Razonamiento costo-efectivo |
| `oai-model-image` | `"gpt-image-1"` | Generación de imágenes |
| `oai-model-whisper` | `"whisper-1"` | Transcripción de audio |
| `oai-model-emb-small` | `"text-embedding-3-small"` | Embeddings 1536d |
| `oai-model-emb-large` | `"text-embedding-3-large"` | Embeddings 3072d |

---

### ◼ Model Shortcuts — Proveedores Compatibles

Para usarlos, inicializar el cliente con `oai-init-compat` y el `base_url` correspondiente.

| Trigger | Model ID | Proveedor |
| :--- | :--- | :--- |
| `oai-model-groq-llama4` | `"meta-llama/llama-4-maverick-17b-128e-instruct"` | Groq |
| `oai-model-groq-fast` | `"llama-3.1-8b-instant"` | Groq — máxima velocidad |
| `oai-model-deepseek` | `"deepseek-chat"` | DeepSeek V3 |
| `oai-model-deepseek-r1` | `"deepseek-reasoner"` | DeepSeek R1 — CoT |
| `oai-model-grok` | `"grok-3"` | xAI |
| `oai-model-ollama` | `"llama3"` | Ollama local |

---

### ◼ Base URL Shortcuts

| Trigger | URL |
| :--- | :--- |
| `oai-url-groq` | `"https://api.groq.com/openai/v1"` |
| `oai-url-deepseek` | `"https://api.deepseek.com"` |
| `oai-url-xai` | `"https://api.x.ai/v1"` |
| `oai-url-ollama` | `"http://localhost:11434/v1"` |

---

## 🔄 Migration Guide & Changelog

### v1 → v2: Lo que cambió

Esta extensión comenzó como un set de snippets para el SDK legacy de OpenAI (pre-1.0). La v2 es una reescritura completa. Si venías usando v1, esto es lo que cambió:

#### Imports y cliente

| v1 (legacy) | v2 (actual) |
| :--- | :--- |
| `import openai` | `from openai import OpenAI` |
| `openai.api_key = "..."` | `client = OpenAI(api_key=...)` |
| `from dotenv import load_dotenv` (snippet dedicado) | `oai-init-dotenv` con `load_dotenv()` integrado |
| `openai.organization = "..."` | parámetro `organization=` en el constructor de `OpenAI()` |

#### Métodos de llamada

| v1 (legacy) | v2 (actual) |
| :--- | :--- |
| `openai.ChatCompletion.create(...)` | `client.chat.completions.create(...)` |
| `openai.Completion.create(...)` | eliminado — usar Chat Completions |
| `openai.Edit.create(...)` | eliminado — endpoint deprecado por OpenAI |
| `openai.Model.list()` | `client.models.list()` |
| `openai.Model.retrieve()` | `client.models.retrieve()` |

#### Acceso a la respuesta

| v1 (legacy) | v2 (actual) |
| :--- | :--- |
| `response.choices[0].message['content']` | `response.choices[0].message.content` |
| `response.choices[0].text` | eliminado (era de Text Completions) |

#### Prefijos de snippets

| v1 (legacy) | v2 (actual) |
| :--- | :--- |
| `import setup init basic` | `oai-init` |
| `def get chat_completion() [base]` | `oai-chat` / `oai-helper-completion` |
| `def get text_completion() [full]` | eliminado (Text Completions deprecado) |
| `def get text_edit() [full]` | eliminado (Edit API deprecada) |
| `gpt-4 [chat model]` | `oai-model-gpt54` |
| `gpt-3.5-turbo [chat model]` | `oai-model-gpt54-mini` (equivalente moderno) |
| `text-davinci-003 [text model]` | eliminado |
| `model= [parameter]` / `temperature= [parameter]` / etc. | eliminados — parámetros inline sin snippet dedicado |

#### Modelos eliminados (end-of-life)

`text-davinci-003`, `text-davinci-002`, `text-curie-001`, `text-babbage-001`, `text-ada-001`, `text-davinci-edit-001`, `code-davinci-edit-001`, `gpt-4-0314`, `gpt-4-32k`, `gpt-4-32k-0314`, `gpt-3.5-turbo-0301`

---

### Changelog

#### v2.1.0 (Mar 2026)
Ver [CHANGELOG.md](CHANGELOG.md) para el detalle completo.

#### v2.0.0 (Mar 2026)
- Reescritura completa al SDK v1.0+, nuevo sistema de prefijos `oai-`

#### v1.0.0 (May 2023)
- Release inicial con snippets para SDK legacy

---

## 📄 License

MIT © [Jesparzarom](https://github.com/Jesparzarom)