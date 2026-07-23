# Changelog

All notable changes to the **SovereignInterpreter Handbook** are documented in this file.

The format is based on [Keep a Changelog](https://keepachangelog.com/en/1.1.0/),
and this project adheres to [Semantic Versioning](https://semver.org/spec/v2.0.0.html).

## [1.0.0] — 2026-07-23 — Sovereign Edition

First stable **SovereignInterpreter Handbook v1.0.0** release, fully aligned with [SovereignInterpreter v1.0.0](https://github.com/Flakiopai/SovereignInterpreter/tree/v1.0.0).

### Updated chapters

| Chapter | Sync highlights |
|---------|-----------------|
| [01 — Philosophy](docs/handbook/01-philosophy.md) | Sovereign Edition framing; doctrine pillars; local-first / cloud-free / auditable; upstream Rust/TUI/MCP/OS Seatbelt excluded |
| [03 — Safety](docs/handbook/03-safety.md) | Confirm-first; envelopes; sandbox skip; soft ANSI; thinking timer; kill-switch envelope + Ready/HALTED examples |
| [04 — Intent](docs/handbook/04-intent-detection.md) | Confirm flow; multi-line input; one-shot exec; `[system]` examples |
| [06 — Sandbox](docs/handbook/06-sandbox-modes.md) | safe/strict/full definitions; `%sandbox full` tip; live `[system]` / `[error]` / `[skip]` examples |
| [08 — REPL UX](docs/handbook/08-repl-ux.md) | Magics; banner/Ready; boxed preview; soft ANSI; thinking timer; confirm; sandbox skip; live examples |
| [10 — Context reset](docs/handbook/10-context-reset.md) | `[system] Conversation reset.`; post-reset boxed confirm shape |
| [15 — Doctrine](docs/handbook/15-sovereign-doctrine.md) | Memory packs local-only/cloud-free; `%memory export\|import` + `[system]` |
| [16 — Glossary](docs/handbook/16-glossary.md) | Magics, envelopes, sandbox modes, kill-switch, memory packs, multi-line, boxed preview, thinking timer, one-shot |

Supporting version bumps: index / handbook README metadata → **1.0.0**.

### Alignment

- Matches SovereignInterpreter **v1.0.0** doctrine, safety model, and REPL UX
- Release notes: [docs/releases/v1.0.0.md](docs/releases/v1.0.0.md)

## [0.3.0] — 2026-07-22

Initial handbook documentation set covering SovereignInterpreter 0.3.0 behavior (philosophy through glossary).
