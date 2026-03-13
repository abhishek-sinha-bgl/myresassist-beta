# Distila · Research Intelligence

> *A structured epistemic environment for researchers, analysts, and anyone who needs to think harder than their tools do.*

**Version 5.9** · Single-file HTML application · No server required · Bring your own LLM keys

---

## What it is

Distila is a local-first research intelligence tool that turns scattered evidence into structured argument. It's not a chatbot with a search bar. It's an environment where claims have provenance, confidence decays over time, you can commit to positions and have them challenged, and the relationships between ideas become a queryable graph.

You bring your LLM API keys (Gemini, OpenAI, Anthropic, Ollama, or any OpenAI-compatible endpoint). Distila orchestrates them into a research pipeline — scan, deep research, validate, synthesise — then helps you build on top of the output.

---

## Core philosophy

Most AI research tools accelerate the gathering of information. Distila is designed around a different problem: **the rigour of what you do with it.**

That means:
- Claims trace to sources, not just to an LLM
- Your confidence in a claim isn't static — it decays as the field moves
- When you commit to a position, the tool stress-tests it
- The map of how your ideas relate to each other is something you can query, not just see

---

## Getting started

Download `distila_v59.html`. Open it in any modern browser. Add at least one LLM provider in Settings.

That's it. Everything runs client-side. State is saved to localStorage.

### Supported providers

| Provider | Notes |
|---|---|
| Google Gemini | All models including 1.5 Pro, 2.0 Flash, 2.5 Pro |
| OpenAI | GPT-4o, GPT-4 Turbo, o1/o3 series |
| Anthropic | Claude 3.5 Sonnet, Claude 3 Opus |
| Ollama | Local models via `http://localhost:11434` |
| OpenAI-compatible | Any endpoint that speaks the OpenAI API format |

You can add multiple providers and run them in parallel (multi-LLM mode) to cross-validate claims from different models simultaneously.

---

## The research pipeline

```
Scan → Research → Validate → Synthesise
```

**Scan** reads a URL or brief description and extracts topics, entities, and initial signal. It doesn't overwrite your brief — scan results appear in a banner with three choices: run the full pipeline, synthesise only from the scan, or discard.

**Research** runs a deep research pass, extracting structured claims with confidence scores, evidence types, citation years, source credibility assessment, and reasoning. Each claim is immediately scored against citation age and source credibility.

**Validate** sends each unvalidated claim to a second provider (or the same one) for a cross-check pass. Claims get VERIFIED, CONTESTED, WEAK, or UNVERIFIED status.

**Synthesise** compiles everything into a markdown brief, explicitly anchoring to any positions you've committed to.

Each stage button is clickable — you can run them individually, re-run synthesis over existing claims, or trigger a batch validation sweep.

---

## Claims Vault

The Claims Vault is where all extracted claims live. Each claim card shows:

- Status badge (VERIFIED / CONTESTED / WEAK / UNVERIFIED)
- Confidence score with any citation-age or credibility adjustments
- **Decay chip** — ⏱ Stale or ⏱ Drifting if confidence has dropped since extraction
- Source provenance: either a linked primary document with a passage, or an explicit `⚠ AI hypothesis` warning
- Evidence type (peer-reviewed / government / industry / journalism / opinion)
- Validation history

Filter the vault by: All / Grounded / Needs Source / ⊕ Positions.

The **⏱ Refresh Confidence** button runs a full decay sweep on demand.

---

## Sprint 1 — Primary source anchoring

Every claim is either **grounded** (linked to a URL you attached, with a specific quoted passage) or explicitly flagged as an AI hypothesis.

You attach source documents to a topic via the Sources panel in the sidebar. Distila fetches the URL, extracts 3–6 key passages with AI, and stores them. When research runs, claims are auto-matched to passages where possible. The **🔗 Ground** button manually finds the best matching passage across all attached sources for any ungrounded claim.

Grounded claims get a green left border and a domain favicon chip. Ungrounded claims get amber + the AI hypothesis warning.

---

## Sprint 2 — Position objects

A **Position** is a claim you personally commit to. It's not just a finding from the AI — it's a statement you're willing to defend.

Click **◈ I believe this** on any claim to promote it to a Position. Immediately:

1. The card gets a distinct visual treatment — cyan gradient, glow border, position badge
2. An async adversarial challenge pass runs, returning: weak assumptions, counterarguments, and one "must establish" requirement to defend the position
3. The position anchors synthesis — subsequent briefs explicitly note where evidence supports or challenges it
4. The Argument Map shows Position nodes larger, with a ring

