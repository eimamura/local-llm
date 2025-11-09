# Local LLM Agent Starter (LangChain v1)

This repository bootstraps a hands-on notebook for experimenting with a LangChain v1 agent that talks to a local LLM runtime (tested with [Ollama](https://ollama.ai), but easily swappable for other servers such as llama.cpp or LM Studio).

## Requirements

- Python 3.11+ (recommended for LangChain v1)
- A local LLM runtime (defaults to Ollama listening on `http://localhost:11434`)
- `make`, `pip`, and `git`

## Quick start

```bash
git clone <this-repo>
cd local-llm
python -m venv .venv
source .venv/bin/activate
pip install --upgrade pip
pip install -r requirements.txt
```

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

### Launch the notebook

```bash
jupyter lab notebooks/local_llm_agent.ipynb
```

The notebook walks through:
1. Ensuring dependencies are installed.
2. Loading environment variables (via `python-dotenv`).
3. Building a LangChain v1 ReAct-style agent around `ChatOllama`.
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
- **LangChain compatibility**: stick to the pinned LangChain v1 packages in `requirements.txt`.

## Next steps

- Add real tools (filesystem, search, vector stores) and wire them into the agent.
- Swap `ChatOllama` with another `langchain_community` LLM wrapper that targets your runtime.
- Wrap the notebook logic inside a CLI or FastAPI service for automation.
