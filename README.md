# Nobli: Research at Noble Speed, Growth at Your Pace 

Nobli is a product demo built during the **Paris Research Hackathon / EHL Paris Match**, organized by **TUM.ai, Iterate, and the European Hackathon League** (https://ehl.gg/). The demo was developed by **Team BaoByte** from **June 27 to June 28, 2026**, at **Metafore Vincennes Foch, Paris, France**.  Give it a research question and it autonomously runs the full loop: idea spark, design and run experiments, iterate on the method, plot results, and write a paper. 

Beyond automation, Nobli is also designed to support researchers’ growth journey. Suggestions, reviews, and revision feedback are made transparent and visible, so the system not only makes the research workflow smoother, but also helps researchers learn by doing and improve their research capabilities over time.

## Team BaoByte
Build by Wenchao Hao · Shuo Chen · Huang Lin · Yize Sun

## Prerequisites

- **Python 3.11** and **[uv](https://docs.astral.sh/uv/)**
- **[tectonic](https://tectonic-typesetting.github.io/)** for LaTeX → PDF
  (`brew install tectonic`, or `apt install tectonic`)
- **An OpenAI API key** (any OpenAI-compatible endpoint works)
- **Node.js** (for the frontend)

## setup 

```bash
cd research-claw
bash scripts/setup_uv.sh
```

Then create `research-claw/settings.json` with your API key (this file is
git-ignored, so create it locally):

```json
{
  "agents": { "defaults": { "workspace": "./workspace", "model": "gpt-4o-mini" } },
  "provider": {
    "activeId": "openai-main",
    "instances": [
      {
        "id": "openai-main",
        "provider": "openai",
        "model_name": "gpt-4o-mini",
        "api_key": "sk-...PUT-YOUR-KEY-HERE...",
        "api_base": "https://api.openai.com/v1",
        "enabled": true
      }
    ]
  }
}
```

> Verify everything is green with `.venv/bin/python cli/main.py doctor`.

## Backend - `research-claw`
- Directly run in Terminal via CLI 
```bash 
cd research-claw
.venv/bin/python cli/main.py research --rounds 2 -p AutoBudgetCoT \
    -m "Does chain-of-thought prompting help or hurt LLM reasoning accuracy under a tight output-token budget, and can an answer-first prompt do better?"
```

This runs the autonomous pipeline end-to-end:
**survey → design & verify experiment → metric-driven iteration → figure → NeurIPS paper → compiled PDF.**
The paper lands at
`research-claw/workspace/AutoBudgetCoT/AutoBudgetCoT/main.pdf`; a pre-built
example is in
[`research-claw/examples/budget-cot-paper/`](research-claw/examples/budget-cot-paper/).

> Use `.venv/bin/python` explicitly (a shell `python` alias may point elsewhere).
> Use `--rounds 4` for a more thorough run.

- Launch the backend server
```bash
cd research-claw
uvicorn agent.services.gateway_server:app --host 127.0.0.1 --port 18790
```

## Frontend - `frontend/nobli_lovable_v1` 

```bash
cd frontend/nobli_lovable_v1
npm install   # first time only
npm run dev 
```

The dev server runs on **http://localhost:8080**. Start the backend server
above first so the UI can reach the API.

## Test Entire

This repository is configured for Entire session tracking through `.entire/`,
`.codex/`, and `.claude/`.

To verify the local Entire setup:

```sh
entire status --detailed
entire doctor
```

To confirm checkpoint search works, run a non-interactive JSON search:

```sh
entire search --json --limit 5 "repo:researcher-baobyte"
```

If `entire` is not installed or not on `PATH`, install the CLI first:

```sh
entire --help
```

Search requires authentication. If the JSON search reports an auth error, run:

```sh
entire login
```
