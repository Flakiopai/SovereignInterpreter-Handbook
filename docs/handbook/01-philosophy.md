# Chapter 1 — Philosophy

## Senior engineer

**Sovereign Edition** is the local-first, cloud-free, auditable cut of the chat → code → console model: same operator loop as upstream interpreter frameworks, but **sovereignty as an enforceable invariant**, not a README aspiration.

Doctrine pillars (runtime-enforced):

- **Policy sandbox** — sandbox modes + filesystem jail bound blast radius
- **Kill-switch** — `.kill_switch` halts chats and FS ops immediately
- **Fence rule** — model-fenced code never auto-runs without intent/confirm/`%run`
- **Intent detection** — chat vs execute vs typed Python before any runner
- **Cloud gate** — `allow_cloud: false` + `assert_llm_allowed` reject non-local LLM URLs

That means, in the main repo today:

- Inference defaults to a **local** HTTP endpoint (`http://127.0.0.1:11434/v1`).
- `allow_cloud: false` rejects non-local LLM URLs via `SovereignConfig.assert_llm_allowed`.
- Execution is gated by **intent**, **confirmation**, **sandbox mode**, and a **filesystem jail**.
- A filesystem **kill-switch** can halt chats and FS ops immediately.
- Cloud SDKs and remote message brokers are absent by design.

Upstream Rust / TUI / MCP / OS Seatbelt surfaces are **intentionally excluded** from Sovereign Edition — smaller attack surface, clearer audit path.

The philosophical bet: local operators should own the blast radius. Convenience features (`auto_run`, shell shortcuts, `full` sandbox) are **opt-in risk**, never silent defaults.

Independence stance (main `README.md` / `SECURITY.md`): this is an independent fork of an upstream interpreter framework. It is not affiliated with, endorsed by, or sponsored by any organization.

## Beginner

This project lets you talk to a model on your computer and optionally run the code it suggests. By default it:

- stays offline / on your LAN
- asks before running model code
- only touches allowed folders
- can be stopped with a kill-switch file

It exists because “chat that runs code” is powerful — and dangerous if it phones home or runs things you did not ask for.

## Explain like I’m 12

Imagine a robot helper that lives **inside your computer**. It can think and write little programs. It is not allowed to call strangers on the internet unless you say so, and it has to ask before cooking anything in the kitchen (running code).

```text
  Your brain (you)
       │
       ▼
  Local robot brain (Ollama / local model)
       │
       ▼
  "May I run this?" ──yes──► kitchen on YOUR machine
                 └──no───► just show the recipe
```

## Repo examples (main SovereignInterpreter)

- Doctrine framing: root `README.md` (“Why SovereignInterpreter Exists”, “Sovereign Doctrine”)
- Local-only LLM gate: `sovereigninterpreter/config.py` (`assert_llm_allowed`, `is_local_url`)
- Doctrine demos: `examples/sovereign/`

## Key takeaway

Sovereignty is **policy encoded as runtime checks**, not a marketing adjective.

---

← [Index](index.md) · [Next: Architecture →](02-architecture.md)
