# Skills

> **What lives here.** Reusable expertise — "how to do X". A skill is a passive reference document that Bro consults when performing a recurring type of task.
>
> **What does NOT live here.** Templates (those are in `meta/templates/`). Templates are empty schemas. Skills are procedures.

## Distinction: skill vs template vs agent

| | What it is | Active or passive |
|---|---|---|
| **Template** | An empty form to fill in (e.g., `note_atomic.md` placeholder) | Passive |
| **Skill** | A procedure / set of rules / quality criteria for doing a thing well (e.g., "how to write a regulatory memo") | Passive (consulted) |
| **Agent** | A specialized role with mindset, tone, constraints (e.g., "the regulatory memo writer agent") | Active (invoked) |

A skill is **what** to do. A template is **what to fill**. An agent is **who** does it.

## When to create a skill

Create a skill when you notice you're explaining the same procedure to Bro twice. Skills are crystallized experience.

## Format

Each skill is a single markdown file. Suggested structure:

```markdown
---
type: skill
domain: [domain name]
created: YYYY-MM-DD
---

# Skill — [Name]

## Purpose
What this skill is for. When to apply it.

## When to use
Concrete triggers — what situations call for this skill.

## Procedure
Step by step. Numbered. Specific.

## Quality criteria
What "done well" looks like. Checklist.

## Anti-patterns
What NOT to do. Common mistakes.

## Examples
Real examples (anonymized if needed) of good output.

## Related skills / templates
- 
```

## Skills planned (to be built when needed)

Based on user's role and recurring work:
- `regulatory_memo.md` — how to structure a regulatory memo (BdI/CSSF style, citation rigor, sections)
- `board_slide.md` — [Company] board slide style and structure
- `product_teardown.md` — how to analyze a payment product (unit economics, flows, risks, compliance)
- `outsourcing_contract_review.md` — EBA/GL/2019/02 checklist for outsourcing contract review
- `stakeholder_email.md` — internal/external stakeholder email patterns

**Don't pre-create these.** Build each one the first time you need it, with user's input on what good looks like. That way they're grounded in real practice, not theory.
