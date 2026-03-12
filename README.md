# Distila · Research Intelligence

**[→ Open the app]([https://your-live-link-here.com](https://abhishek-sinha-bgl.github.io/myresassist-beta/))** &nbsp;·&nbsp; No account &nbsp;·&nbsp; No server &nbsp;·&nbsp; Bring your own keys

---

![Distila research dashboard](screenshot.png)

---

## *knowledge accumulates · claims persist · contradictions surface*

Most AI research tools are amnesiac by design. Every conversation resets. Every finding evaporates. You re-establish context, re-run the same questions, re-discover the same contradictions — and call it research.

Distila is built on a different premise: that serious research is cumulative. Claims extracted today should still be there tomorrow, scored for confidence and cross-referenced against everything that came before. Contradictions shouldn't hide in your notes — they should be surfaced automatically and treated as evidence, not noise. And the things you *don't* know yet should have names, importance ratings, and a button that turns them into your next investigation thread. Research that compounds is research worth doing.

---

## Features

**Multi-phase pipeline** — A single question triggers four sequential phases: Scan → Research → Validate → Synthesize. Each phase has a distinct prompt strategy; the Cognitive Stream logs every step in real time.

**Persistent knowledge base** — Topics, claims, voids, and contradictions are stored in `localStorage` and survive across sessions. Research compounds instead of resetting.

**Multi-model consensus** — Configure one Orchestrator and any number of Workers. In Parallel mode, Workers are queried independently; their outputs are reconciled and contradictions are captured structurally, not discarded.

**Claims Vault** — Every claim extracted from a pipeline run is stored as an individual evidence unit with confidence score (0–100), status (VERIFIED / CONTESTED / WEAK / UNVERIFIED), source provider, evidence type, and full validation history.

**Contradiction detection** — Claims are cross-referenced automatically. Contradictions surface as a dedicated tab with opposing claims shown side by side. They also appear as badges in the Claims Vault and as a distinct sector in the Research Map.

**Research Map** — A polar confidence visualization of all claims for a topic. Radial distance encodes confidence: high-confidence claims orbit the centre; weak claims sit at the outer ring. Contradictions occupy a dedicated angular sector. Every node is clickable and jumps to its claim in the Vault.

**Knowledge Voids** — AI-identified gaps in the current evidence base, rated High / Medium / Low importance. Each void has a ▶ Research button that pre-loads the gap as your next research question.

**Topic Tree** — Hierarchical navigation with two views: your own category structure and an AI-detected semantic grouping. The ✨ AI Categorize button classifies all uncategorized topics in a single pass.

**Living synthesis** — The research brief for each topic is updated incrementally as new claims arrive (delta synthesis), preserving prior findings rather than overwriting them.

**URL & PDF ingestion** — Paste a URL to scan its content directly into the pipeline. Attach a PDF via the 📎 button in the input area; text is extracted and fed to the active provider.

**Seven themes** — Night, Midnight, Forest, High Contrast, Light, Sepia, Academic.

---

## Getting Started

Distila is a single HTML file. There is nothing to install.

1. Download `distila.html`
2. Open it in any modern browser
3. Go to **Settings → API Providers** and add at least one provider with an API key
4. Type a research question and press **▶ Research**

---

## Supported Providers

| Provider | Default endpoint | Notes |
|---|---|---|
| **Gemini** | `generativelanguage.googleapis.com/v1beta` | Supports `gemini-2.5-pro`, `gemini-2.0-flash`, `gemini-1.5-pro/flash` |
| **OpenAI** | `api.openai.com/v1` | `gpt-4o`, `gpt-4o-mini`, `o3-mini`, `gpt-4-turbo` |
| **Claude** | `api.anthropic.com/v1/messages` | `claude-opus-4-6`, `claude-sonnet-4-6`, `claude-haiku-4-5` |
| **Custom** | Any URL | Any OpenAI-compatible endpoint (Groq, OpenRouter, Ollama, etc.) |

The URL field auto-normalizes — you can paste a full model path and it will be stripped to the correct base.

---

## Multi-Model Setup

For best results, configure at least two providers:

- **Orchestrator** — handles synthesis, validation coordination, and fallback. One allowed.
- **Worker** — queried in parallel during the Research phase. Add as many as you have keys for.

Enable **Parallel mode** (the Solo pill in the input area) to activate Workers. If all Workers fail, the Orchestrator is used as fallback automatically.

---

## Pipeline Phases

```
Scan        → theme extraction, source characterisation, citation age check
Research    → deep query; Workers run in parallel if enabled
Validate    → cross-provider claim verification; confidence scoring
Synthesize  → living brief update; contradiction and void identification
```

You can also run individual phases:
- **⚡ Quick Scan** — Scan phase only, fast overview
- **🛡 Forensics** — bias and credibility analysis
- **Basic mode** — single-pass summary, skips the full pipeline

---

## Data & Privacy

All data is stored in your browser's `localStorage`. Nothing is sent to any Distila server — there isn't one. API calls go directly from your browser to whichever provider you configured. Your keys never leave your machine.

To export your knowledge base: **Settings → Export / Import → Export JSON**.

---

## Keyboard Shortcuts

| Shortcut | Action |
|---|---|
| `Ctrl + Enter` | Run research pipeline |
| `Ctrl + Z` | Undo last claim discard |

---

## Architecture

Distila is intentionally a single HTML file (~3,200 lines). The design choice is philosophical as much as practical: research tools should be durable, portable, and auditable. A single file can be archived, diffed, and inspected in full. It has no build step, no dependencies to update, and no supply chain.

```
distila.html
├── CSS           theme variables, layout, component styles
├── HTML          three-pane layout (topics · center tabs · stream/input)
└── JavaScript
    ├── API layer         callAI / callAIWithRetry (Gemini, Claude, OpenAI-compat)
    ├── Pipeline engine   initiateResearch, runPhase, basicResearch
    ├── Claims engine     extractClaims, detectContradictions, scoreCitations
    ├── Render layer      renderSynthesis, renderClaims, renderVoids,
    │                     renderContradictions, renderResearchMap, buildKnowledgeGraph
    ├── Topic tree        renderTopicTreeFull, setTreeView, autoCategorizeTopic
    └── State             localStorage persistence, undo stack, export/import
```

---

## Version

**V5.4** — Research Map replaces Signal Map · Contradiction tab · Topic Tree dual view · Right pane redesigned · Voids research popup · AI Categorize All

---

## License

MIT
