# SovereignInterpreter Handbook

Documentation-only guide for **SovereignInterpreter** — the local-first chat → code → console interpreter.

This repository holds the operator and engineer handbook. It explains existing behavior in the main project: sovereignty doctrine, safety gates, intent detection, the execution pipeline, sandbox modes, REPL UX, and related surfaces. It does **not** ship interpreter code, tests, or runtime configs.

**Main project:** [Flakiopai/SovereignInterpreter](https://github.com/Flakiopai/SovereignInterpreter)

**Version covered:** 0.3.0

Each chapter includes three explanation layers (senior engineer, beginner, and explain-like-I’m-12).

---

## Table of contents

| # | Chapter |
|---|---------|
| — | [Handbook index](docs/handbook/index.md) |
| 1 | [Philosophy](docs/handbook/01-philosophy.md) |
| 2 | [Architecture](docs/handbook/02-architecture.md) |
| 3 | [Safety](docs/handbook/03-safety.md) |
| 4 | [Intent detection](docs/handbook/04-intent-detection.md) |
| 5 | [Execution pipeline](docs/handbook/05-execution-pipeline.md) |
| 6 | [Sandbox modes](docs/handbook/06-sandbox-modes.md) |
| 7 | [Error-handling envelope](docs/handbook/07-error-envelope.md) |
| 8 | [REPL UX](docs/handbook/08-repl-ux.md) |
| 9 | [Model switching](docs/handbook/09-model-switching.md) |
| 10 | [Context reset](docs/handbook/10-context-reset.md) |
| 11 | [Filesystem jail](docs/handbook/11-filesystem-jail.md) |
| 12 | [Runners](docs/handbook/12-runners.md) |
| 13 | [Tests](docs/handbook/13-tests.md) |
| 14 | [Roadmap](docs/handbook/14-roadmap.md) |
| 15 | [Sovereign doctrine](docs/handbook/15-sovereign-doctrine.md) |
| 16 | [Glossary](docs/handbook/16-glossary.md) |

Chapter files also live under [`docs/handbook/`](docs/handbook/README.md).

---

## Scope

- Markdown documentation only
- No interpreter source, no unit tests, no `sovereign.yaml` from the main project
- Cross-references in chapters point at paths in the **main** SovereignInterpreter repository

## License

MIT — see [LICENSE](LICENSE).
