# OpenUI + Ollama Local Setup

A minimal reproducible setup for running OpenUI with Ollama using the official OpenUI CLI workflow.

This repository demonstrates:
- local OpenUI setup,
- Ollama integration,
- configurable model selection,
- and practical troubleshooting notes from real testing.

---

## Tested Models

### Local Models

| Model | Notes |
|---|---|
| `qwen2.5-coder:14b` | Good balance between performance and output quality |
| `gpt-oss:20b` | More stable UI generation but significantly slower on 16GB RAM |
| `ministral-3:3b` | Frequently produced malformed UI output |
| `phi4-mini:3.8b` | Inconsistent structured generation |
| `gemma4:e2b` | Partial success but unstable for larger layouts |

### Cloud Models

The following Ollama cloud-hosted models also worked during testing:

- `nemotron-3-super:cloud`
- `qwen3-next:80b-cloud`
- `gemma4:31b-cloud`

> Some cloud-hosted models may require subscriptions or gated access.

---

# Prerequisites

Install:

- [Node.js](https://nodejs.org/)
- [Ollama](https://ollama.com/download)

Recommended:
- 16GB+ RAM for larger local models

---

# 1. Pull an Ollama Model

Example:

```bash
ollama run qwen2.5-coder:14b
```

Verify installed models:

```bash
ollama list
```

---

# 2. Create the OpenUI App

```bash
npx @openuidev/cli@latest create --name genui-chat-app
cd genui-chat-app
```

---

# 3. Create the `.env` File

Linux/macOS:

```bash
touch .env
```

Windows PowerShell:

```powershell
New-Item .env -ItemType File
```

Add:

```env
OPENAI_BASE_URL=http://localhost:11434/v1
OPENAI_API_KEY=ollama
MODEL=qwen2.5-coder:14b
```

---

# 4. Update the Model Variable

Open:

```txt
src/app/api/chat/route.ts
```

Replace:

```ts
model: "gpt-5.4",
```

with:

```ts
model: process.env.MODEL || "gpt-5.4",
```

This allows model switching through environment variables.

---

# 5. Start the Development Server

```bash
npm run dev
```

Open:

```txt
http://localhost:3000
```

If everything is configured correctly, OpenUI should now be running locally with Ollama.

---

# Common Issues

## `404 model not found`

Check installed models:

```bash
ollama list
```

Update the `MODEL` value inside `.env`.

---

## `403 subscription required`

Some Ollama cloud-hosted models require subscriptions or gated access.

Try:
- another cloud model,
- or a local model instead.

---

## Blank Screen or Broken UI

Smaller local models may generate malformed `openui-lang` output.

Possible fixes:
- increase context length,
- retry generation,
- use a stronger model.

---

## Increasing Context Length

Windows PowerShell example:

```powershell
setx OLLAMA_CONTEXT_LENGTH 4096
```

Restart the terminal after changing the value.

> Higher context lengths increase RAM usage significantly.

---

# Notes

- Generative UI workloads are much more demanding than normal text chat.
- Larger models generally produced more stable component trees during testing.
- Smaller models often struggled with structured UI generation.

---

# Companion Article

This repository accompanies the OpenUI + Ollama setup tutorial and troubleshooting guide.