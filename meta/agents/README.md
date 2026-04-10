# Agents

> **What lives here.** Specialized agent definitions. Each agent is a system prompt that gives Bro a specific role, mindset, constraints, and toolset for a particular type of work.
>
> **Status: empty scaffold.** Bro and user do not need specialized agents yet. The two existing prompts (`daily_processing.md` and `weekly_refactoring.md` in `meta/prompts/`) are de facto agents — just not formalized as such.

## When to add an agent here

Create an agent when you notice you're repeatedly invoking Bro with the same elaborate setup for the same type of work. Signals:
- "Every time I need to write a regulatory memo, I have to re-explain the structure and tone"
- "When I prep for a board meeting, the briefing instructions are always the same"
- "When I review an outsourcing contract, I follow the same checklist"

These are agent triggers.

## Format

Each agent is a single markdown file. Suggested structure:

```markdown
# Agent — [Name]

## Identity
Who this agent is. What they are an expert in.

## When to invoke
"Bro, agent X, do Y" — concrete triggers.

## Mindset & priorities
What this agent cares about. What they prioritize when there's tension.

## Hard rules
What this agent must never do.

## Tools and skills used
- skill: [link to skills/...]
- template: [link to templates/...]

## Output format
What the agent's output looks like.

## Example invocation
A real example of how user calls this agent and what comes back.
```

## How invocation works

In a session, user writes:
> Bro, sei l'agente `regulatory_memo_writer`. Carica il file `meta/agents/regulatory_memo_writer.md` come istruzioni di sistema aggiuntive. Esegui [task].

Bro reads the agent file and behaves accordingly for that session.

## Relationship to skills

An agent **uses** one or more skills. Skills are passive knowledge ("how to do X"). Agents are active roles ("be the person who does X with these priorities").

Example:
- Skill `regulatory_memo.md` = the structural rules and quality criteria for a regulatory memo
- Agent `regulatory_memo_writer.md` = the mindset, the paranoid attitude on citations, the tone, the questions to always ask user before drafting

Same domain, different layer.

## Built-in implicit agents

These already exist as prompts and don't need to be re-created here:
- **Daily Processor** → `meta/prompts/daily_processing.md`
- **Weekly Refactorer** → `meta/prompts/weekly_refactoring.md`
