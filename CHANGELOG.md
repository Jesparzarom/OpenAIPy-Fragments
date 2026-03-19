# Change Log

All notable changes to the "OpenAIPy Fragments" extension will be documented in this file. The format is based on [Keep a Changelog](https://keepachangelog.com/en/1.0.0/), and this project adheres to [Semantic Versioning](https://semver.org/spec/v2.0.0.html).

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


- Initial release of OpenAIPy Fragments

## [1.0.0] - 2023-05-26

### Added

- Added the initial list of snippets for triggers and fragments.
- Included snippets for text in the OpenAI Python library.

### Future Additions (Pending)

- Will add snippets for working with images in the OpenAI Python library.
- Will add snippets for working with audio in the OpenAI Python library.
- More snippets related to other functionalities of the OpenAI Python library will be included.

Stay tuned for upcoming updates!