Positions decay more slowly (1.5× the normal half-life) because commitment implies you've done more scrutiny. But they don't decay to zero — the floor is 20% rather than 10%.

---

## Sprint 3 — Typed argument graph

The Argument Map is not just a visualisation. It's a queryable structure.

**Four edge types:**
- **Supports** — A provides evidence for B
- **Contradicts** — A directly opposes B  
- **Qualifies** — A limits or nuances B
- **Depends on** — A's validity requires B to be true

Click **✦ Infer Edges** to have the AI analyse all claims and assign typed relationships. Edges run automatically after every full pipeline.

**Cascade analysis**: click any node to see what depends on it. The map highlights the chain and dims everything else. The **Load-bearing panel** appears below with every downstream claim listed as chips — click any to jump. If the node is a Position with challenges, the `mustProve` requirement is surfaced as the anchor condition.

Add relationships manually with **⊕ Link** on any claim card.

The map stats track: Claims Mapped / Typed Edges / Contradictions / Verified / Avg Confidence.

---

## Sprint 4 — Confidence decay

Confidence is not a number stamped at extraction time. It decays.

### Decay model

| Field velocity | Half-life | Examples |
|---|---|---|
| High | 4 months | AI, crypto, geopolitics, LLMs |
| Medium | 12 months | Policy, health, markets, regulation |
| Low | 30 months | Science, history, geography |

Evidence type modifies this:
- Peer-reviewed: 2.5× longer half-life
- Government: 1.8×
- Industry: 1.0×
- Journalism: 0.7×
- Opinion: 0.5×

Formula: `confidence(t) = confidence₀ × 0.5^(t / halfLife)`

### What it looks like

A silent decay sweep runs every time you open a topic. Claims that have dropped ≥30 points since extraction get a red **⏱ Stale** chip. Claims down 10–29 points get amber **⏱ Drifting**. The confidence badge shows the delta inline.

The **Confidence History** sparkline in the stats panel shows a dated timeline of decay sweeps. Bars are green (fresh), amber (drifting), or red (stale) based on that snapshot. The trend label shows the net change since the first sweep.

---

## Argument Map

The Argument Map (Research Map tab) renders all claims as a radial graph:
- Distance from centre = lower confidence (outer ring = uncertain)
- ◈ Position nodes are larger with a glowing ring
- Contradicting claims are separated into a purple `CONTRA` sector
- Edge types each have distinct visual treatment

Click any node to run cascade analysis. Use **✦ Infer Edges** to populate relationships. **⊕ Link** adds manual edges.

---

## Multi-LLM mode

Enable in Settings. When active, all enabled providers run every research phase in parallel. Claims are merged with deduplication (>72% textual similarity). Contradictions across providers are automatically detected. The source provider is tracked on every claim.

This is the most powerful mode for high-stakes research — you get cross-model validation built into the extraction pass, not as a separate step.

---

## Exporting

**Print to PDF** — File → Print → Save as PDF. The print view renders synthesis, claims (with provenance and decay state), and source citations.

**Topic export** — available from the topic context menu.

---

## Architecture

Single HTML file. No build step. No server. No telemetry. ~4,200 lines.

State is stored in `localStorage`. Import/export JSON for backup and transfer.

All LLM calls go directly from the browser to your provider's API. Distila never sees your API keys or research data.

---

## Honest limitations

**Claims still originate from LLMs.** The primary source anchoring is real — but the workflow is still "AI generates claim, then finds supporting passage." The architectural ideal is inverted: document as source of truth, AI as analyst. That requires a different data model and is on the roadmap.

**Adversarial challenge only fires on Positions.** High-confidence ungrounded claims you haven't committed to aren't challenged. There's an asymmetry: your explicit beliefs get scrutinised, your implicit assumptions don't.

**No memory of revision.** Discarded claims disappear. Withdrawn positions leave no trace. The history of what you changed your mind about — which is often the most valuable intellectual record — isn't kept.

---

## Roadmap

- **Document-first extraction** — attach a corpus, claims originate from it, LLM provides analysis
- **Universal challenge mode** — adversarial pass on all high-confidence ungrounded claims
- **Revision ledger** — discarded claims and withdrawn positions recorded with timestamps and reasons
- **Desktop client** — Electron wrapper, local file system, no browser storage limits
- **Enterprise intranet mode** — primary source layer backed by internal document stores (SharePoint, Confluence, proprietary databases) with restricted commercial data source connectors
- **SaaS** — team workspaces, shared topic graphs, collaborative position-taking

---

## License

MIT

---

*Built with care for people who need to think harder than their tools do.*
