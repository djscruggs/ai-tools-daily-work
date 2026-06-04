I've built a deeply integrated Claude Code environment that goes well beyond chat. It functions as an automated engineering co-pilot across my entire workflow.

**Custom agent fleet.** I maintain ~20 specialized sub-agents organized by role: domain-expert code reviewers (TypeScript, Python, Rails, security, performance), research agents (framework docs, git archaeology, best-practices synthesis), design agents (Figma-to-implementation comparison, iterative UI refinement), and workflow agents (bug reproduction, spec analysis, migration safety review). Rather than asking a generalist for everything, I route tasks to purpose-built agents with the right context and tools.

**Default mode and session discipline.** My default mode is `acceptEdits` — Claude proposes and executes file edits without prompting for each one. But I cycle modes deliberately within a session: Plan mode first (Claude explores and proposes, cannot execute), then default mode for execution with review, then acceptEdits for trusted mechanical stretches, then back to Plan at each new decision point. The key is matching oversight level to confidence rather than picking one mode and staying there. I also run separate terminal instances for separate tasks — each Claude instance gets one job and its own clean context. Sessions get named with `/rename` so they're resumable via `/resume`.

**Context management.** For long sessions I use `/compact [instructions]` rather than bare `/compact` — the instructions tell Claude what to treat as load-bearing before it compresses (open questions, architectural decisions, constraints like "don't touch the auth module"). Without instructions, compaction is lossy and drops exactly the details that matter. This directly prevents goal drift across long sessions.

**Effort and thinking settings.** Extended thinking is off by default (`alwaysThinkingEnabled: false`); effort level set to `medium`. I toggle thinking manually for hard reasoning tasks rather than paying the cost on every prompt.
**Automated guardrails via hooks.** My Claude config runs shell hooks at key moments outside the LLM loop — reliable because the harness executes them, not Claude:

- **Prettier** (PostToolUse on Edit/Write/MultiEdit for `.ts tsx js jsx json css md`) — auto-formats every file Claude touches
- **TypeScript check** (PostToolUse on `.ts tsx` if `tsconfig.json` exists) — runs `tsc --noEmit` immediately after any TS change
- **Pre-commit size check** (PreToolUse matching `git commit` or `git push`) — enforces file size limits
- **direnv** (CwdChanged) — reloads env vars when the working directory changes
- **Audio ping** (Stop) — `afplay Ping.aiff` signals when Claude finishes a long task

**Plugins.** Four plugins enabled: `pg@aiguide` (PostgreSQL docs and query guidance), `code-simplifier` (post-implementation simplicity review), `typescript-lsp` (TypeScript language server), and `obsidian@obsidian-skills` (vault operations: markdown, canvas, CLI, Bases).

**Persistent memory across sessions.** A spaced-repetition memory system (Vestige) retains project context, preferences, and past decisions across conversations. Claude checks it at session start and updates it proactively. I also maintain a file-based memory layer in the vault itself — structured notes on user preferences, project state, and feedback that Claude reads at the top of any session.

