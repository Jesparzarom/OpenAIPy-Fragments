=========================================
ARCHIVO: README.md (VERSIÓN DETALLADA)
=========================================
# OpenAIPy Fragments 🚀

[![Version](https://img.shields.io/visual-studio-marketplace/v/Jesparzarom.openaipy-fragments.svg?label=Version&logo=visual-studio-code)](https://marketplace.visualstudio.com/items?itemName=Jesparzarom.openaipy-fragments)
[![Installs](https://img.shields.io/visual-studio-marketplace/i/Jesparzarom.openaipy-fragments.svg?label=Installs&logo=visual-studio-code)](https://marketplace.visualstudio.com/items?itemName=Jesparzarom.openaipy-fragments)
[![License](https://img.shields.io/badge/license-MIT-blue.svg)](LICENSE)

**OpenAIPy Fragments** is a comprehensive collection of Python snippets for **OpenAI v1.0+**. Designed for high-speed development, it supports the latest GPT models and compatible APIs like **Groq, Ollama, and DeepSeek**.

> **Important:** This version (2.0.0) is a total rewrite. Legacy triggers have been replaced by the new `oai-` prefix system for better performance and organization.

---

## ✨ Features

- **Full OpenAI v1.0+ Support**: No more deprecated `ChatCompletion.create` errors.
- **Unified Prefix**: All snippets are grouped under the `oai-` trigger.
- **Multi-Provider Ready**: Easily switch between OpenAI, Groq, or local Ollama instances.
- **Modern Models**: Built-in shortcuts for `gpt-4o`, `o1-mini`, `llama3`, and more.
- **Multimedia Support**: Snippets for DALL-E 3, Whisper, and Embeddings.

---

## 📝 Usage & Shortcuts

### **◼ Client Initialization**
| TRIGGER | DESCRIPTION |
| :--- | :--- |
| `oai-init` | Basic setup using `os.environ.get('OPENAI_API_KEY')`. |
| `oai-init-custom` | Setup for **Groq, Ollama, or DeepSeek** (custom `base_url`). |

### **◼ Chat Completions**
| TRIGGER | DESCRIPTION |
| :--- | :--- |
| `oai-chat` | Standard Chat Completion block with system/user roles. |
| `oai-stream` | Chat Completion with real-time streaming output (for-loop). |
| `oai-json` | Forced JSON Mode response using `response_format`. |
| `oai-func` | Clean Python function wrapper for quick completions. |
| `oai-messages` | Standard array structure for conversation history. |

### **◼ Multimedia & Advanced Tools**
| TRIGGER | DESCRIPTION |
| :--- | :--- |
| `oai-image` | **DALL-E 3** image generation block. |
| `oai-audio` | **Whisper** audio-to-text transcription. |
| `oai-embed` | **Text Embeddings** generation (3-small/large). |

### **◼ Model Shortcuts**
| TRIGGER | MODEL NAME |
| :--- | :--- |
| `oai-gpt4o` | `"gpt-4o"` (Flagship model) |
| `oai-gpt4o-mini` | `"gpt-4o-mini"` (Fast & Cheap) |
| `oai-o1-mini` | `"o1-mini"` (Reasoning) |
| `oai-llama3-groq` | `"llama3-70b-8192"` (via Groq) |
| `oai-ollama-llama3` | `"llama3"` (Local via Ollama) |

---

## 🛠 Requirements

- **Python 3.7+**
- **OpenAI Library**:
```bash
pip install openai >= 1.0.0
