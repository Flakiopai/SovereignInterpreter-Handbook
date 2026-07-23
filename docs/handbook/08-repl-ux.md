# Chapter 8 — REPL UX

## Senior engineer

Entry points: `sovereigninterpreter` or `python -m sovereigninterpreter`. Commands: `repl` (default) | `version` | `run`. Flag: `--auto-run`.

One-shot exec: `sovereigninterpreter run "print(2+2)"` — explicit operator request; confirm auto-approved for that call.

### Banner / Ready

Dim monochrome **SI** wordmark + `SovereignInterpreter v{version}`, then a boxed **Ready** panel:

```text
┌─────────────────────────────────────────┐
│ Ready  model=llama3.2                   │
│ endpoint=http://127.0.0.1:11434/v1      │
│ sandbox=strict  auto_run=off            │
└─────────────────────────────────────────┘
```

Followed by `[system]` tip / magics footer. Wordmark and box use dim ANSI via `util.paint` and respect `NO_COLOR`.

| Input | Behavior |
|-------|----------|
| `%help` | Magics, doctrine dials, sandbox modes, model/endpoint, memory → `[system]` |
| `%status` | Live sandbox, auto_run, model, endpoint, roots, kill-switch, memory → `[system]` |
| `%reset` | `interpreter.reset()` → `[system] Conversation reset.` |
| `%undo` | Drop last user turn + everything after → `[system] Undid…` / `Nothing to undo.` |
| `%memory export\|import [path]` | Memory pack JSON (default `memory.json`) → `[system] Memory exported/imported…` |
| `%auto_run on\|off` | toggles `config.auto_run` → `[system]` |
| `%model` / `%model name` | show / resolve against `ollama list` → `[system]` |
| `%models` | list installed; `*` marks active → `[system]` |
| `%run` | `run_last_code()` → `[console]` |
| `%sandbox [safe\|strict\|full]` | get/set mode + refresh FS roots → `[system] sandbox=…` |
| `"""…"""` | Multi-line input (continuation prompt `... ` until closing `"""`) |
| `!cmd` | shell if `allows_shell()`; else `[error]` + `[system]` tip to `%sandbox full` |
| other | `chat(..., display=True, confirm=_confirm)` |

### Confirm / skip / labels

**Confirm flow:** `_confirm` prints `[confirm] {language}`, a dim **boxed full-code preview** (`format_confirm_box`), then `Run this code? [y/N]:` (only `y` / `yes`; deduped — not re-echoed after approval).

**Sandbox skip:** policy denials suppress `[run]` and show only `[skip]` (e.g. `SandboxBlocked`).

**Soft ANSI:** color applies to the `[tag]` only for `confirm` / `console` / `error` / `skip` / `system` (yellow / dim / red / yellow / cyan); `[model]` and `[run]` stay plain. `NO_COLOR` disables color.

**Thinking timer:** `[model] thinking…` while awaiting completion, then `[model] thinking… (Ns)` after return (in-place `\r` cleared before the elapsed line).

| Label | Meaning |
|-------|---------|
| `[model] thinking…` / `thinking… (Ns)` | awaiting completion (elapsed after return) |
| `[model]` | assistant prose |
| `[confirm]` | language tag before boxed code preview |
| `[run]` | about to execute |
| `[console]` | runner output |
| `[skip]` | policy denial (intent/sandbox) |
| `[error]` | typed failure |
| `[system]` | banner tips, magic status, shell tip |

Magic status lines use **`[system]`** envelopes (not bare prose).

User messages are not re-echoed by `format_message_for_repl`.

```text
  line from keyboard
       │
       ├─ %…      → magic → [system]
       ├─ !…      → shell gate
       ├─ """…""" → multi-line gather
       └─ …       → chat + labels
```

## Beginner

The REPL is a chat box with cheat codes:

- Type normally to talk to the local model  
- Type Python yourself to run it immediately  
- Paste multi-line code between `"""` markers  
- Use `%run` if the model showed code and you want to run that last block  
- Use `%help` / `%status` / `%undo` / `%memory export|import` / `%sandbox` / `%model` / `%reset` to steer the session  
- Prefix with `!` for a shell command (needs `full` sandbox)  
- One-shot outside the REPL: `sovereigninterpreter run "…"`  

When asked `Run this code? [y/N]:`, press Enter or `n` to skip; type `y` to approve. The full code sits in a dim box above the prompt.

### Live examples

```text
You: print(1)
[run] python → print(1)
[console] 1

You: write a python script that prints hi
[model] thinking…
[confirm] python
────────────────────────────
print("hi")
────────────────────────────
Run this code? [y/N]: y
[model] Sure.
[console] hi

You: """
... def greet(name):
...     print(f"hi {name}")
... greet("world")
... """
[run] python → def greet(name): print(f"hi {name}") greet("world")
[console] hi world

$ sovereigninterpreter run "print(2+2)"
[run] python → print(2+2)
[console] 4

You: %memory export
[system] Memory exported short=0 long=0 → …/memory.json

You: %memory import
[system] Memory imported short=0 long=0 ← …/memory.json

You: %sandbox
[system] sandbox=strict

You: !ls
[error] Shell blocked by sandbox=strict
[system] Tip: use %sandbox full to enable shell commands
```

## Explain like I’m 12

```text
  You: hello
  Robot: [model] thinking… (0.2s)
         [model] Hi!

  You: print(2+2)
  Robot: [run] python → print(2+2)
         [console] 4

  You: write a python script that prints hi
  Robot: [model] thinking…
         [confirm] python
         ────────────────────────────
         print("hi")
         ────────────────────────────
  You: y
  Robot: [model] Sure.
         [console] hi
```

Special buttons start with `%`. Shell shortcuts start with `!`.

## Repo examples (main SovereignInterpreter)

- `sovereigninterpreter/cli.py`
- `sovereigninterpreter/display.py`
- `sovereigninterpreter/util.py` — color / truncate helpers
- Root `README.md` — Safety / REPL helpers section

## Key takeaway

REPL UX is **labeled and confirm-first**: magics steer policy; chat never silently executes model fences.

---

← [Error envelope](07-error-envelope.md) · [Next: Model switching →](09-model-switching.md)
