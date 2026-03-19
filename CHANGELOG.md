# Change Log

All notable changes to the "OpenAIPy Fragments" extension will be documented in this file. The format is based on [Keep a Changelog](https://keepachangelog.com/en/1.0.0/), and this project adheres to [Semantic Versioning](https://semver.org/spec/v2.0.0.html).

## [2.1.0] - 2026-03-19

### Added
- **Structured Outputs** (`oai-structured`): Output validado con Pydantic via `client.beta.chat.completions.parse`.
- **Vision / Multimodal** (`oai-vision`, `oai-vision-b64`): Análisis de imágenes via URL y via archivo local en base64.
- **Tool Calling** (`oai-tools`): Definición de funciones, llamada y manejo de `tool_calls` en la respuesta.
- **Error Handling** (`oai-try`): Try/except con errores tipados del SDK (`RateLimitError`, `AuthenticationError`, `APITimeoutError`, `APIConnectionError`, `BadRequestError`).
- **Retry automático** (`oai-retry`): Wrapper con exponential backoff para rate limits y errores de red.
- **dotenv boilerplate** (`oai-dotenv`): Archivo `.env` base con las keys de los proveedores principales.
- **Cliente con dotenv** (`oai-init-dotenv`): Setup de cliente OpenAI con `load_dotenv()`.
- **Responses API** (`oai-response`, `oai-response-thread`): Nueva API recomendada por OpenAI con estado server-side via `previous_response_id`.
- **Reasoning effort** (`oai-reasoning`): Parámetro `reasoning_effort` para `gpt-5.4` y `o3`.
- **Async client** (`oai-init-async`, `oai-helper-async`): Cliente `AsyncOpenAI` y helper async.
- **xAI (Grok)**: Snippets de model ID y base URL para Grok 3.
- **Base URL shortcuts** (`oai-url-groq`, `oai-url-deepseek`, `oai-url-xai`, `oai-url-ollama`).

### Changed
- Modelos actualizados a la lineup actual (Mar 2026): `gpt-5.4`, `gpt-5.4-mini`, `o3`, `o3-mini`.
- Lista de modelos recortada a los esenciales — removidos modelos legacy y variantes de bajo uso.
- Modelos de terceros simplificados: un modelo representativo por proveedor.

### Removed
- Snippets de modelos deprecados: `o1-preview`, `o1-mini`, `gpt-4o` (movido a legacy), `llama3-70b-8192`, `mixtral-8x7b-32768`.
- Variantes de modelos de poco uso: `gpt-5.4-nano`, `gpt-5.4-pro`, `gpt-5.3-codex`, `gpt-5.2`, `gpt-4.1-*`.

---

## [2.0.0] - 2026-03-18

### Added
- **Total Rewrite**: Migrated all snippets to support **OpenAI Python SDK v1.0+**.
- **New Prefix System**: Standardized all triggers with the `oai-` prefix for faster access.
- **Modern Models**: Added support for `gpt-4o`, `gpt-4o-mini`, and reasoning models (`o1-preview`, `o1-mini`).
- **Open-Source & Local Support**: Added snippets for OpenAI-compatible APIs like **Groq**, **Ollama**, and **DeepSeek**.
- **New Capabilities**: Included snippets for:
  - **DALL-E 3** (Image generation).
  - **Whisper** (Audio transcription).
  - **Embeddings** (text-embedding-3-small/large).
  - **Streaming** and **JSON Mode** responses.
- **Maintenance**: Project now maintained by **Mango Estudio Web**.

### Changed
- Deprecated and removed all legacy snippets (v0.x) as they are no longer compatible with current SDKs.
- Improved snippet descriptions and tab-stops for better DX.

### Fixed
- Fixed the long-standing issue of outdated syntax (Goodbye `openai.ChatCompletion.create`).

---

## [1.0.0] - 2023-05-26

### Added
- Added the initial list of snippets for triggers and fragments.
- Included snippets for text in the OpenAI Python library.

### Future Additions (Pending)
- Will add snippets for working with images in the OpenAI Python library.
- Will add snippets for working with audio in the OpenAI Python library.
- More snippets related to other functionalities of the OpenAI Python library will be included.