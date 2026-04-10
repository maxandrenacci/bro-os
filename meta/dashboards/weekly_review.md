---
type: dashboard
purpose: weekly_review
---

# 📅 Weekly Review

## Notes touched this week
```dataview
TABLE file.mtime as "Modified"
FROM "notes"
WHERE file.mtime >= date(today) - dur(7 days)
SORT file.mtime DESC
```

## New notes this week
```dataview
LIST
FROM "notes"
WHERE created >= date(today) - dur(7 days)
SORT created DESC
```

## Files with pending APPENDs (need consolidation)
*(Bro identifies these via grep on `<!-- APPEND` markers during weekly)*

## Tag distribution
```dataview
TABLE length(rows) as "Count"
FROM "notes"
FLATTEN tags as tag
GROUP BY tag
SORT length(rows) DESC
```

## To-dos closed this week
```tasks
done after 7 days ago
sort by done
```

## To-dos created this week
```tasks
created after 7 days ago
sort by created
```
