# Chapter 8 — REPL UX

## Senior engineer

Entry points: `sovereigninterpreter` or `python -m sovereigninterpreter`. Commands: `repl` (default) | `version`. Flag: `--auto-run`.

Banner shows title, `auto_run=…`, `sandbox=…`, safety line, magic list, and `!command` hint.

| Input | Behavior |
|-------|----------|
| `%reset` | `interpreter.reset()` → “Conversation reset.” |
| `%auto_run on\|off` | toggles `config.auto_run` |
| `%model` / `%model name` | show / resolve against `ollama list` |
| `%models` | list installed; `*` marks active |
| `%run` | `run_last_code()` → `[console]` |
| `%sandbox [safe\|strict\|full]` | get/set mode + refresh FS roots |
| `!cmd` | shell if `allows_shell()`, else `SandboxBlocked` |
| other | `chat(..., display=True, confirm=_confirm)` |

Confirm prompt: `Run this code? [y/N]:` — only `y` / `yes` approve.

Display labels (`display.py`):

| Label | Meaning |
|-------|---------|
| `[model] thinking…` | awaiting completion |
| `[model]` | assistant prose |
| `[confirm]` | code awaiting approval (lang + short preview) |
| `[run]` | about to execute |
| `[console]` | runner output |
| `[skip]` | policy denial (intent/sandbox) |
| `[error]` | typed failure |

User messages are not re-echoed by `format_message_for_repl`. `NO_COLOR` disables ANSI via `util.use_color`.

```text
  line from keyboard
       │
       ├─ %…  → magic
       ├─ !…  → shell gate
       └─ …   → chat + labels
```

## Beginner

The REPL is a chat box with cheat codes:

- Type normally to talk to the local model  
- Type Python yourself to run it immediately  
- Use `%run` if the model showed code and you want to run that last block  
- Use `%sandbox` / `%model` / `%reset` to steer the session  
- Prefix with `!` for a shell command (needs `full` sandbox)  

When asked `Run this code? [y/N]:`, press Enter or `n` to skip; type `y` to approve.

## Explain like I’m 12

```text
  You: hello
  Robot: [model] Hi!

  You: print(2+2)
  Robot: [console] 4

  You: write a python script that prints hi
  Robot: [confirm] python → print("hi")
  You: y
  Robot: [run] …
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
