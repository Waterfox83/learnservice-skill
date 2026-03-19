# 📚 LearnService — AI-Powered Codebase Tutor for Claude Code

Stop reading outdated READMEs. Let AI teach you any microservice, chapter by chapter.

## What It Does

LearnService is a skill for [Claude Code](https://claude.ai/code) that turns any repository into an interactive course. Point it at a service directory and it will:

- **Read the actual source code** — not just the README
- **Generate a tailored course** — chapters based on what's actually in the repo
- **Teach interactively** — conversational prose, not bullet dumps
- **Quiz you as you go** — one question at a time, grounded in real code
- **Open files in your editor** — click a file reference, it opens right there in VS Code
- **Save your progress** — close and come back exactly where you left off
- **Export your notes** — generate a personal reference doc with the chapters you choose

It's the difference between reading documentation and actually learning a codebase.

---

## Demo

```
/learnservice ./order-service

> I've finished reading the order-service repo. Here's your course:
>
> 1. Overview & Mental Model          ← start here
> 2. API Surface (12 endpoints)
> 3. Data Model (Postgres, 4 tables)
> 4. Core Order Processing Logic
> 5. Infrastructure & Ops (Docker + SQS)
>
> ⚠️ README is partially outdated — I'll call out conflicts as we go.
>
> Ready to start Chapter 1?
```

---

## Installation

```bash
git clone https://github.com/yourname/learnservice-skill \
  ~/.claude/skills/learnservice
```

That's it. Claude Code picks up skills from `~/.claude/skills/` automatically.

**Requirements:** [Claude Code](https://claude.ai/code) with an Anthropic API key, or a GitHub Copilot subscription via VS Code.

---

## Usage

| Command | What happens |
|---------|-------------|
| `/learnservice <path>` | Start a new course on a repo |
| `/learnservice continue` | Resume where you left off |
| `/learnservice status` | See progress across all courses |
| `/learnservice reset` | Start a course over |
| `export doc` | Generate a personal reference doc |
| `export doc chapters 1, 3` | Export specific chapters only |
| `export doc + this conversation` | Include your deep-dive Q&A in the doc |
| `add this to my doc` | Append the current conversation to your notes |

During a chapter you can say things like:
- *"go deeper on the auth flow"* — tutor dives in, then returns to the chapter
- *"I already know this, skip"* — jumps to the quiz
- *"explain that again"* — re-explains with a different analogy
- *"open that file"* — opens the referenced file in your editor

---

## How It Works

LearnService reads your repo intelligently — prioritizing READMEs, entry points, route files, data models, and config over generated or utility files. It never blindly trusts documentation:

> "⚠️ README says this service uses MySQL, but docker-compose.yml and the migration files show PostgreSQL. I'll teach what the code actually does."

Chapters are tailored to the repo. No database? No data model chapter. Heavy event-driven architecture? It adds an events chapter. The course reflects the actual service, not a generic template.

Every file reference in the teaching content is openable directly in your VS Code editor. When the tutor says *"see how the JWT filter works at `src/auth/JwtFilter.java:34`"* — you can open it with one click.

---

## Progress & Notes

Progress is saved to `.learnbot_progress.json` in the repo directory, so it travels with the code. You can commit it, share it with teammates, or `.gitignore` it — your call.

Exported notes are saved to `learnservice-notes.md` in the repo:

```markdown
# Order Service — Personal Reference
Generated: 2025-01-16 | Chapters: 1, 3, 4

## Overview & Mental Model
- Handles the full order lifecycle: create → pay → fulfill → ship
- Built on Node.js + Express, Postgres via Prisma, SQS for fulfillment events
- Entry point: src/server.ts, starts on port 3000

## My Notes
**Q:** Why does the order status use a state machine instead of simple flags?
**A:** Prevents invalid transitions — can't go from CANCELLED back to PENDING.
       Enforced in src/domain/OrderStateMachine.ts.
```

---

## Multi-Service Architecture

Want a bird's-eye view of all your services at once? Use Claude Code's subagent capability:

```
Read all service directories under ./services and spawn one subagent per
service to analyze it in parallel. Synthesize into an architecture-overview.md
covering: service inventory, dependency map, data ownership, and event flows.
```

Each subagent runs independently, then the results are merged into a single architecture document. Good starting point before diving into individual service courses.

---

## What It Won't Do

- **It can't run your code** — it reads and reasons about source files only
- **It can't guarantee accuracy** — it clearly flags when it's inferring vs. when it directly read something. Treat it as a knowledgeable guide, not an authoritative reference
- **It works best on codebases under ~200 files** — larger repos get smart sampling but may miss things

---

## Contributing

PRs welcome. The most useful contributions:

- **Better repo-signals.md** — framework detection patterns, new languages
- **Better quiz-patterns.md** — question templates for specific chapter types
- **Real-world feedback** — open an issue describing a repo where the skill struggled and why

---

## How This Was Built

This skill was designed iteratively in conversation with Claude, then refined based on real usage across multiple enterprise microservices. The full design conversation — covering architecture decisions, VS Code extension implementation, anti-hallucination strategies, and the export feature — is what produced the SKILL.md you're reading.

The companion **VS Code extension** (in `/vscode-extension`) provides the same experience with a purpose-built UI: chapter sidebar, streaming content, clickable file references, and quiz cards — no CLI required.

---

## License

MIT
