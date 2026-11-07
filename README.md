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

- **CLAUDE.template.md** — a work-machine version of Claude Code's global `CLAUDE.md`: same Communication/Explanations/Code sections as the personal one, but the "About" section is trimmed to what's functionally useful at work (role, experience, stack, scope) and drops location/native-language, since those only fed personal-tool decisions (UI language, etc.) that don't apply there. The real, personal `CLAUDE.md` stays local-only (not in this repo, not symlinked). On a new work machine: copy this to `~/.claude/CLAUDE.md` as-is; on a new personal machine, copy it and add back `Based in` / `Native language` and change "Uses Claude for" to personal projects/research.
