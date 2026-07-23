# Chapter 4 — Intent detection

## Senior engineer

Two helpers in `safety.py` classify user text before / during the execution decision.

### `looks_like_user_code(text)`

- Compiles as Python `exec`
- Rejects empty / magic (`%`) / shell (`!`) lines
- Requires code markers (`print`, `import`, `def`, `class`, `=`, `(`, etc.) or multi-line structural cues
- **Critical:** plain words like `hello` compile as expression statements but must return **False**

When true, `SovereignInterpreter.chat` **bypasses the LLM** and runs Python locally via `Computer.run`.

### `user_requests_execution(text)`

Returns true when:

- `looks_like_user_code(text)`, **or**
- an execution-request regex matches phrases such as run / execute / compute / calculate / “write a … script” / “in python” / shell or bash command

Used by `respond()` as `execution_requested`. Combined with `auto_run` and `confirm` in `_should_execute`.

**Confirm flow (REPL):** when intent says run and `auto_run` does not already cover it, `_confirm` prints `[confirm] {language}`, a dim boxed full-code preview, then `Run this code? [y/N]:` (only `y` / `yes`; deduped — not re-echoed after approval).

**Multi-line input:** wrap a block in `"""` … `"""` (continuation prompt `... `). The joined text is classified by the same helpers — multi-line Python markers make `looks_like_user_code` true and bypass the LLM.

**One-shot exec:** `sovereigninterpreter run "…"`. Invoking `run` is an explicit operator execution request; confirm is auto-approved for that call (`confirm=lambda …: True`).

This is the primary **hallucination-loop prevention** surface: unsolicited fences produce `ExecutionDenied`, display/skip, and **break** — no silent auto-run, no retry storm.

### CLI order (before `chat`)

```text
  REPL line
     │
     ├─ %magic  → handle locally (no LLM) → [system] status
     ├─ !cmd    → shell if sandbox allows, else [error] + [system] tip
     └─ else    → interpreter.chat(...)   (""" multi-line joins first)
```

Inside `chat`:

```text
  user text
     │
     ├─ looks_like_user_code? ──yes──► run python locally (no LLM)
     │
     └─ LLM path (respond)
           user_requests_execution?
              yes + auto_run → may run without prompt
              yes + confirm  → [confirm] + box + [y/N]
              no             → show fence, ExecutionDenied / [skip], stop
```

Live shapes:

```text
  You: %sandbox
  [system] sandbox=strict

  You: """
  ... print(1)
  ... print(2)
  ... """
  [run] python → print(1) print(2)
  [console] 1
  2

  $ sovereigninterpreter run "print(2+2)"
  [run] python → print(2+2)
  [console] 4
```

## Beginner

The interpreter asks: “Is this **you writing code**, **you asking to run something**, or **just talking**?”

| You type | What happens |
|----------|----------------|
| `print(2+2)` | Runs locally; model not called |
| `"""` … multi-line Python … `"""` | Same as typed code — local run if it looks like Python |
| `Compute 2+2 in python` | Model may answer; then `[confirm]` + box + `[y/N]` |
| `hello` | Chat only; fences (if any) shown and skipped |
| `%run` | Runs the last assistant code block (`[system]` / console as applicable) |
| `!ls` | Shell shortcut (only in `full`; else `[error]` + `[system]` tip) |
| `sovereigninterpreter run "…"` | One-shot; explicit exec, no interactive confirm |

## Explain like I’m 12

- If you hand the robot a finished homework sheet (`print(...)`), it grades it right away.  
- If you say “please do my homework,” it can work on it.  
- If you just say “hi,” it chats — even if it doodles a fake homework sheet, it won’t turn it in unless you say so.

## Repo examples (main SovereignInterpreter)

- `sovereigninterpreter/safety.py` — `looks_like_user_code`, `user_requests_execution`
- `sovereigninterpreter/interpreter.py` — direct-exec branch in `chat`
- `sovereigninterpreter/respond.py` — `execution_requested` + deny path
- `sovereigninterpreter/cli.py` — `%` / `!` precedence
- Tests: `tests/test_interpreter.py`

## Key takeaway

Intent is a **local heuristic**, not the model’s claim. The model cannot self-authorize execution.

---

← [Safety](03-safety.md) · [Next: Execution pipeline →](05-execution-pipeline.md)
