# Game Concept Creator

A multi-agent system, built on top of **Claude Code**, that turns a raw game idea into a worked-out, prototype-ready game concept. It runs as a disciplined, phased co-authoring process: a single orchestrator-assistant drives the work and conducts a set of specialized agents (Analyst, Writer, Critic) plus a Brainstorm mode.

The system is opinionated about *argumentation*: no praise, every proposal must be justified against the core loop, and every radical change is checked for integrity before it is committed.

---

## How it works

The work is driven by one main skill — **`concept-assistant`** — which you invoke in the chat. From there everything is orchestrated for you: the assistant keeps the whole concept in mind, presses on weak spots, and calls the right agent at the right phase.

### Quick start

1. Open this folder in Claude Code.
2. Drop your idea into the chat (or place it as a `source-idea.md`) and invoke the assistant — e.g. *"turn this idea into a concept"* or trigger the `concept-assistant` skill.
3. Answer the assistant's questions phase by phase. It will create a per-concept folder under `concepts/<slug>/` and fill it with artifacts.
4. Iterate until the exit criterion is met: **the Critic reports no 🔴 blockers AND you are satisfied.** The assistant then produces the final document.

You can stop at any time — state is persisted in each concept's `_state.md` and the `concepts/_index.md` registry — and resume later. On every new session the assistant reads the registry and restores context automatically.

---

## Skills & agents structure

### Skills (you invoke these in the chat)

| Skill | Role |
|-------|------|
| **`concept-assistant`** | The main entry point and orchestrator. Drives the phases, conducts the agents, keeps argumentation discipline, maintains state. Works in the main chat and sees the whole conversation. |
| **`concept-brainstorm`** | A disciplined idea-generation / stress-testing mode. Generates justified variants (**Idea → Why → Risk**) anchored to the concept frame, or attacks an idea as "devil's advocate". Does not write decision files itself. |

Skills live in `.claude/skills/<name>/SKILL.md`.

### Agents (the assistant invokes these as isolated subagents)

Agents are **single-use and isolated**: each runs without seeing the chat, returns one result, then its context is destroyed. If a result is weak, the assistant launches a fresh agent with a refined task.

| Agent | Phase | Reads | Writes |
|-------|-------|-------|--------|
| **`market-analyst`** | 2 (+ ad-hoc) | the frame from `concept-card.md` | `research/market-report.md` |
| **`concept-writer`** | 4 & 8 | card, market report, `decisions.md`, assistant notes | `concept-v{N}.md` / `concept-final.md` |
| **`concept-critic`** | 5 | **only** `concept-v{N}.md` + `concept-card.md` | `feedback/critic-v{N}.md` |

- **market-analyst** gathers a market snapshot (genre benchmarks, recent releases, direct references, niche saturation, typical mechanics, observations). Never invents numbers or game names.
- **concept-writer** structures (never invents) the supplied artifacts into the 12-section concept document; gaps are marked *"undefined, needs elaboration"*.
- **concept-critic** is deliberately uninformed — it sees only the document and the card — and reviews through three lenses: **game-design integrity · feasibility · market**. Every remark is marked 🔴 blocking · 🟡 important · 🟢 to consider, with an argument; praise is forbidden.

Agents live in `.claude/agents/<name>.md`.

---

## External tools

| Tool | Used by | Purpose | Fallback |
|------|---------|---------|----------|
| **Claude Code** | the whole system | Host harness that runs the skills and agents | — |
| **Tavily** (`tavily_search` / `tavily_research` / `tavily_extract`) | market-analyst | Primary market-research data source | ↓ |
| **WebSearch / WebFetch** | market-analyst | Used when Tavily is unavailable or returns empty results | ↓ |
| **Model knowledge** | market-analyst | Last resort when there is no internet at all | — |

The analyst always follows this source chain in priority order and **states in the report header which source was actually used** — if it is not Tavily, it adds a caveat about reduced confidence. Treat that mark as a signal of how much to trust the data.

---

## Exit criterion

A concept is considered ready for its final document when **both** conditions hold:

1. The Critic reports **no 🔴 blocking** remarks, and
2. The author is satisfied.

The assistant tracks both and voices how far off the final is. If iterations fail to converge, it names the concrete blockers and proposes a resolution (cut a mechanic, reposition the genre, etc.) rather than looping indefinitely.
