# AI Agent

## node 24

```bash
curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.40.4/install.sh | bash
\. "$HOME/.nvm/nvm.sh"
nvm install 24
node -v
corepack enable pnpm
pnpm -v
pnpm setup
source ~/.bashrc
```

## Google workspace CLI (to allow Google files/drives/services)

```bash
pnpm install -g @googleworkspace/cli
```

## Popular choices (both offline and online)

```bash
# opencode
pnpm install -g opencode-ai

# kilo (uses ~/.config/opencode/opencode.json)
pnpm install -g @kilocode/cli

# Claude Code (curl -fsSL https://claude.ai/install.sh | bash)
pnpm install -g @anthropic-ai/claude-code

# Gemini CLI
pnpm install -g @google/gemini-cli

# OpenAI Codex
pnpm i -g @openai/codex
```

## Kilo

Install `npm install -g @kilocode/cli`
Create `mkdir -p ~/.config/kilo && vi ~/.config/kilo/opencode.json`
Copy opencode json `cp ~/.config/opencode/opencode.json ~/.config/kilo`
Copy auth `cp ~/.onfig/opencode/auth.json ~/.config/kilo/`

```
{
  "$schema": "https://opencode.ai/config.json",
  "provider": {
    "ollama": {
      "npm": "@ai-sdk/openai-compatible",
      "name": "Ollama",
      "options": {
        "baseURL": "http://localhost:11434/v1"
      },
      "models": {
        "gpt-oss": {
          "name": "gpt-oss"
        }
      }
    }
  }
}
```

## OpenCode + Ollama + gpt-oss

Install `curl -fsSL https://opencode.ai/install | bash`
Create `mkdir -p ~/.config/opencode && ~/.config/opencode/opencode.json`

```
{
  "$schema": "https://opencode.ai/config.json",
  "provider": {
    "ollama": {
      "npm": "@ai-sdk/openai-compatible",
      "name": "Ollama",
      "options": {
        "baseURL": "http://localhost:11434/v1"
      },
      "models": {
        "gpt-oss": {
          "name": "gpt-oss"
        }
      }
    }
  }
}
```

## Claude Code + Ollama + gpt-oss

Install `curl -fsSL https://claude.ai/install.sh | bash`
Create `claude-ollama.sh`

```bash
#!/bin/bash
DEFAULT_OLLAMA_HOST="localhost"
DEFAULT_MODEL="gpt-oss"
DEFAULT_ANTHROPIC_BASE_URL=$DEFAULT_OLLAMA_HOST
if [ -z "$1" ]; then
    ANTHROPIC_AUTH_TOKEN=ollama ANTHROPIC_BASE_URL=$DEFAULT_ANTHROPIC_BASE_URL claude --model "$DEFAULT_MODEL"
else
    ANTHROPIC_AUTH_TOKEN=ollama ANTHROPIC_BASE_URL=$DEFAULT_ANTHROPIC_BASE_URL claude --model "$@"
fi
```

## OpenAI Codex + Ollama + gps-oss

Install `npm i -g @openai/codex` and then run `codex --oss -m gpt-oss:20b` or create `~/.codex/config.toml`

```bash
model = "gpt-oss:20b"
model_provider = "ollama"

[model_providers.ollama]
name = "Ollama"
base_url = "https://ollama.com/v1"
env_key = "<<OLLAMA_API_KEY>>"
```

## Cline

After installing node 24, install `npm install -g cline`

## using Qwen3.5 35B-A3b FP8 vllm w/ nvidia (<100 GB)

```
services:
  vllm-qwen:
    image: vllm/vllm-openai:nightly
    container_name: vllm-qwen3.5
    runtime: nvidia
    ports:
      - "11435:8000"
    volumes:
      - ~/.cache/huggingface:/root/.cache/huggingface
    environment:
      - HF_TOKEN=${HF_TOKEN}
    ipc: host
    deploy:
      resources:
        reservations:
          devices:
            - driver: nvidia
              count: all
              capabilities: [gpu]
    # The actual model launch command
    command: >
      --model Qwen/Qwen3.5-35B-A3B-FP8
      --kv-cache-dtype fp8_e4m3
      --max-model-len 131072
      --trust-remote-code
      --reasoning-parser qwen3
    restart: unless-stopped
```
