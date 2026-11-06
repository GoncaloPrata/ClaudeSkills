# Claude Skills

Gonçalo's personal Claude Code skills — how he likes frameworks, scripts, POCs, and their docs built.

## Structure

One folder per skill, each containing a `SKILL.md`, matching Claude Code's own skill layout:

```
<skill-name>/
└── SKILL.md
```

## How this stays in sync with Claude Code

Each skill folder here is the single source of truth. It's symlinked from `~/.claude/skills/<skill-name>` into this repo — not copied — so editing the skill in either location edits the same file. There's nothing to keep in sync by hand.

## Skills

- **build-conventions** — folder layout, config handling, testing, CLI, and OOP/naming conventions for new personal scripts, POCs, and frameworks, scaled by tier.
- **design-docs** — templates for design docs, ADRs, READMEs, and non-technical usage guides for frameworks, produced bilingually (English + Portuguese).

## Other files

- **CLAUDE.template.md** — a sanitized starting point for Claude Code's global `CLAUDE.md`, with the identifying bio details and local paths blanked out. The real `CLAUDE.md` stays local-only (not in this repo, not symlinked) since it has personal details this repo doesn't need to carry. On a new machine: copy this to `~/.claude/CLAUDE.md`, fill in the "About" section, and adjust the Obsidian vault path.
