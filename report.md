---
description: Write a detailed, rich HTML report or explanation for any purpose (investigation, explainer, comparison, design proposal, postmortem, study notes)
argument-hint: <topic / purpose> [category or path]
---

# Rich HTML Report

Write a detailed report or explanation on the topic/purpose given in the arguments: $ARGUMENTS

If no arguments are given, report on the main topic of the current conversation. Save it as one self-contained HTML file under `~/dev/report/<category>/` (see step 5). Write the prose in Japanese. Keep code, identifiers, paths and proper nouns as they are.

## 1. Decide the purpose and reader

Before writing, settle these in one line each (put them in the header too):

- **Purpose**: what kind of report this is. Examples: explainer (how X works), investigation (why X happens), comparison / selection (A vs B), design proposal, postmortem, study notes, summary of a codebase or document.
- **Reader**: who reads it and what they already know. Default: the user, a few months later, with no memory of this conversation.
- **Question**: the one question the report answers. The conclusion must answer it.

## 2. Gather facts

Use whatever sources fit the purpose:

- The conversation context.
- Code: read the relevant files, `git log` / `git show` / `git diff`, so you can quote real code with `path:line`.
- Files the user pointed at (docs, PDFs, data).
- The web (WebSearch / WebFetch) when the topic needs outside facts. Record the URLs.
- Run things (commands, small scripts, benchmarks) when a claim can be checked. Show the command and the real output.

Do not invent anything. Mark each claim you could not verify as unverified. Separate facts from your opinion.

## 3. Content

Pick the sections that fit the purpose. Leave out any that would be empty. Order them in the order the reader needs them.

1. **Header**: date, author, purpose, reader, question, and a one-line headline. Add project path / branch / commits when code is involved.
   - **Author (著者)**: put it right next to the date. Write the model developer, model name and reasoning effort of the agent writing the report, as `<developer> · <model> · effort <level>`. Use only what you actually know about yourself from your own context; write `不明` for any part you cannot tell, never guess.
2. **TL;DR / conclusion**: 3–5 bullets. Answer the question first.
3. **Background / premises**: only what the reader needs to follow the body. Define terms once.
4. **Body**: the main part, as one card per topic. Use whatever each topic needs:
   - Explanation: what it is, how it works, why it is that way.
   - Real code or data excerpts (≤ ~30 lines each), syntax-highlighted, with source.
   - A diagram when structure, flow, state or time matters (inline SVG or HTML/CSS). Draw the real mechanism, not decoration.
   - A worked example: concrete input → step → output.
   - Tables for anything with more than two items to compare.
5. **Comparison / options**: option → pros → cons → when to pick it. Use a table. End with a recommendation and its reason.
6. **Evidence**: commands, measurements, error text, quotes, each with where it came from.
7. **Risks / caveats / unknowns**: what may be wrong, what was not checked, limits of the conclusion.
8. **Next actions**: concrete steps, each one line.
9. **References**: links and files used.

Use collapsible `<details>` for long material most readers can skip (full logs, long code, derivations). Keep the main path short.

## 4. HTML requirements

- One file, all CSS and JS inline, no build step. The only external resources allowed are Google Fonts and libraries from cdnjs (`https://cdnjs.cloudflare.com/ajax/libs/`): highlight.js 11.9.0 (light and dark themes), and when needed KaTeX for math or Mermaid for diagrams. The page must still read fine offline (text and tables intact without them).
- `<html lang="ja">`, `<meta charset="utf-8">`, viewport meta, `<title>` = `YYYY-MM-DD · <headline>`.
- Put colours in `:root` tokens with a `prefers-color-scheme: dark` variant. Give `body` an explicit background.
- Layout: a readable column (~760px max; wide tables and diagrams may break out to ~1100px), a sticky table of contents on wide screens, sticky section headings, cards for body topics. Code blocks and wide tables scroll horizontally on their own; the page itself never scrolls sideways.
- Interactivity only where it helps understanding: tabs to switch between options, a toggle to switch views, `<details>`, hover/click to highlight a diagram part. Every motion is a result of a user action.
- Look: follow `~/dev/design/DESIGN.md` (colours, fonts, lines, copy). A monospace font may appear only inside `pre`/`code`.
- Escape `<`, `>` and `&` in all code.
- Add `<meta name="report:author">` (same text as the author field), `<meta name="report:purpose">`, `<meta name="report:question">` and, when code is involved, `<meta name="report:project">` so later tooling can grep them.

## 5. Save

Save to `~/dev/report/<category>/YYYY-MM-DD-<slug>.html`.

- **category**: the main subject area, as one short kebab-case word (e.g. `frontend`, `claude-code`, `infra`, `data`, `study`). Run `ls ~/dev/report` first and reuse an existing category when one fits. Create a new one only when none does.
- **slug**: 2–4 kebab-case English words for the headline.
- If the user names a path or category in the arguments, use theirs.

```bash
CAT=<category> SLUG=<slug> && D=~/dev/report/$CAT && mkdir -p "$D" && \
F="$D/$(date +%Y-%m-%d)-$SLUG.html" && N=2 && \
while [ -e "$F" ]; do F="$D/$(date +%Y-%m-%d)-$SLUG-$N.html"; N=$((N+1)); done && echo "$F"
```

Write the HTML to that path with the Write tool. Then run `open <path>` to show it.

## 6. Report

Reply with the saved path and a 2–3 line summary: the question and the conclusion.
