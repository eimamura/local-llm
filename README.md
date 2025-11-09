# Local LLM Agent Starter (LangChain v1)

This repository bootstraps a hands-on notebook for experimenting with a LangChain v1 agent that talks to a local LLM runtime (tested with [Ollama](https://ollama.ai), but easily swappable for other servers such as llama.cpp or LM Studio).

## Requirements

- Python 3.11+ (recommended for LangChain v1)
- A local LLM runtime (defaults to Ollama listening on `http://localhost:11434`)
- [`uv`](https://github.com/astral-sh/uv) and `git`

> **Note:** Dependencies now target the LangChain `1.x` release line (with `langchain-community` tracking the latest stable `0.3+` builds). Run `uv sync` after pulling to make sure your environment picks up the new major version.

## Quick start

```bash
git clone <this-repo>
cd local-llm
curl -LsSf https://astral.sh/uv/install.sh | sh   # skip if uv is already installed
uv venv                                          # creates .venv locally
source .venv/bin/activate
uv sync                                          # installs deps from pyproject.toml
```

If you prefer not to activate the environment manually, you can run commands with `uv run <cmd>` (for example, `uv run jupyter lab ...`).

### Configure your local model

1. Install/pull a model inside your runtime (example for Ollama):
   ```bash
   ollama pull llama3
   ollama serve   # keep running in a separate terminal
   ```
2. Copy `.env.example` to `.env` (or create `.env`) and set:
   ```text
   MODEL_NAME=llama3
   OLLAMA_BASE_URL=http://localhost:11434
   ```
   The notebook now relies on the standalone `langchain-ollama` integration, so as long as your Ollama server is reachable over HTTP the agent can connect without any extra glue code.
3. Confirm your runtime/model advertises tool-calling support. Ollama images that expose OpenAI-style function calling (e.g., `llama3.1`, `llama3.1:8b`, Mistral-function variants, etc.) work best. Override `SYSTEM_PROMPT` in `.env` if you want to tweak the agent instructions globally.

### Launch the notebook

```bash
uv run jupyter lab notebooks/local_llm_agent.ipynb
```

The notebook walks through:
1. Ensuring dependencies are installed.
2. Loading environment variables (via `python-dotenv`).
3. Building a LangChain 1.x tool-calling agent graph via `create_agent` (requires tool-enabled models).
4. Attaching a sample local tool (system info & math scratchpad) to showcase function-calling.

## Git workflow

This repo is initialized but uncommitted to keep history clean. Typical flow:

```bash
git init
git add .
git commit -m "feat: bootstrap local llm agent notebook"
```

## Troubleshooting

- **Model not found**: double-check `MODEL_NAME` matches what your runtime exposes.
- **Connection refused**: ensure your local server is running and `OLLAMA_BASE_URL` points to it.
- **LangChain compatibility**: stick to the pinned LangChain packages declared in `pyproject.toml`, and use a model/runtime that advertises tool-call support—`llama3` base images will throw `does not support tools`, so switch to something like `llama3.1` or another tool-aware build.

## Next steps

- Add real tools (filesystem, search, vector stores) and wire them into the agent.
- Swap `ChatOllama` with another `langchain_community` LLM wrapper that targets your runtime.
- Wrap the notebook logic inside a CLI or FastAPI service for automation.
