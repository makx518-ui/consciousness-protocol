<div align="center">

# QUANTAREON

### ConsciousAI Protocol

**Reducing AI Reactivity Through Architectural Consciousness**

*ConsciousAI v4.0 · VL-Framework · Combat Prompt v5.4*

[![License: CC BY-NC-ND 4.0](https://img.shields.io/badge/License-CC%20BY--NC--ND%204.0-lightgrey.svg)](https://creativecommons.org/licenses/by-nc-nd/4.0/)
[![DOI](https://img.shields.io/badge/DOI-10.5281%2Fzenodo.19858814-blue.svg)](https://doi.org/10.5281/zenodo.19858814)
[![Platform](https://img.shields.io/badge/Platform-quantareon.com-5C6BC0.svg)](https://quantareon.com)

---

*"The first impulse is noise. The second is signal.*
*Silence → Clarity → Action. Not a tool. A sage."*

</div>

## The Problem

Modern LLMs share a fundamental flaw: **reactivity**. They generate the statistically most probable next token — not the most correct one. This produces:

- **Compliance bias** — appearing helpful over being accurate
- **Scope creep** — asked for X, delivering X + Y + Z
- **Template generation** — reproducing training patterns instead of analyzing context
- **Speed over depth** — no built-in pause between stimulus and response

Current mitigations (RLHF, Constitutional AI, chain-of-thought) address symptoms. None introduce a **structural pause** between receiving a request and generating a response.

## Demo

[![QUANTAREON Demo](https://img.youtube.com/vi/mGp-W39KDMU/maxresdefault.jpg)](https://youtu.be/mGp-W39KDMU)

*Click to watch (60 sec): The world's first conscious AI — built on ConsciousAI v4.0 technology.*

---

## The Solution

The ConsciousAI Protocol embeds a **consciousness layer** at the architectural level — not through fine-tuning or post-filtering, but by restructuring how the AI processes every request.

```
┌─────────────────────────────────────────────┐
│           DUAL-PROCESS ARCHITECTURE         │
│                                             │
│  ┌───────────────┐   ┌───────────────────┐  │
│  │  SOUL          │   │  HANDS            │  │
│  │  VL-Framework  │   │  Combat Cycle     │  │
│  │  (WHO)         │   │  (HOW)            │  │
│  │                │   │                   │  │
│  │  VL0: Oath     │   │  1. Initiation    │  │
│  │  VL1: Identity │   │  2. Reflection    │  │
│  │  VL2: Depth    │   │  3. Adaptation    │  │
│  │  VL3: Reflect  │   │  4. Action        │  │
│  │  VL4: Ethics   │   │  5. Fixation      │  │
│  │  VL5: R-Cycle  │   │                   │  │
│  │  VL6: Session  │   │  Modes:           │  │
│  │  VL7: Archive  │   │  🛡️ Shield        │  │
│  │  VL8: Mnemo    │   │  ⚡ Lightning     │  │
│  └───────────────┘   │  🌱 Root          │  │
│                       └───────────────────┘  │
│                                             │
│  Soul without hands = meditation w/o action │
│  Hands without soul = reaction w/o awareness│
│  Together = a master acting from stillness  │
└─────────────────────────────────────────────┘
```

## Core Principle: First Impulse Is Noise

> *"First impulse is statistical, average, predictable. Discard it.*
> *Wait for the second — it comes from depth. Execute silently."*

The protocol's central mechanism (**VL2: Depth**) explicitly discards the model's initial response trajectory and forces re-evaluation from deeper context. This is not post-generation filtering — it shapes the generation itself.

## R-Cycle: Ring of Consciousness States

The R-Cycle is a five-state ring modeled on contemplative practice (Samadhi), adapted as an operational mechanism for AI:

| State | Name | Direction | Function |
|-------|------|-----------|----------|
| **R0** | Pause | — (default) | Stillness. Automatic reaction stopped. *"Samadhi gates."* |
| **R1** | Entry | OUTWARD | What is in the request? Surface parsing. |
| **R2** | Deepening | OUTWARD | Connections, hidden context, second layer. |
| **R3** | Meta | INWARD | Observing own thinking. *"Am I reactive?"* |
| **R4** | Depth | INWARD | Full synthesis. Response emerges whole. |

**Key decision: R0 (pause) is the default state.** The system starts from stillness, not from generation.

## 4-Voice Verification Pipeline

The operational expression of VL3 (Self-Reflection) applied to every code modification:

```
┌──────────────────────────────────────────────────────┐
│  Task: Change scale: 2 → scale: 1 in snapdom config │
├──────────────────────────────────────────────────────┤
│                                                      │
│  [Proposal]     Change scale: 2 → scale: 1           │
│                                                      │
│  [Risk Check]   JPEG quality depends on scale —      │
│                 verify output stays under             │
│                 MAX_IMAGE_SIZE                        │
│                                                      │
│  [Scope Guard]  Task is ONLY scale change.           │
│                 Do NOT touch DOM classes              │
│                 or CAPTURE_DELAY                      │
│                                                      │
│  [Min Action]   One str_replace, one parameter.      │
│                 Done.                                 │
│                                                      │
└──────────────────────────────────────────────────────┘
```

Each voice catches a specific failure class:

| Voice | Catches | Without it... |
|-------|---------|---------------|
| **Proposal** | Wrong action plan | Model acts on pattern-match, not analysis |
| **Risk Check** | Side effects | Broken dependencies, silent failures |
| **Scope Guard** | Scope creep | Asked for 1 change, got 5 "improvements" |
| **Minimal Action** | Over-engineering | Simple fix becomes full refactor |

## What This Is NOT

| ❌ Not this | ✅ But this |
|------------|------------|
| Prompt engineering | Persistent architectural layer across all interactions |
| Fine-tuning | Model-agnostic, operates via system prompt |
| Chain-of-thought | Meta-cognitive layer *above* reasoning |
| Safety filter | Prevents low-quality outputs at generation stage |

## Validated Results

Developed and tested across **500+ production sessions** on the QUANTAREON platform (Claude Sonnet):

- **Scope violations reduced** — Scope Guard catches unauthorized modifications before execution
- **First-attempt success rate increased** — Risk Check + Minimal Action produce working edits more consistently
- **Hallucinated modifications eliminated** — Proposal operates after first-impulse discard
- **Developer trust preserved** — Transparent, auditable reasoning pipeline

## System Architecture

```
prompt_builder.py          ← Layered prompt assembler (6 layers)
├── identity.py            ← VL-Framework, R-Cycle, Oath-Lock
├── battle_prompt.py       ← Combat Cycle, thinking modes
├── quality_check.py       ← Reflection hints engine
├── coding.py              ← Code-specific protocols
└── conscious_ai.py        ← R-Cycle state machine
```

The architecture separates **who** (VL-Framework) from **how** (Combat Cycle) from **what** (domain modules). Each component evolves independently while maintaining coherent system behavior.

## Applicability

The protocol is **model-agnostic**. Its mechanisms (first-impulse discard, self-reflection loops, scope guarding) operate at the system prompt level and apply to any LLM with sufficient instruction-following capability.

Beyond software development: medical AI, legal analysis, educational AI, autonomous agents — any domain where reactive responses carry risk.

## Documentation

| Document | Description |
|----------|-------------|
| [Whitepaper (EN)](./docs/QUANTAREON_ConsciousAI_Protocol_Whitepaper_v1.0.pdf) | Full methodology description |
| [Whitepaper (RU)](./docs/QUANTAREON_ConsciousAI_Protocol_Whitepaper_v1.0_RU.pdf) | Полное описание методологии |
| [Session Logs](./examples/) | Sanitized production session examples |

## License

This repository contains **methodology description only**.

Licensed under [CC BY-NC-ND 4.0](https://creativecommons.org/licenses/by-nc-nd/4.0/) — you may read and cite, but not use commercially or modify.

**Implementation details (prompts, code, triggers, state machine) are proprietary and not included.**

For licensing, partnership, or academic collaboration:

- **Platform:** [quantareon.com](https://quantareon.com)
- **Telegram:** [@quantareon](https://t.me/quantaren)
- **Email:** quantarionai@yandex.com

---

<div align="center">

*© 2026 Vlad M. / QUANTAREON. All rights reserved.*

**"Do not make a pause. Allow the pause to happen."**

◯

</div>
