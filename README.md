# MLX-LM Quick Start Guide

A complete guide to installing MLX-LM, running local LLMs on Apple Silicon (M1/M2/M3/M4), generating text, chatting interactively, and exposing an OpenAI-compatible API server.

---

# Prerequisites

- macOS
- Apple Silicon (M1/M2/M3/M4)
- Python 3.10+

Verify Python:

```bash
python3 --version
```

---

# 1. Create Virtual Environment

```bash
python3 -m venv .venv
source .venv/bin/activate
```

Upgrade pip:

```bash
pip install --upgrade pip
```

---

# 2. Install MLX-LM

```bash
pip install mlx-lm
```

Verify installation:

```bash
python -c "import mlx_lm; print('MLX-LM installed successfully')"
```

---

# 3. Generate Text

Run a single prompt and get a response.

## Example: Qwen3

```bash
mlx_lm.generate \
    --model mlx-community/Qwen3-8B-4bit \
    --prompt "Explain Python multiprocessing in simple terms."
```

## Example: Llama

```bash
mlx_lm.generate \
    --model mlx-community/Llama-3.2-3B-Instruct-4bit \
    --prompt "Write a Python Singleton implementation."
```

---

# 4. Interactive Chat Mode

Launch a ChatGPT-style terminal chat.

```bash
mlx_lm.chat \
    --model mlx-community/Qwen3-8B-4bit
```

Example session:

```text
>>> Hello
>>> Explain Django ORM
>>> What is CQRS?
>>> How does sharding work?
```

Exit:

```text
Ctrl + C
```

---

# 5. Start OpenAI-Compatible API Server

MLX-LM can expose an API compatible with the OpenAI SDK.

```bash
mlx_lm.server \
    --model mlx-community/Qwen3-8B-4bit
```

Default endpoint:

```text
http://localhost:8080
```

API base URL:

```text
http://localhost:8080/v1
```

---

# 6. Test API Using cURL

## Chat Completion

```bash
curl http://localhost:8080/v1/chat/completions \
-H "Content-Type: application/json" \
-d '{
  "model": "default",
  "messages": [
    {
      "role": "user",
      "content": "Explain FastAPI"
    }
  ]
}'
```

---

## Text Completion

```bash
curl http://localhost:8080/v1/completions \
-H "Content-Type: application/json" \
-d '{
  "model": "default",
  "prompt": "Write a Python decorator"
}'
```

---

# 7. Using OpenAI Python SDK

Install OpenAI SDK:

```bash
pip install openai
```

Example:

```python
from openai import OpenAI

client = OpenAI(
    base_url="http://localhost:8080/v1",
    api_key="dummy"
)

response = client.chat.completions.create(
    model="default",
    messages=[
        {
            "role": "user",
            "content": "Explain dependency injection"
        }
    ]
)

print(response.choices[0].message.content)
```

---

# 8. Model Downloading & Caching

MLX-LM automatically downloads models from Hugging Face.

Example:

```bash
mlx_lm.chat \
    --model mlx-community/Qwen3-8B-4bit
```

First run:

- Downloads model
- Stores in local cache

Subsequent runs:

- Uses cached version
- Starts instantly

---

# 9. Useful Models

## Small Models (Fast)

### Qwen3 4B

```bash
mlx-community/Qwen3-4B-4bit
```

### Llama 3.2 3B

```bash
mlx-community/Llama-3.2-3B-Instruct-4bit
```

---

## Medium Models

### Qwen3 8B

```bash
mlx-community/Qwen3-8B-4bit
```

Recommended for:

- Coding
- Backend engineering
- System design
- General reasoning

---

## Large Models

### Qwen3 14B

```bash
mlx-community/Qwen3-14B-4bit
```

### Qwen3 32B

```bash
mlx-community/Qwen3-32B-4bit
```

---

# 10. Recommended by RAM

| Unified Memory | Suggested Model |
|---------------|------------------|
| 16 GB | Qwen3-4B |
| 24 GB | Qwen3-8B |
| 36 GB | Qwen3-14B |
| 48 GB | Qwen3-32B |
| 64 GB+ | Qwen3-32B / DeepSeek |
| 96 GB+ | DeepSeek-R1 |

---

# 11. Typical Development Workflow

## Interactive Chat

```bash
mlx_lm.chat \
    --model mlx-community/Qwen3-8B-4bit
```

## Local API Server

```bash
mlx_lm.server \
    --model mlx-community/Qwen3-8B-4bit
```

## Use From Python

```python
from openai import OpenAI

client = OpenAI(
    base_url="http://localhost:8080/v1",
    api_key="dummy"
)

response = client.chat.completions.create(
    model="default",
    messages=[
        {
            "role": "user",
            "content": "Design a scalable task queue"
        }
    ]
)

print(response.choices[0].message.content)
```

---

# 12. MLX vs Ollama

| Feature | MLX-LM | Ollama |
|----------|---------|---------|
| Apple Silicon Optimized | ✅ | ⚠️ |
| OpenAI API | ✅ | ✅ |
| Chat Mode | ✅ | ✅ |
| Model Management | ❌ | ✅ |
| Maximum Performance | ✅ | ⚠️ |
| Apple Ecosystem Integration | ✅ | ❌ |

Use MLX-LM when:

- Running on Apple Silicon
- Building custom AI applications
- Wanting maximum performance
- Using OpenAI-compatible APIs

Use Ollama when:

- You want simple model management
- You frequently switch between many models
- You rely on tools that directly integrate with Ollama

---

# Quick Commands Cheat Sheet

## Install

```bash
pip install mlx-lm
```

## Generate

```bash
mlx_lm.generate \
  --model mlx-community/Qwen3-8B-4bit \
  --prompt "Hello"
```

## Chat

```bash
mlx_lm.chat \
  --model mlx-community/Qwen3-8B-4bit
```

## Start Server

```bash
mlx_lm.server \
  --model mlx-community/Qwen3-8B-4bit
```

## API Endpoint

```text
http://localhost:8080/v1
```

## OpenAI SDK

```python
client = OpenAI(
    base_url="http://localhost:8080/v1",
    api_key="dummy"
)
```
