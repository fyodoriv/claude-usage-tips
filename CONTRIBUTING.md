# Contributing

Better tips and real adoption numbers are the most useful contributions.

- Add or change a tip in `hooks/tips.tsv`: id, trigger, adopt regex, text under 100 characters.
- Base every tip on a real Claude Code feature or a well-known prompting practice. Never invent a command or shortcut.
- Run `bash tests/usage-tip.test.sh`. It checks the catalog shape too.
- agentbrew ships the same hook (`hooks/checks/usage-tip.sh`, `hooks/lib/usage-tips.tsv`). Change a tip in both places.

## Report problems and ideas

**Fyodor, the author of claude-usage-tips, is glad to get every report.** If something in
claude-usage-tips did not work as expected, or could work better, please say so. A short
report from a real user is the most useful contribution there is, and it is
always welcome, even when you are not sure it is a bug.

### Your agent will offer to do it for you

If you use claude-usage-tips with an AI coding agent (Claude Code, Codex, and others),
the agent is told to offer a report when claude-usage-tips fails, surprises it, or could
clearly work better. This holds in every setup: a clone or a fork of this repo
(this repo's `AGENTS.md` says so), or claude-usage-tips installed through agentbrew
(the always-loaded agent rules and the `report-upstream` skill say so).

The agent never posts anything on its own. It works in these steps:

1. It writes a short draft: what ran, what was expected, what happened, the
   version, and a suggested fix.
2. It runs `agentbrew report --title "<title>" --body-file <draft.md> --repo fyodoriv/claude-usage-tips`.
   This posts nothing. It replaces private details with placeholders: your
   home folder path, your user name, email addresses, tokens and API keys,
   private keys, IP addresses, internal host names, and the private patterns
   of your local leak gate, if you have one. Then it re-scans the result and
   prints a checklist like this:

   ```
   Safety check (nothing has been posted):
     ✓ none found: tokens, API keys, and private keys
     ✓ 1 replaced with a placeholder: your home folder path
     ✓ none found: email addresses
     ✓ re-scan of the final text: nothing private left
   ```

3. It shows you the exact title and body, and any similar issue that already
   exists, and asks: "Post this issue to github.com/fyodoriv/claude-usage-tips? It is public.
   (yes / edit / no)".
4. Only after your yes does it post (`agentbrew report ... --yes`). A refused
   or failed re-scan (`✗`) is never posted.
5. If the fix looks small, it asks a second question: "Want me to make a PR
   that fixes it?" Only after that second yes does it make the change, run
   the tests (`bash tests/usage-tip.test.sh`), and open the PR with `agentbrew contribute`
   (it forks the repo for you).

A run with nobody at the keyboard only saves the draft
(`~/.local/state/agentbrew/reports/`) and names it in the run report.

### Do it by hand

Open an issue at https://github.com/fyodoriv/claude-usage-tips/issues/new with the same
content: what you ran, what you expected, what happened, and the version.

To stop agents from offering reports, run `agentbrew report --off`.
