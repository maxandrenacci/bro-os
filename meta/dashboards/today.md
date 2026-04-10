---
type: dashboard
purpose: daily_brief
---

# 📊 Today

## ⚠️ Overdue
```tasks
not done
due before today
sort by due
```

## 🔥 Today
```tasks
not done
due on today
```

## 📅 Next 3 days
```tasks
not done
due after today
due before in 3 days
sort by due
```

## 📥 Inbox to process
```dataview
LIST
FROM "inbox"
WHERE !contains(file.path, "_archive")
SORT file.ctime DESC
```

## 📝 Notes modified in last 48h
```dataview
TABLE file.mtime as "Modified", tags
FROM "notes"
WHERE file.mtime >= date(today) - dur(2 days)
SORT file.mtime DESC
LIMIT 15
```

## 🎯 Active projects
```dataview
TABLE target_date as "Target", file.mtime as "Last touched"
FROM "notes"
WHERE contains(tags, "project") AND contains(tags, "active")
SORT target_date ASC
```

## ❓ Open questions for me
*(extracted from memory.md and notes)*
```dataview
LIST
FROM "notes"
WHERE contains(tags, "follow-up")
LIMIT 10
```
