# Reasonix SDK for Workshop

This SDK provides the Reasonix CLI, a DeepSeek-native AI coding agent that runs
as a single static Go binary in your workshop terminal. It installs `reasonix`
onto the `PATH` and persists the Reasonix home directory — configuration,
provider secrets, session history, and checkpoints — on the host so your setup
survives across workshop updates.

---

## Reference workshop

A minimal workshop:

```yaml
# workshop.yaml
name: reasonix-example
base: ubuntu@24.04
sdks:
  - name: reasonix
    channel: latest/stable

actions:
  review: |
    reasonix run "review the changes in this repository"
```

This demonstrates running a one-shot Reasonix coding task inside the workshop,
with configuration and credentials persisted between workshop updates.

---

## Using the SDK

### Prerequisites, project layout

1. No prerequisite SDKs are required. Reasonix talks to a remote,
   OpenAI-compatible endpoint (DeepSeek by default), so a network connection and
   a provider API key are all you need.
2. No specific project layout is needed. Run Reasonix from any directory; use
   `/init` to generate an `AGENTS.md` project memory file for the current repo.
3. On launch the SDK adds `reasonix` to the `PATH` and mounts the Reasonix home
   directory (`~/.reasonix`) from the host, so config and secrets written by
   `reasonix setup` are preserved.

### Configure and run

From within the workshop shell:

```bash
workshop shell
reasonix setup                       # scaffold config.toml (+ .env) in ~/.reasonix
export DEEPSEEK_API_KEY=sk-...        # or let setup save it to the Reasonix home .env
reasonix run "implement the TODOs in main.go"
```

Reasonix is not limited to the Deepseek provider, although it is the best supported.

Configuration via other providers is possible via OpenAI/Anthropic compatible endpoints,
e.g. to configure with OpenRouter, you can edit `~workshop/.reasonix/config.toml` with:

```toml
default_model = "custom-openrouter-ai/deepseek/deepseek-v4-pro"
...
[desktop]
provider_access = ["custom-openrouter-ai"]
...
[[providers]]
name        = "custom-openrouter-ai"
kind        = "openai"
base_url    = "https://openrouter.ai/api/v1/"
models      = ["deepseek/deepseek-v4-flash", "deepseek/deepseek-v4-pro"]
default     = "deepseek/deepseek-v4-pro"
api_key_env = "CUSTOM_OPENROUTER_AI_API_KEY"
context_window = 128000   # tokens; compaction triggers near this limit
...
```

Then just add your OpenRouter API key as `CUSTOM_OPENROUTER_AI_API_KEY=sk-...`
to `~workshop/reasonix/.env`.

Other providers should follow a similar pattern, providing they offer a
compatible endpoint.

Configuration and secrets are written under `~/.reasonix`, which is mounted from
the host and therefore persists across workshop updates.

### Verify from the command line

```bash
workshop shell
reasonix version
```

This prints the installed Reasonix version, confirming the binary is on the
`PATH`.

---

## Plugs (resources this SDK consumes)

### `reasonix-home`

- Interface: `mount`
- Workshop target: `/home/workshop/.reasonix`
- Purpose: Persists the Reasonix home directory — `config.toml`, provider
  secrets in `.env`, session history, checkpoints, and subagent profiles —
  across workshop updates.

## Slots (resources this SDK provides)

This SDK doesn't define any slots.

---

## Documentation and guidance

- [Reasonix Guide](https://github.com/esengine/DeepSeek-Reasonix/blob/main-v2/docs/GUIDE.md)
- [Reasonix CLI reference](https://github.com/esengine/DeepSeek-Reasonix/blob/main-v2/docs/CLI.md)
- [Workshop documentation](https://ubuntu.com/workshop/docs/)

---

## Community and support

- Reasonix community: [Discord](https://discord.gg/XF78rEME2D) and
  [GitHub Discussions](https://github.com/esengine/DeepSeek-Reasonix/discussions)
- Workshop forum: [Discourse](https://discourse.ubuntu.com/)
- Please review our
  [Code of Conduct](https://ubuntu.com/community/ethos/code-of-conduct) before
  participating.

---

## Contributions

All contributions, including code, documentation updates, and issue reports,
are welcome!

- See [CONTRIBUTING.md](https://github.com/esengine/DeepSeek-Reasonix/blob/main-v2/CONTRIBUTING.md)
  for guidelines.
- Open issues or pull requests on the
  [official repository](https://github.com/esengine/DeepSeek-Reasonix).

---

## License and copyright

Copyright 2026 Canonical Ltd.

This SDK is distributed under the MIT License.

Reasonix is licensed under the
[MIT License](https://github.com/esengine/DeepSeek-Reasonix/blob/main-v2/LICENSE).
