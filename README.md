# report

A Claude Code slash command that writes a detailed, self-contained HTML report or explainer on any topic — investigation, comparison, design proposal, postmortem, study notes.

```
/report <topic / purpose> [category or path]
```

With no arguments, it reports on the main topic of the current conversation.

## What it does

1. Settles the purpose, reader, and the one question the report answers.
2. Gathers facts from the conversation, code (`git log`, real `path:line` quotes), files, the web, and commands it actually runs. Unverified claims are marked as such.
3. Writes only the sections that fit: TL;DR, background, body cards with code / diagrams / worked examples, comparison table, evidence, risks, next actions, references.
4. Outputs one HTML file — inline CSS/JS, dark mode, sticky TOC and headings, no horizontal page scroll, readable offline.
5. Saves to `~/dev/report/<category>/YYYY-MM-DD-<slug>.html` and opens it.

## Install

```bash
curl -fsSL https://raw.githubusercontent.com/f42gh/report/main/report.md -o ~/.claude/commands/report.md
```

## Customize

- **Language:** the prose is written in Japanese. Change the line `Write the prose in Japanese.` in `report.md`.
- **Design:** the look follows `~/dev/design/DESIGN.md` (the author's personal design rules, not included). Point it at your own file or remove the line.
- **Output location:** change `~/dev/report/` in step 5.

## License

MIT
