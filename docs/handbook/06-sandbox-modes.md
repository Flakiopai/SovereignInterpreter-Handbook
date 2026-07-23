# Chapter 6 — Sandbox modes

## Senior engineer

`sandbox_mode` (default **`strict`**) is a policy triad enforced in config helpers, terminal runners, the respond pre-run checks, and the CLI `!` handler.

| Mode | Definition | Python | Shell / `!` | `effective_roots()` |
|------|------------|--------|-------------|---------------------|
| `safe` | Inspect-only; no runners | blocked | blocked | `["./workspace"]` |
| `strict` (default) | Python in workspace; shell off | allowed | blocked | `["./workspace"]` |
| `full` | Python + shell + configured roots | allowed | allowed | configured `allowed_roots` |

Helpers: `allows_python()`, `allows_shell()`, `effective_roots()`.

`%sandbox [safe|strict|full]` updates config and refreshes `computer.files.roots` (status via `[system] sandbox=…`).

**Shell tip:** `!` and model shell fences require **`%sandbox full`**. In default `strict`, a blocked `!` prints `[error] Shell blocked by sandbox=strict` then `[system] Tip: use %sandbox full to enable shell commands`. Policy denials from the respond loop show `[skip]` only (no misleading `[run]`).

**Deletes:** `allow_delete_default()` is **False** — filesystem delete stays denied unless an operator explicitly constructs the mutator with delete enabled.

```text
  Mode matrix

           python   shell    FS roots
  safe       ✗        ✗      ./workspace
  strict     ✓        ✗      ./workspace
  full       ✓        ✓      allowed_roots
                             (e.g. ./workspace, ./examples)
```

Live REPL shape:

```text
  You: %sandbox
  [system] sandbox=strict

  You: !ls
  [error] Shell blocked by sandbox=strict
  [system] Tip: use %sandbox full to enable shell commands

  You: %sandbox full
  [system] sandbox=full

  You: !ls
  [console] …
```

Important distinction: sandbox mode gates **what may run** and **which roots the FS mutator sees**. It is not an OS container. Subprocesses still run as local processes under policy (see [Runners](12-runners.md)).

## Beginner

Pick how brave you want to be:

- **safe** — talk / inspect only; no Python or shell runs  
- **strict** — Python OK inside `./workspace`; no shell (default)  
- **full** — Python + shell + your configured folders — required for `!`  

Use `%sandbox` to check or change. If shell is blocked, the tip says to run `%sandbox full`.

| You try | In `strict` | In `full` |
|---------|-------------|-----------|
| `print(1)` | `[console]` | `[console]` |
| `!ls` | `[error]` + `[system]` tip | `[console]` |
| model shell fence | `[skip]` | may run if intent/confirm pass |

## Explain like I’m 12

Three toy-box settings:

```text
  safe   = "Look, don't touch the tools"
  strict = "You may use the pencil (Python) in the toy box"
  full   = "Pencil + scissors (shell) + bigger toy boxes you listed"
```

If you try scissors in strict mode, the robot says “not allowed” and stops.

## Repo examples (main SovereignInterpreter)

- `sovereigninterpreter/config.py` — mode helpers, `effective_roots`
- `sovereigninterpreter/terminal.py` — pre-run sandbox checks
- `sovereigninterpreter/cli.py` — `%sandbox`, `!` gate
- `sovereign.yaml` — `sandbox_mode: strict`
- Tests: `tests/test_sandbox.py`
- CHANGELOG `[1.0.0]` / `[0.3.0]` sandbox notes

## Key takeaway

Sandbox mode is an **operator dial**: safer by default, wider only when you choose `full`.

---

← [Execution pipeline](05-execution-pipeline.md) · [Next: Error envelope →](07-error-envelope.md)
