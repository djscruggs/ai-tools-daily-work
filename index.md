I've built a deeply integrated Claude Code environment that goes well beyond chat. It functions as an automated engineering co-pilot across my entire workflow.

**Custom agent fleet.** I maintain ~20 specialized sub-agents organized by role: domain-expert code reviewers (TypeScript, Python, Rails, security, performance), research agents (framework docs, git archaeology, best-practices synthesis), design agents (Figma-to-implementation comparison, iterative UI refinement), and workflow agents (bug reproduction, spec analysis, migration safety review). Rather than asking a generalist for everything, I route tasks to purpose-built agents with the right context and tools.

**Automated guardrails via hooks.** My Claude config runs shell hooks at key moments: Prettier auto-formats every file Claude touches, pre-commit type and size check. direnv reloads environment variables when the working directory changes, and an audio ping signals when Claude finishes a long task.

**Persistent memory across sessions.** A spaced-repetition memory system (Vestige) retains project context, preferences, and past decisions across conversations. Claude checks it at session start and updates it proactively.

**Structured skill library.** Reusable workflow skills (/commit-push, /onboard, /spec, /notebooklm, /thinking-partner) handle multi-step operations as single commands. The /onboard skill, for example, auto-explores an unfamiliar repo and suggests the right agents for that codebase.

**MCP integrations.** Claude has direct tool access to GitHub, Linear, Google Calendar/Drive/Gmail, Slack, Notion, and a PostgreSQL guide server.

**Explicit engineering standards baked in.** My global instructions enforce Red/Green TDD by default, require an eval strategy before any AI-in-the-loop feature, mandate type checking before any task is called done, and enforce branch scope discipline so unrelated changes never sneak into a commit.

**Session archaeology with Entire.** I use [Entire](https://entire.io/) to capture AI agent sessions alongside git commits. Each time I push code, Entire creates a searchable checkpoint linking the conversation to the resulting changes i.e. the *why* behind any commit. This means I can review the full agent reasoning that produced any code change weeks or months later, and agents starting fresh on a project can draw on that history rather than starting from scratch. 

**CLI tools.** I use and have built small CLIs that wrap  APIs directly — personal commands for summarizing, asking questions, and processing text from the terminal without opening a chat interface. This follows [the pattern Karpathy described on X](https://x.com/karpathy/status/2026360908398862478): CLIs are "legacy" technology, which means AI agents can natively use and compose them via the full terminal toolkit. A script that wraps an API is immediately usable by both humans and agents.

**Knowledge management in Obsidian.** I run Claude Code directly inside my Obsidian vault, where it maintains a structured wiki layer on top of raw notes such as daily logs, meeting notes, and research. Claude ingests source files into synthesized wiki pages, cross-links related concepts, answers queries against the accumulated knowledge, and keeps an index and change log up to date. Combined with Obsidian-specific skills for markdown, canvas, and CLI interaction, it functions as a second brain that grows and stays current without manual curation.

The net effect of all this is that Claude reliably handles the mechanical and analytical overhead of software development while I stay focused on decisions and design.
