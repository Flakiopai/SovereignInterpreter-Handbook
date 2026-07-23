# Chapter 16 — Glossary

## Senior engineer

Quick definitions aligned with the main SovereignInterpreter codebase (0.3.0).

| Term | Meaning |
|------|---------|
| **SovereignInterpreter** | Orchestrator class (`interpreter.py`) exposing `chat`, `reset`, `run_last_code` |
| **respond loop** | Chat → model → optional code run cycle in `respond.py` |
| **LocalLLM** | Plain-HTTP client to a local OpenAI-compatible chat endpoint |
| **MockLocalLLM** | Offline stand-in for tests (`mock://local`) |
| **allow_cloud** | Config flag; when false, non-local LLM URLs are rejected |
| **kill-switch** | Presence of `kill_switch_path` (default `.kill_switch`) halts protected ops |
| **SafetyRules** | Regex/pattern gate for cloud hosts, secret-like tokens, destructive shells |
| **Universal fence rule** | Model fenced code never auto-runs without intent/confirm/`%run` |
| **Intent** | Local heuristics: `looks_like_user_code`, `user_requests_execution` |
| **auto_run** | Skips confirm **only when** execution was already requested |
| **Sandbox mode** | `safe` / `strict` / `full` policy for python, shell, and FS roots |
| **effective_roots** | Roots actually applied after sandbox mode filtering |
| **FilesystemMutator** | Path jail for read/write/list/(optional) delete |
| **Computer / Terminal** | Facade + subprocess runners for python/shell |
| **Error envelope** | `[Category] message` via `SovereignError` hierarchy |
| **Display labels** | REPL tags: `[model]`, `[confirm]`, `[run]`, `[console]`, `[skip]`, `[error]` |
| **Memory pack** | Portable local recall via `SovereignMemory.export_pack` / `import_pack` |
| **LocalMessageRouter** | In-process message inbox (not a cloud broker) |
| **% magics** | REPL commands: `%reset`, `%run`, `%model`, `%models`, `%sandbox`, `%auto_run` |
| **! shell** | REPL shortcut to run a shell command (requires `full`) |
| **max_iterations** | Hard bound on `respond()` loops (default 10) |
| **show_tracebacks** | When true, envelopes may include detail/traceback |

## Beginner

Handy words you’ll see in the README and REPL:

- **Sandbox** — the “how much am I allowed to do?” setting  
- **Confirm** — the yes/no question before running model code  
- **Skip** — the interpreter refused to run something on purpose  
- **Console** — the output from code that did run  
- **Kill-switch** — emergency stop file  
- **Local model** — a model server on your machine (often Ollama)  

## Explain like I’m 12

```text
  Toy box     = sandbox / filesystem jail
  Permission  = confirm / intent
  STOP sign   = kill-switch
  Thinking hat= local model
  Sticker     = error envelope category
  Chalkboard  = chat messages (%reset wipes it)
```

## Repo examples (main SovereignInterpreter)

- Terms appear across `README.md`, `sovereign.yaml`, and `sovereigninterpreter/*.py`
- Label strings: `sovereigninterpreter/display.py`
- Exception names: `sovereigninterpreter/errors.py`

## Key takeaway

Shared vocabulary keeps operators, docs, and code talking about the **same** gates.

---

← [Sovereign doctrine](15-sovereign-doctrine.md) · [Back to index](index.md) · [README TOC](README.md)
