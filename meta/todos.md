---
type: meta
purpose: todo_master
---

# To-Dos — Master Dashboard

> **Single source of truth for all open work.**
> Tasks live in their `/notes` source files. This dashboard is a live query — never edit it directly.
>
> **How closure works (no sync needed):**
> - **In Obsidian:** click any checkbox here or in the source note → Tasks plugin writes to the source file automatically
> - **Via Bro:** during FASE 0 of daily processing, Bro greps for overdue/today tasks, asks you done/drop/snooze, and edits the source files directly
>
> Every path writes to the same place. The dashboard reflects reality in real time.

---

## ⚠️ Overdue — act now

```tasks
not done
due before today
sort by due
show urgency
```

---

## 🔥 Due today

```tasks
not done
due on today
sort by urgency
```

---

## 📅 Next 7 days

```tasks
not done
due after today
due before in 7 days
sort by due
group by due
```

---

## 🔍 Follow-ups — from daily enrichment

> These are open questions and verifications created by Bro during daily processing. They need your input to close.

```tasks
not done
tag includes #follow-up
sort by due
```

---


## 📋 No due date — backlog

> These have no deadline. Review weekly and either set a date or drop them.

```tasks
not done
no due date
sort by created reverse
```

---

## ✅ Closed this week

> Feedback loop: what actually got done. Bro reads this section to avoid re-creating closed items.

```tasks
done
done after 7 days ago
sort by done reverse
```

---

## How to use

**Creating a task** — anywhere in any `/notes` file:
```
- [ ] Action item 📅 2026-04-15 #project_x ⏫
```

**Priority emoji:** `⏫` high · `🔼` medium · `🔽` low

**Closing options:**
- `- [x]` = done (Tasks adds ✅ date automatically)
- `- [-]` = cancelled/dropped
- Change `📅 date` = snooze to new date

**Bro's role:** FASE 0 of every daily surfaces overdue + today tasks. Bro closes what you confirm done, updates dates for snoozes, marks drops. For everything else → check this dashboard.
