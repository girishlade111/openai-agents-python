# OpenAI Agents SDK (Python)

> My maintained copy of the OpenAI Agents SDK for Python, with a small fix on top of upstream: filtered `instructions` are now passed through to the `on_llm_start` agent hook.

The OpenAI Agents SDK is a lightweight yet powerful framework for building multi-agent workflows. It is provider-agnostic, supporting the OpenAI Responses and Chat Completions APIs as well as 100+ other LLMs.

<img src="https://cdn.openai.com/API/docs/images/orchestration.png" alt="Image of the Agents Tracing UI" style="max-height: 803px;">

Upstream project: [openai/openai-agents-python](https://github.com/openai/openai-agents-python) (Apache 2.0 / MIT). This repository keeps the full SDK (v0.3.3) plus a custom patch:

- **fix: Pass filtered instructions to `on_llm_start` agent hook** — guarantees the instructions string seen by the hook is the filtered one, so hooks that log, redact, or mutate instructions behave consistently.

## Features

- **Agents** — LLMs configured with instructions, tools, guardrails, and handoffs
- **Handoffs** — a specialized tool call for transferring control between agents
- **Guardrails** — configurable safety checks for input and output validation
- **Sessions** — automatic conversation history management across agent runs
- **Tracing** — built-in tracking of agent runs for debugging and optimization
- **MCP support** — use Model Context Protocol servers as tools
- **Realtime agents** — speech-to-speech voice agent support
- **Provider-agnostic** — works with OpenAI plus 100+ other model providers via LiteLLM

## Tech Stack

- **Language:** Python 3.9+
- **Build / packaging:** `pyproject.toml`, `uv.lock` (uv-managed)
- **Key dependencies:** `openai`, `pydantic`, `griffe`, `mcp`, `openai-realtime`, `typeguard`, `requests`
- **Docs:** MkDocs (see `mkdocs.yml`)
- **Lint/format:** Ruff, Mypy, Prettier (see `Makefile`)

## Quick Start

### Install

```bash
pip install openai-agents
```

or from this repo with uv:

```bash
uv sync
```

### Minimal example

```python
import asyncio
from agents import Agent, Runner

agent = Agent(
    name="Assistant",
    instructions="You are a helpful assistant.",
)

async def main():
    result = await Runner.run(agent, "What is the capital of France?")
    print(result.final_output)

asyncio.run(main())
```

Set your key first: `export OPENAI_API_KEY=sk-...`

### Run the examples

```bash
cd examples
python basic/hello_world.py
```

### Run tests

```bash
make tests
# or
pytest tests
```

## Project Structure

```
├── src/agents/        # SDK source (agents, runner, sessions, tracing, MCP, realtime)
├── examples/          # Runnable examples (basic, agent_patterns, handoffs, mcp, model_providers, ...)
├── tests/             # Pytest suite
├── docs/              # MkDocs documentation
├── pyproject.toml     # Package metadata and dependencies
├── uv.lock            # Locked dependency versions
└── Makefile           # make targets: tests, lint, format, docs
```

## Deployment Notes

This is a Python library, not a web app — there is nothing to deploy. Publish to PyPI with `uv build && uv publish` (or `hatch build` + `twine upload`) if you want a distributable built from this fork.

## License

MIT — see `LICENSE`. Upstream © OpenAI; local patch © Girish Lade.

---

Built by Girish Lade — https://ladestack.in
