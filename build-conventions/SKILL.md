---
name: build-conventions
description: Defines how Gonçalo likes new personal-project code built — folder layout, config handling, testing, and CLI conventions — scaled by whether it's a quick script, a POC, or a full framework. Use this whenever starting a new personal script, POC, or framework, scaffolding a project's structure, or deciding design patterns/coding conventions for one of these — even if the user doesn't say "conventions" or "style guide" explicitly, e.g. "let's build a quick script to...", "I want to POC this idea", "set up a new framework for...", "how should I structure this project".
---

# Build conventions

Gonçalo's global CLAUDE.md only keeps two universal code rules now: readable/explicit code over clever one-liners, and user-facing error handling that never fails silently. Everything else about how he builds new personal code — logging vs `print`, secrets handling, project structure, git/README workflow, UI language, and (for frameworks specifically) OOP design patterns and naming conventions — lives here, scaled by how long-lived the code is meant to be. Where a rule differs by tier (secrets, logging, structure), that's intentional: a throwaway script and a reusable framework don't carry the same cost for skipping formality.

## Step 1: figure out the tier

Infer from context; ask once, briefly, only if genuinely unclear (don't re-ask if the user already said which it is):

- **Quick script** — a one-off utility or automation, not meant to be extended.
- **POC** — validating an idea or approach before committing to it; may get promoted to a framework later if it proves out.
- **Framework** — long-lived, reusable, likely to grow or be handed to others.

## Quick script

- Single `.py` file (or a couple, only if the logic genuinely needs splitting) — no package structure.
- Config/credentials: declare them as constants directly below the imports. No config file, no `.env`.
- No automated tests. It's throwaway code — a manual run is the validation.
- No CLI framework. Use `argparse` only if the script genuinely takes variable parameters at runtime; otherwise hardcode the constants and edit them directly when they need to change.
- Use `print`, not `logging`. This is throwaway, never-committed code — the overhead of a logging setup isn't worth it here.
- Use a venv if the script has external dependencies, but no `pyproject.toml`/`requirements.txt` ceremony — a one-line comment at the top listing what to `pip install` is enough.
- No README, no git init. It's a single file the user runs once or a handful of times — there's no "project" here to set up or document.
- Still apply the two universal rules from CLAUDE.md: readable code, user-facing error handling.

## POC

- `src`-style layout split by concern, e.g. `ingest.py`, `transform.py`, `main.py`. One script doing everything defeats the point of a POC meant to validate a specific approach — you want to see that approach in its own module.
- Config/credentials: same as quick scripts — constants directly below the imports, no external config file. A POC is proving an idea, not productionizing config management; don't let that become the thing you're building.
- Tests: a handful of targeted tests on the core logic the POC exists to validate — not full coverage. If the POC isn't proving out a specific risky piece of logic, it probably doesn't need tests at all.
- Use `print`, not `logging` — same reasoning as quick scripts. Switch to `logging` only if the POC gets promoted to a framework.
- CLI: only if the POC genuinely needs runtime parameters; `argparse` is enough, no need for a CLI framework.
- Venv: yes, set one up — a POC has real dependencies worth isolating.
- Git init + a README before adding features, same as any real project. The README should say what the POC is validating and how to run it — that's the part that won't be obvious later once it's sitting half-finished in a folder.

## Framework

- Real package: `src/<package_name>/` with `__init__.py`, modules split by concern, mirrored by a `tests/` package.
- Config: an external file — not necessarily `.env`, pick whatever fits (YAML is often the right call for structured config) — with a single constant or variable pointing at its location, loaded once at startup. Secrets still go through `.env`; this is for the rest of the config that isn't a secret.
- Tests: a real pytest suite covering the modules, written to serve two purposes at once — a developer runs it locally (`pytest`) to check a change didn't break anything, and the exact same suite runs as a CI/CD step. Don't write a test that only works in one of those two contexts (e.g. one that depends on a local file path that won't exist in CI).
- Logging: a dedicated logger scoped to the framework (e.g. `logging.getLogger("my_framework")`), not the root logger. The point is that Gonçalo can set his framework's log level to `DEBUG` without also getting `DEBUG`-level noise from underlying libraries it calls (Spark being the recurring one). Configure the framework's own logger explicitly; don't touch or rely on the root logger's level.
- CLI: `typer`, if the framework exposes commands at all. Don't add a CLI layer if nothing needs to be invoked from one.
- Venv + `pyproject.toml` for packaging and dependency metadata.
- Git init + a README before adding features — use the `design-docs` skill for the README's actual template (and for any design doc, ADR, or non-technical usage guide the framework needs).
- The two universal rules from CLAUDE.md apply with no shortcuts here either: readable code, user-facing error handling.

### Object-oriented structure

Classic OOP: one class per file. The only file allowed at the top level of the package is `main.py` (or its equivalent); every other file lives inside a folder that reflects what it's for.

The entry point follows the **facade pattern**: `main.py` exposes a handful of simple functions that hide all internal complexity. The public surface needs to stay small enough that user-facing documentation can say "call these functions" without anyone needing to read the internals — these frameworks are often used by non-technical clients or colleagues. Whatever's inside the package is free to be as complex as it needs to be; that complexity should never leak through the facade.

### Naming & typing

- Classes: `CamelCase`.
- Functions, methods, variables, and file names: `snake_case`.
- Constants: `UPPER_CASE_WITH_UNDERSCORES`.
- Private methods/variables/constants: prefix with a double underscore (`__like_this`), regardless of whether Python actually enforces it.
- Type hints on every function signature — public or private, top-level or deeply internal. No exceptions for "it's just a helper."
- Docstrings: Google style, on every class and function.

### Error handling

Errors are exceptions, not return codes or silent `None`s — this is the concrete mechanism behind the universal "never fail silently" rule. Let exceptions propagate and stop execution; don't catch one just to log it and carry on as if nothing happened.

## UI language

Personal-use apps/tools (any tier) default to Portuguese for anything user-facing — CLI help text, prompts, output messages. This doesn't apply to work-related tooling, which stays in English, or to code/identifiers (always English regardless of who the tool is for).

**Exception: logging.** Log messages default to English always, even in a personal-use Portuguese-UI tool and even when the user will actually see them during a run. Switch logging to Portuguese only when Gonçalo explicitly asks for it.

## Judgment calls

ETL/data-pipeline-specific patterns (connector abstractions, retry/idempotency wrappers, pipeline-as-steps) are deliberately **not** standardized here — Gonçalo prefers to decide those per project rather than force a template. Don't impose one; follow whatever the project's own code already does, or ask if it's a fresh build.

If a project seems to be growing past its current tier mid-build (a "quick script" that's accumulating config and tests, a POC turning into something that'll stick around), say so and ask whether to promote it rather than silently keeping it at the lower tier or silently upgrading it. Promotion to framework means actually doing the swap, not just adding structure around the old code: hardcoded/`getenv`-style credentials move to the proper secrets channel, `print` calls become calls through the framework's own named logger, and config constants move into the external config file.
