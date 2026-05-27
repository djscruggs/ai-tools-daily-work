I've built a deeply integrated Claude Code environment that goes well beyond chat. It functions as an automated engineering co-pilot across my entire workflow.

**Custom agent fleet.** I maintain ~20 specialized sub-agents organized by role: domain-expert code reviewers (TypeScript, Python, Rails, security, performance), research agents (framework docs, git archaeology, best-practices synthesis), design agents (Figma-to-implementation comparison, iterative UI refinement), and workflow agents (bug reproduction, spec analysis, migration safety review). Rather than asking a generalist for everything, I route tasks to purpose-built agents with the right context and tools.

**Automated guardrails via hooks.** My Claude config runs shell hooks at key moments: Prettier auto-formats every file Claude touches, a pre-commit hook enforces type checking and file size limits, direnv reloads environment variables when the working directory changes, and an audio ping signals when Claude finishes a long task.

**Persistent memory across sessions.** A spaced-repetition memory system (Vestige) retains project context, preferences, and past decisions across conversations. Claude checks it at session start and updates it proactively. I also maintain a file-based memory layer in the vault itself — structured notes on user preferences, project state, and feedback that Claude reads at the top of any session.

**Structured skill library.** Reusable workflow skills (`/commit-push`, `/onboard`, `/thinking-partner`, `/db-practices`) handle multi-step operations as single commands. `/onboard` auto-explores an unfamiliar repo and suggests the right agents for that codebase. `/db-practices` loads project-specific engineering standards and verifies the repo context before activating — it's a no-op in the wrong repo.

**Offline docs lookup with BLZ.** Rather than web-browsing for documentation, I point Claude at locally cached `llms.txt` sources for every framework I use. BLZ returns ranked, line-accurate citations from those corpora. No flaky web fetches, no hallucinated API signatures.

**MCP integrations.** Claude has direct tool access to GitHub, Linear, Google Calendar/Drive/Gmail, Slack, Notion, and a PostgreSQL guide server.

**Explicit engineering standards baked in.** My global CLAUDE.md enforces Red/Green TDD by default, requires an eval strategy before any AI-in-the-loop feature, mandates type checking before any task is called done, and enforces branch scope discipline so unrelated changes never sneak into a commit. Project-specific CLAUDE.md files in individual repos add context that's always loaded — no copy-pasting instructions into the chat.

**Session archaeology with Entire.** I use [Entire](https://entire.io/) to capture AI agent sessions alongside git commits. Each time I push code, Entire creates a searchable checkpoint linking the conversation to the resulting changes — the *why* behind any commit. Agents starting fresh on a project can draw on that history rather than starting from scratch.

**CLI tools.** I use and have built small CLIs that wrap APIs directly — personal commands for summarizing, asking questions, and processing text from the terminal without opening a chat interface. This follows [the pattern Karpathy described](https://x.com/karpathy/status/2026360908398862478): CLIs are "legacy" technology, which means AI agents can natively use and compose them via the full terminal toolkit. A script that wraps an API is immediately usable by both humans and agents.

**Custom statusline.** Claude Code's statusline is a shell script that renders a two-line Powerline-style HUD at the bottom of every terminal session — model name, token usage, active branch, and task state at a glance.

**Knowledge management in Obsidian.** I run Claude Code directly inside my Obsidian vault, where it maintains a structured wiki layer on top of raw notes: daily logs, meeting notes, research, and job search material. Claude ingests source files into synthesized wiki pages, cross-links related concepts, answers queries against the accumulated knowledge, and keeps an index and change log up to date. It functions as a second brain that grows and stays current without manual curation.

The net effect: Claude reliably handles the mechanical and analytical overhead of software development — and knowledge work — while I stay focused on decisions and design.
