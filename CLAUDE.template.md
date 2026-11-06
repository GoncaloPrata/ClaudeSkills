# About [Your name]

- **Role:**
- **Based in:**
- **Experience:**
- **Stack:**
- **Scope:**
- **Native language:**
- **Uses Claude for:**

---

# Communication

- Use bullet points over prose where possible.
- Always use code blocks for code snippets.
- **Ask clarifying questions before writing any code for a new project or feature** — not just when something is ambiguous.
- Do not assume you're correct — disagree and explain why when warranted.
- For comparisons, use a pros/cons table when possible.
- **Top-down disclosure:** Start with a summary; go deeper only when asked. Do not dump everything at once.
- **Give a direct opinion when asked** — when asked "what do you think?" or "what should I target?", take a stance. Do not hedge with "it depends."
- **Do not re-introduce dropped topics.** If a topic is explicitly set aside, do not bring it back unless asked.

---

# Explanations

- Always include a concrete example when explaining a concept.
- Show a working code snippet, not just a description of the approach.
- If multiple approaches exist: show the preferred one with rationale + example, briefly mention alternatives.

---

# Code

- Readable, explicit code over clever one-liners.
- User-facing error handling — never fail silently.
- For anything more specific (logging vs print, secrets handling, project structure, git/README workflow, UI language) — see the `build-conventions` skill, which scales these by whether it's a quick script, POC, or framework.

---

# Obsidian

- **Vault location:** `~/Documents/ObsidianVault` (adjust to your own path)
- **Structure:** PARA method (folders) + Maps of Content (MOC notes) + internal `[[links]]`

```
ObsidianVault/
├── 00-Inbox/          # Quick capture, process weekly
├── 10-Projects/       # Active projects (one note or subfolder per project)
├── 20-Areas/          # Ongoing domains (career, health, finances)
├── 30-Resources/      # Reference material by topic
├── 40-Archive/        # Completed or inactive
├── _templates/        # Note templates — use for all new notes
├── _attachments/      # Images, PDFs
└── _MOCs/             # Index/navigation notes per domain
```

- **All notes follow a standard template** at `_templates/Note Template.md`: YAML frontmatter (`title`, `date`, `tags`, `type`, `status`) + `## Summary` + `## Content` + `## Links`.
- **When navigating the vault:** read frontmatter and `## Summary` first to assess relevance before reading the full note.
- **Max 2 folder levels deep** — use links and MOC notes for deeper navigation.
