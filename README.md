# claude-usage-tips

A Claude Code plugin that ends each turn with **one very short tip** on using
AI better, picked for what you just typed, and **measures** which tips you
actually adopt so the weak ones can be rewritten or deleted.

```
💡 Paste the exact error and command; a paraphrase makes the agent guess.
```

The tip is a `systemMessage`: you see it, the model does not, so it costs no context.

## Install

```
/plugin marketplace add fyodoriv/claude-usage-tips
/plugin install usage-tips@claude-usage-tips
```

Needs `bash` and `jq`. Silence it for one shell with `HOOK_BYPASS_USAGE_TIP=1`.
[agentbrew](https://github.com/fyodoriv/agentbrew) users get the same hook
built in: list `usage-tip` under `enabled:` in the hooks overlay.

## How a tip is picked

Tips live in [`hooks/tips.tsv`](hooks/tips.tsv): id, trigger, adopt regex, text.

| Trigger | Fires when your last prompt… |
|---|---|
| `short` | is under 40 characters and not a slash command |
| `correction` | starts with "no", "wrong", "still broken", … |
| `error` | reports a failure but holds no error text |
| `long` | belongs to a transcript over 400 lines |
| `any` | always |

**Never a repeat.** A tip already shown never comes back while any tip that
fits is still unshown. The history is the whole log, across sessions. Once
every fitting tip was shown, the one shown longest ago comes next
(oldest-first rotation). Among unshown tips, a matching specific trigger wins,
then file order. A tip you already adopted never shows again, and the same tip
never shows twice in a row.

## In the status line

Each `shown` row also holds the tip text, so a status line can show the
current tip. agentbrew's `agent-perf.mjs statusline` prints it on a second line.

## Measure and improve

Each shown tip appends a row to `~/.cache/agentbrew/usage-tips/log.jsonl`
(`USAGE_TIPS_DIR` moves it): time, session id, tip id, event. Never prompt text.
When a later prompt in the session matches a shown tip's adopt regex (for
example `^/compact` after the compact tip), it logs `adopted`.

```bash
bash ~/.claude/plugins/*/usage-tips/scripts/report.sh   # or scripts/report.sh in a clone
```

prints shown, adopted, and adoption rate per tip. Adoption is a proxy, not
proof the tip caused it. PRs with better tips, or with your report numbers,
are welcome (see [CONTRIBUTING.md](CONTRIBUTING.md)): add a row to `hooks/tips.tsv` and a case to `tests/usage-tip.test.sh`.

## Test

```bash
bash tests/usage-tip.test.sh
```
