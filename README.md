# plugin-process

Process-presence probing for OpenCharly — the `process:` check verb.

The verb matches a process by exact name (`pgrep -x`) against a live deployment.
It is a host-coupled, probe-only check verb (no act/step role) whose `RunVerb`
runs against the live check engine (`sdk/kit.CheckContext`), so it is
**compiled-in only**.

## What it provides

| Capability | Surface |
|---|---|
| `verb:process` | the `process:` check verb — assert a named process is running (`process:`, `running:` defaulting to `true`) |

## How to use it

Compose the plugin candy in a box or check bed's `candy:` list:

```yaml
- '@github.com/opencharly/plugin-process/candy/plugin-process:<tag>'
```

Then author the verb in a plan:

```yaml
- check: the charly probe process is absent
  id: process-absent
  process: {process: charly-absent-probe, running: false}
  context: [runtime]
```

## Layout

- `candy/plugin-process/` — the plugin module: `plugin.go` (the `verb` +
  `NewCheckVerb()`/`NewMeta()`), `schema/process.cue` (the self-contained
  `#ProcessInput`), `params/cue_types_gen.go`, `cmd/serve/main.go`.
- `charly.yml` — the root project manifest (`discover: candy`).
- `.github/workflows/tag-on-merge.yml` — CalVer tag + `CHANGELOG/` on merge.

## Related

- Owning skill: `/charly-check:check` — the check verb catalog. This candy
  carries no `skill:` entity of its own; the gap is tracked in
  [opencharly/opencharly#291](https://github.com/opencharly/opencharly/issues/291).
- `/charly-internals:plugin` — the plugin/provider model.
- [`opencharly/charly`](https://github.com/opencharly/charly) — the charly CLI.
