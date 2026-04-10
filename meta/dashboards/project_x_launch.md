---
type: dashboard
purpose: project_x_tracker
---

# 🏦 Project X Launch Tracker

## Active Project X projects
```dataview
TABLE status, target_date as "Target", file.mtime as "Last touched"
FROM "notes"
WHERE contains(tags, "project") 
  AND (contains(tags, "project_x"))
SORT target_date ASC
```

## Critical path — next 90 days
```tasks
not done
due before in 90 days
(tag includes #project_x)
sort by due
```

## Pending decisions
```dataview
LIST
FROM "notes"
WHERE contains(tags, "decision") AND contains(tags, "pending")
  AND (contains(tags, "project_x"))
```

## Applicable regulations
```dataview
TABLE jurisdiction, status
FROM "notes"
WHERE contains(tags, "regulation")
  AND (contains(applies_to, "project_x"))
```

## Stakeholders
```dataview
TABLE role, company
FROM "notes"
WHERE contains(tags, "person")
  AND (contains(tags, "project_x"))
```
