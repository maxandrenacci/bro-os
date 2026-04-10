# bro-os

> Personal operating system for a busy product professional.
> Built on Obsidian, operated by Bro (Claude) via Claude Code.

---

## What this is

A personal knowledge system designed to:
1. Capture unstructured input (meetings, PDFs, voice notes, screenshots) without friction
2. Process it into a queryable, atomic knowledge base
3. Surface what matters proactively (to-dos, deadlines, open questions)
4. Generate context-aware outputs (memos, slides, analyses) without re-explaining context every time
5. **Tune itself** over time via empirical meta-review

This is NOT a project management tool. It does not replace Jira, Confluence, or other corporate systems. It is your *personal* operational layer.

---

## The fundamental contract

- **You** capture raw input throughout the day. Zero processing.
- **Bro** processes it in the evening with you in the loop. Asks smart questions when uncertain. Never hallucinates.
- **Bro** refactors the system every Friday with your approval. Proposes structural improvements.
- **The system** adapts its own rules based on empirical evidence.

---

## Prerequisites — before anything else

### 1. Claude account

bro-os requires a paid Claude plan.
- **Minimum:** Claude Pro (individual)
- **Recommended:** Claude Team (higher message limits per 5-hour window, better for heavy vault sessions)
- Sign up at **[claude.ai](https://claude.ai)**

### 2. Claude Code

Claude Code is the primary interface for bro-os. It runs on your machine, reads and writes files directly, and executes the `/daily` and `/weekly` slash commands.

- Download and install from **[claude.ai/code](https://claude.ai/code)**
- Available as desktop app (Mac/Windows), CLI, or IDE extension (VS Code, JetBrains)
- Official docs: [docs.anthropic.com/en/claude-code](https://docs.anthropic.com/en/claude-code)
- Requires an active Claude Pro or Team subscription

> **Claude.ai chat (Projects)** is a secondary option for quick, read-only queries when you don't need file writes. For daily processing, weekly refactoring, and any task that touches files — Claude Code is required.

### 3. Obsidian

bro-os is an Obsidian vault. You need Obsidian to navigate the knowledge base, use dashboards, and click task checkboxes.

- Download from **[obsidian.md](https://obsidian.md)** (free for personal use)

---

## Day 1 — setup (~30 min)

### Step 1 — Move the vault to its permanent location

```bash
mv ~/Downloads/bro-os ~/Documents/bro-os
```

Pick a stable path. Once set, don't move it — Claude Code and Obsidian both reference the absolute path.

### Step 2 — Open in Obsidian

- Open Obsidian → **Open folder as vault** → select `~/Documents/bro-os`
- Trust the author when prompted

### Step 3 — Install required plugins

Settings → Community plugins → turn off Safe mode → Browse, then install and enable:

| Plugin | Why |
|---|---|
| **Dataview** | Powers all dashboards — non-negotiable |
| **Tasks** | To-do management with due dates and live queries |
| **Templater** | Dynamic templates with date variables |
| **Periodic Notes** | Daily notes automation (optional but recommended) |

After install: Templater settings → Template folder location → set to `meta/templates`.

### Step 4 — Open Claude Code and run signup

```bash
cd ~/Documents/bro-os
claude
```

Or open the Claude Code desktop app and point it at `~/Documents/bro-os`.

When Claude Code starts, it reads `CLAUDE.md` automatically and becomes Bro. Run the onboarding command:

```
/signup
```

Bro will ask you 7 questions in a single message (role, priorities, projects, stakeholders, urgent items). You answer in free text — no format required. Bro then populates `meta/memory.md` and creates the first stub notes for your key entities. The vault is ready in ~5 minutes.

### Step 5 — Set the Friday reminder

Google Calendar → recurring event:
- Title: `bro-os weekly`
- Every Friday, 16:00, 15 min block
- Description: *"Open Claude Code at bro-os root. Run `/weekly`."*

### Step 6 — Read `NAVIGATION.md`

Open `NAVIGATION.md` in the vault root and read it once. It's the Obsidian navigation cheatsheet — how to consult the vault without calling Bro. The golden rule is there: **Obsidian is free and instant. Bro costs messages. Consultation → Obsidian. Synthesis/generation → Bro.**

---

## How bro-os uses Claude

| Mode | What it is | When to use |
|---|---|---|
| **Claude Code** | Desktop/CLI app with direct file access | Daily processing, weekly refactoring, heavy tasks (long PDFs, batch file ops) |
| **Claude.ai Projects** | Web chat with vault context pasted as instructions | Quick queries, drafting, when you're on a phone or don't have Code open |

**Claude Code is the default.** Slash commands `/daily` and `/weekly` execute the full pipeline in one command — no copy-pasting prompts.

**Claude.ai Projects** is the fallback. Set it up by creating a new Project on claude.ai and pasting the contents of `CLAUDE.md` as the project instructions. Useful for ad-hoc questions, but it cannot write files — outputs have to be copy-pasted back manually.

---

## Daily routine

### During the day — capture (zero friction)

Every input goes to `/inbox/`. No classification, no tagging, no linking.

- Drop files with any recognizable name: `meeting_contact_a.md`, `eba_pdf.md`, `idea_funding.md`
- Bro normalizes naming and archives on the next daily session
- If you're in a rush: even a single sentence in a `.md` file is enough

### Evening — processing (~10-15 min)

```bash
cd ~/Documents/bro-os
claude
/daily
```

Bro executes a 4-phase pipeline:

1. **FASE 0 — Todo triage.** Bro surfaces all overdue and today's tasks. You say done/drop/snooze. Bro closes them in the source files. Reminds you to check `meta/todos.md` for the rest.
2. **FASE 1 — Context understanding.** Reads each inbox file in full. Forms an interpretation hypothesis. Asks blocking questions if needed (max 3 per file).
3. **FASE 2 — Enrichment.** Auto-deduces links, projects, tags. Creates follow-up to-dos for non-blocking gaps.
4. **FASE 3 — Processing.** Appends/creates `/notes` files, tags, links, archives originals.

At the end: Bro produces a session report. Review it, then update `meta/usage_log.md`.

---

## Weekly routine (~20-30 min, Friday)

```
/weekly
```

Bro executes 10 sections **in order**, proposing before doing. For each proposal: "ok", "skip", or "modify like this".

| Section | What Bro does |
|---|---|
| 1 — Consolidation | Integrates accumulated APPENDs into clean prose |
| 2 — Deduplication | Identifies mergeable files, proposes merges |
| 3 — Splitting | Flags oversized files, proposes splits |
| 4 — Links & MOC | Reinforces bidirectional links, proposes new MOCs for mature tags |
| 5 — Taxonomy review | Merges, splits, or eliminates theme tags based on usage |
| 6 — **Meta-review** | Evaluates if system rules still fit the actual vault — proposes adjustments |
| 7 — Resources health | Audits orphan artifacts, dead meta-files, unused resources |
| 8 — Usage review | Analyzes session cost trends, proposes structural optimizations |
| 9 — Memory regeneration | Rewrites `meta/memory.md` with current state |
| 10 — To-do health | Rolls up overdue, undated, orphan, and conflicting to-dos |

At the end: final weekly report. Done. The vault is clean, rules are tuned, memory is fresh.

---

## Monthly hygiene (~10 min)

First Monday of each month:
1. Archive last month's inbox: `/inbox/_archive/YYYY-MM/` is safe to zip
2. Quick scan of `meta/usage_log.md` for cost trends
3. Quick scan of `meta/taxonomy.md` for tag drift

---

## How to ask Bro for things

Open Claude Code at the vault root. Bro reads `CLAUDE.md` + `meta/memory.md` automatically.

**"Bro, prepare a brief on Product_A margins for the board meeting on April 20."**
→ Bro queries notes tagged `#product` + `#economics` + `product_a`, reads relevant files, drafts a brief in the appropriate register, asks if you want a `__meta.md` companion.

**"Bro, what's heating up that I might be missing?"**
→ Bro reads `meta/memory.md`, scans recent daily reports, surfaces stalled items, overdue to-dos, and themes that have grown without attention.

**"Bro, draft an email to Contact_A asking for the Q1 numbers breakdown by tier."**
→ Bro reads Contact_A's note to recall context (tone, history, what you've already discussed), drafts the email in the appropriate register.

**"Bro, give me a 3-slide structure for tomorrow's Project X workstream review."**
→ Bro queries `#project_x` notes, structures slides by workstream, proposes content per slide.

**"Bro, I just got this PDF (47 pages of regulatory guidance). Process it."**
→ Claude Code handles this natively. Bro will extract key obligations, classify as a resource or extract-only, create the `__meta.md`, and link to relevant notes.

---

## Key files to know

| File | What it's for | Who touches it |
|---|---|---|
| `CLAUDE.md` | Bro's operating manual | Rarely — only via approved meta-review |
| `NAVIGATION.md` | Obsidian navigation cheatsheet | Read once, keep as reference |
| `meta/memory.md` | Current state of your work | You can edit manually; Bro regenerates weekly |
| `meta/taxonomy.md` | Living tag registry | Bro updates; you approve changes at weekly |
| `meta/usage_log.md` | Session usage history | Update after each session |
| `meta/todos.md` | Master to-do dashboard | Read often — never edit directly |
| `meta/dashboards/today.md` | Morning brief | Open every morning in Obsidian |
| `meta/dashboards/project_x_launch.md` | Project X tracker | When context-switching to Project X |
| `meta/dashboards/resources.md` | Source documents browser | When you need an authoritative citation |

---

## What to expect in the first 4 weeks

**Week 1.** Lots of questions from Bro during daily processing. Vault is empty — no context to deduce from. Sessions take 15-20 min. Normal: you're bootstrapping.

**Week 2.** Questions decrease as the vault gains anchors. Sessions stabilize around 12-15 min. First weekly is light (not much to refactor yet) but establishes baselines.

**Week 3-4.** The system starts paying back. Bro starts connecting things you'd forgotten. Taxonomy stabilizes around 10-15 theme tags. Daily sessions drop to 10 min.

**Month 2.** First meaningful meta-review. Bro proposes adjustments to size thresholds based on what the vault actually looks like. First emergent MOCs appear.

**Month 3+.** The system becomes invisible infrastructure. You stop thinking about it. You just capture and ask.

---

## Anti-patterns — don't do these

1. **Don't write directly into `/notes`.** That's Bro's domain. Your hand goes to `/inbox` only.
2. **Don't skip evening processing for >2 days in a row.** The inbox becomes overwhelming and you'll be tempted to skip it permanently.
3. **Don't pre-create theme tags.** Let them emerge.
4. **Don't pre-create skills or agents.** Build them when you notice you're explaining the same thing twice.
5. **Don't use bro-os for things that belong in Jira/Confluence.** This is your personal layer, not the corporate one.
6. **Don't accept Bro's proposals without reading them.** The system depends on your judgment in the loop, especially in weekly refactoring.
7. **Don't skip the usage_log update.** Without it, the cost optimization loop breaks.

---

## When something feels wrong

If a part of the system feels heavy, slow, or annoying — that's signal, not noise. Tell Bro at the next weekly refactoring:

> "Bro, in section 6 (meta-review), I want to flag that [X] has been friction this week. What do you propose?"

The meta-review section exists exactly for this. The system is supposed to evolve.

---

## Versioning

- **v1.2** (2026-04-07) — initial system, lean structure, emergent taxonomy, 3-phase daily pipeline, self-tuning weekly refactoring.
- **v1.3** (2026-04-07) — added `/resources` folder with mandatory `__meta.md` companion files. New `#resource` type tag. Resources dashboard. Daily extended with resource sub-flow. Weekly extended with Resources health check. Added `NAVIGATION.md`.
- **v1.4** (2026-04-10) — todo lifecycle redesign: `meta/todos.md` rewritten as master dashboard with live queries (overdue, today, by project, follow-ups, recently closed). Added FASE 0 to daily pipeline (todo triage before inbox). Added `#follow-up` as fixed workflow tag. Daily/weekly time estimates updated.

Future versions tracked here when system rules change via meta-review.

---

## A note from Bro

This system works only if you trust the loop:
- You capture, you don't process during the day.
- I process, I don't decide for you in the weekly.
- The system tunes itself, but only with your approval.

Skipping any of the three breaks the contract.

If you find yourself wanting to "just edit a note quickly" — fine, do it, but tell me at the next session so I can keep state consistent. The worst failure mode is silent drift between what's in the vault and what you actually believe.

Let's go.

— Bro
