# WARP.md

This file provides guidance to WARP (warp.dev) when working with code in this repository.
``

Repository type and stack
- This repository is an Obsidian Markdown knowledge base (no application build, lint, or test tooling detected).
- Top-level organization follows a PARA-like structure with daily notes and prompt templates.

Commonly used commands (macOS, zsh)
- Repository status and recent diffs
  - View status: git --no-pager status
  - Show recent changes (last 10 commits): git --no-pager log --oneline -n 10
  - Show unstaged changes: git --no-pager diff
  - Show files changed in the last week: git --no-pager log --since="1 week ago" --name-only --pretty="format:"

- Search across notes (Markdown only)
  - Case-insensitive search for a phrase: grep -Rin --include "*.md" "<search term>" .
  - Search by tag-like token (e.g., #design): grep -Rin --include "*.md" "#design" .

- Daily notes helpers (folder: 05 Daily Notes)
  - Open the latest daily note: open "05 Daily Notes/$(ls -1t "05 Daily Notes" 2>/dev/null | head -n1)"
  - Create/open today’s note (MM-DD-YYYY.md): today=$(date +%m-%d-%Y).md; mkdir -p "05 Daily Notes"; : > "05 Daily Notes/$today"; open "05 Daily Notes/$today"

- Quick folder stats (how many markdown notes per area)
  - Find top-level markdown counts: find . -maxdepth 2 -type f -name "*.md" | awk -F/ '{print $2}' | sort | uniq -c | sort -nr

High-level architecture and structure
- 0 Index
  - Navigation and curation hub (e.g., Index, Templates, Categories). Useful starting point to discover canonical references.
- 01 Inbox
  - Capture/triage for new notes. Content here is typically ephemeral until moved into Projects, Areas, or Resources.
- 02 Projects
  - Time-bounded outcomes. Use when work has a clear end state; related materials and decisions should consolidate here.
- 03 Areas
  - Ongoing responsibilities or themes (no fixed end date). Use for long-lived domains where knowledge accrues over time.
- 04 Resources
  - Reference materials by topic (articles, clippings, research). Non-actionable content organized for retrieval.
- 05 Daily Notes
  - Chronological notes named as MM-DD-YYYY.md. Often contain scratch thoughts, logs, and links back to Projects/Areas.
- Prompt Base/
  - Prompt and role templates to guide AI outputs (e.g., “Role • Growth Product Manager.md”). These shape tone and goals for generated content.
- copilot-custom-prompts/
  - GitHub Copilot custom prompt templates (short, action-focused utilities like “Summarize.md”, “Fix grammar and spelling.md”). These are reusable snippets for editor workflows.
- .smart-env/
  - Smart plugin configuration for the vault. Notable settings: embeddings (TaylorAI/bge-micro-v2 via transformers), Obsidian-specific flags, and chat tooling configured via an “ollama” adapter.
- .smart-chats/
  - Chat transcripts and related artifacts created by the Smart tooling.
- .obsidian/
  - Obsidian’s application configuration for this vault.

Notes for Warp Agents
- There is no build system, linter, or test suite in this repository. Do not attempt to run build, lint, or test commands.
- When generating or organizing content:
  - Prefer placing time-bounded work under 02 Projects and ongoing themes under 03 Areas; keep reference materials in 04 Resources.
  - Use 01 Inbox for quick capture; promote or refactor into Projects/Areas/Resources during triage.
  - Link related notes to improve retrievability from index and searches.
- When searching or summarizing, scope to “*.md” to avoid non-content directories (e.g., .obsidian/, .smart-env/, .smart-chats/).