**Structured skill library.** Reusable workflow skills handle multi-step operations as single commands. Core skills: `/commit-push` (commit + type-check + push), `/onboard` (auto-explores a new repo and suggests relevant agents), `/thinking-partner` (challenges assumptions, applies mental models), `/db-practices` (loads DB SDLC standards; verifies repo remote before activating — no-op in the wrong repo), `/db-lint` (installs and runs DB's eslint config; auto-triggers before commits in DB repos), `/spec`, `/review-pr`, `/security-review`, `/notebooklm`, `/tend` (vault maintenance). Extra skill marketplaces registered: bradfeld (whats-new), forrestchang (karpathy-skills), kepano (obsidian-skills).

**Offline docs lookup with BLZ.** Rather than web-browsing for documentation, I point Claude at locally cached `llms.txt` sources for every framework I use. BLZ returns ranked, line-accurate citations from those corpora. No flaky web fetches, no hallucinated API signatures.

**MCP integrations.** Claude has direct tool access to GitHub, Linear, Google Calendar/Drive/Gmail, Slack, Notion, a PostgreSQL guide server, Artyfacts (persistent knowledge store), computer-use (desktop automation), and Vestige.

**Explicit engineering standards baked in.** My global CLAUDE.md enforces Red/Green TDD by default, requires an eval strategy before any AI-in-the-loop feature, mandates type checking before any task is called done, and enforces branch scope discipline so unrelated changes never sneak into a commit. Project-specific CLAUDE.md files extend this at the repo level — no copy-pasting instructions into the chat.

**Roughdraft.** For any task involving a plan, spec, or document that needs review, I write it as a Markdown file and open it in Roughdraft — a local Markdown viewer/editor that supports inline CriticMarkup comments. Rather than reading Claude's output in the terminal and typing feedback back as a prompt, I comment directly in the document (insertions, deletions, substitutions, highlights) and Claude reads those annotations when I'm done. This keeps the review artifact and the feedback co-located, and means Claude gets precise, structured instructions rather than vague prose. It's the biggest unlock in the DB workflow for anything requiring back-and-forth on a document.

**Digital Bazaar setup.** DB work lives under `~/VSCode/@digitalbazaar/`. A CLAUDE.md at that directory root loads DB's SDLC standards automatically for every session in the tree — no per-repo setup needed. It encodes exact commit message format (imperative tense, subject ≤50 chars, issue references), preferred libraries, tech stack constraints, PR review checklists, and privacy-by-design requirements for any feature touching personal data. A `new-engineer/` reference repo at that root holds onboarding docs (glossary, architecture overview, git setup for conditional identity switching) that Claude can read for context on any DB task.

**Deny rules.** Global config has 7 balanced network/destructive deny rules (wget, nc, scp, rsync, ftp, chmod, chown) to prevent accidental damage. The DB directory settings.json uses allow-before-deny for ssh, docker, and scoped curl across all DB repos — tighter where it matters, not globally restrictive.

**Session archaeology with Entire.** I use [Entire](https://entire.io/) to capture AI agent sessions alongside git commits. Each time I push code, Entire creates a searchable checkpoint linking the conversation to the resulting changes — the *why* behind any commit. Agents starting fresh on a project can draw on that history rather than starting from scratch.

**CLI tools.** I use and have built small CLIs that wrap APIs directly — personal commands for summarizing, asking questions, and processing text from the terminal without opening a chat interface. This follows the pattern Karpathy described: CLIs are "legacy" technology, which means AI agents can natively use and compose them via the full terminal toolkit. A script that wraps an API is immediately usable by both humans and agents.

**Custom statusline.** Claude Code's statusline is a shell script that renders a two-line Powerline-style HUD at the bottom of every terminal session — model name, token usage, active branch, and task state at a glance.

**Knowledge management in Obsidian.** I run Claude Code directly inside my Obsidian vault, where it maintains a structured wiki layer on top of raw notes: daily logs, meeting notes, research, and job search material. Claude ingests source files into synthesized wiki pages, cross-links related concepts, answers queries against the accumulated knowledge, and keeps an index and change log up to date. A `/tend` skill runs periodically to scan recent daily log entries, surface open threads, and propose wiki updates. It functions as a second brain that grows and stays current without manual curation.

**Care and feeding.** This system isn't static. I spend 30–60 minutes a week tuning it — updating agent instructions when I hit edge cases, adding new skills, pruning what doesn't earn its keep, and incorporating practices from the broader community. The upfront investment is real, but the compounding return makes it worthwhile.

The net effect: Claude handles the mechanical and analytical overhead while I stay focused on decisions and design. That division of labor only works because of what Kieran Klaassen calls the "human sandwich" — humans set the frame, AI executes, humans judge the output. The framing half requires genuine engineering judgment: pattern recognition built over 20 years of software development, sharpened by ongoing self-education. The system amplifies that judgment; it doesn't substitute for it.
