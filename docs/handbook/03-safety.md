# Chapter 3 — Safety

## Senior engineer

Safety in SovereignInterpreter is a **stack of gates**, not a single flag.

### 1. Cloud URL gate

- Default: `allow_cloud: false`
- `SovereignConfig.assert_llm_allowed` / `is_local_url` allow loopback, `localhost`, `host.docker.internal`, and RFC1918-style private hosts
- Non-local URLs raise `CloudForbiddenError` when enforcement is on (`LocalLLM` construction)

### 2. Kill-switch

**Doctrine (Sovereign Edition):** the kill-switch is **mandatory** — default `kill_switch: true`, path `.kill_switch`. Presence of the path engages the switch; `assert_not_killed` raises `KillSwitchError`. Asserted in interpreter init/chat/`run_last_code`, `respond` loop, terminal/computer runs, FS ops, and CLI start/loop.

REPL envelope:

```text
[error] KillSwitchError: Kill-switch engaged (found .kill_switch). Halting.
```

Boot when clear — boxed **Ready** line:

```text
┌─────────────────────────────────────────┐
│ Ready  model=llama3.2                   │
│ endpoint=http://127.0.0.1:11434/v1      │
│ sandbox=strict  auto_run=off            │
└─────────────────────────────────────────┘
```

Engaged — **HALTED** (no Ready session; chats/FS ops stop):

```text
You: hello
[error] KillSwitchError: Kill-switch engaged (found .kill_switch). Halting.
```

Remove the file to clear; `%status` reports `kill_switch=engaged` vs `clear`.

### 3. SafetyRules patterns

`SafetyRules` (`safety.py`) defaults include blocking patterns such as:

- `api.openai.com` / `azure.openai`
- secret-like `sk-…` tokens
- destructive `rm -rf /`
- outbound `curl http(s)://` except localhost/127.0.0.1

Checked on user text in `chat()`, and again in `respond()` on accumulated messages, model reply, and code before run. Violations raise `SafetyViolation`; scrubbing replaces matches with `[BLOCKED]`.

### 4. Universal fence rule + confirm-first

Model fenced code never auto-runs (**confirm-first**). `_should_execute` requires explicit intent and either confirmation, or (`auto_run` **and** `user_requests_execution`). Default `auto_run: false` — `auto_run` only skips the prompt when execution was already requested.

Confirm UI: `[confirm] {language}`, a dim **boxed full-code preview**, then `Run this code? [y/N]:` (deduped; only `y` / `yes` approve).

```text
  Incoming text / proposed code
        │
        ▼
  kill-switch? ──yes──► halt
        │ no
        ▼
  SafetyRules.check
        │
        ▼
  intent + confirm / auto_run
        │
        ▼
  sandbox mode (python/shell)
        │
        ▼
  runner / FS jail
```

### 5. Envelopes, skips, and soft labels

Typed envelopes (`PythonError`, `ShellError`, `ModelOutputError`, `SandboxBlocked`, `ExecutionDenied`, `KillSwitchError`, `CloudForbiddenError`, `SafetyViolation`, …) remap in the REPL to `[error]` or `[skip]`.

- **Sandbox skip** — policy blocks emit `[skip]` only (no misleading `[run]` beforehand).
- **Soft ANSI** — color applies to the `[tag]` only for `confirm` / `console` / `error` / `skip` / `system` (yellow / dim / red / yellow / cyan); `[model]` and `[run]` stay plain. `NO_COLOR` disables color.
- **Thinking timer** — `[model] thinking…` while awaiting completion, then `[model] thinking… (Ns)` after return (in-place `\r` cleared before the elapsed line).

## Beginner

Four everyday protections:

| Protection | What it does |
|------------|--------------|
| Local-only LLM | Won’t talk to cloud model URLs by default |
| Kill-switch file | Create `.kill_switch` to stop activity |
| Pattern rules | Blocks obvious cloud/secret/destructive snippets |
| Ask before run | Model code needs your intent or a yes |

You can turn `auto_run` on later — that only skips the prompt when you already asked to execute.

## Explain like I’m 12

The robot has:

1. A rule: “no calling strangers” (local URLs only)  
2. A big red STOP sign you can put on the lawn (`.kill_switch`)  
3. A list of naughty words/commands it must not do  
4. A habit of asking “May I cook this?” before using the stove  

```text
  STOP sign on lawn  → everything freezes
  Naughty pattern    → "Nope"
  Model code block   → "Ask first"
```

## Repo examples (main SovereignInterpreter)

- `sovereigninterpreter/safety.py` — `SafetyRules`, intent helpers
- `sovereigninterpreter/config.py` — kill-switch + `assert_llm_allowed`
- `examples/sovereign/doctrine_demo.py` — offline doctrine checks
- `SECURITY.md` — independence + reporting surface
- Defaults: `sovereign.yaml`

## Key takeaway

Safety is **defense in depth**: network gate, emergency stop, pattern scrub, and execution consent — all before runners fire.

---

← [Architecture](02-architecture.md) · [Next: Intent detection →](04-intent-detection.md)
