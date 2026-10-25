# AGENTS.md

A Claude Code plugin: a Stop hook prints one short usage tip per turn and logs shown and adopted tips.

- Hook: `hooks/usage-tip.sh`. Tips: `hooks/tips.tsv`. Report: `scripts/report.sh`.
- Gate: `bash tests/usage-tip.test.sh` (no CI; run it before every commit).
- Tips must name real Claude Code features. Never invent one.
- agentbrew has a copy of the hook and tips; change both.
- Bug or idea in this plugin? Offer the user a [safe report](CONTRIBUTING.md#report-problems-and-ideas).
