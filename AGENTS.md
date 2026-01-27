# Repository Guidelines

## Project Structure & Module Organization
This repository is documentation-first and currently contains the MAiDAS specification and translated READMEs.
- Core docs live at the repository root: `README.md` and `SPECIFICATION.md`.
- Translations follow the pattern `README.<lang>.md` and `SPECIFICATION.<lang>.md` (for example, `README.ja.md`).
- There is no `src/` or `tests/` directory yet; treat Markdown files as the primary artifacts.

## Build, Test, and Development Commands
There is no formal build pipeline. Use fast, local checks before opening a PR.
- List Markdown files: `rg --files -g "*.md"`
- Quick word-count sanity check: `wc -w README.md SPECIFICATION.md`
- Optional linting (if available): `markdownlint "**/*.md"`

## Coding Style & Naming Conventions
Keep edits simple, consistent, and spec-focused.
- Use ATX headings (`#`, `##`, `###`) and short, scannable sections.
- Prefer bullet lists over dense paragraphs.
- Use fenced code blocks with language hints (for example, ```markdown).
File naming conventions:
- English source files: `README.md`, `SPECIFICATION.md`
- Translations: `README.<lang>.md`, `SPECIFICATION.<lang>.md`
- When updating structure in the English sources, mirror the same headings in translations.

## Testing Guidelines
Testing here means document quality and consistency.
- Check internal links and referenced paths (for example, `SPECIFICATION.md`).
- Ensure examples are copy/paste safe and match the described rules.
- For translation updates, verify key headings, lists, and tables remain aligned with the English source.

## Commit & Pull Request Guidelines
Match the existing history: short, imperative, and scoped.
Good commit message examples:
- `Add multilingual README translations (20 languages)`
- `Clarify schema actions in SPECIFICATION.md`
PRs should include:
- A brief summary of what changed and why.
- A list of affected files (especially translations).
- Notes on any intentional deviations from the English source.

## Security & Configuration Tips
Do not add secrets, tokens, or private URLs. Use neutral example domains and clearly mark placeholders (for example, `https://example.com`).

## Agent-Specific Local Rules
If present, treat `AGENTS.local.md` as mandatory, local-only guidance. Keep it out of Git via `.gitignore` and use it for rules that must always be followed on this machine.
