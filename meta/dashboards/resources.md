---
type: dashboard
purpose: resources_browser
---

# 📚 Resources

> Browse, filter, and verify the health of source documents in `/resources/`.

## All resources (most recent first)
```dataview
TABLE 
  resource_kind as "Kind",
  issuer as "Issuer",
  status as "Status",
  date_published as "Published"
FROM "resources"
WHERE contains(tags, "resource")
SORT created DESC
```

## By kind

### Regulations
```dataview
TABLE issuer, reference, status, date_effective as "Effective"
FROM "resources"
WHERE contains(tags, "resource") AND contains(tags, "regulation")
SORT date_effective DESC
```

### Competitor intelligence
```dataview
TABLE issuer as "Company", date_published as "Published"
FROM "resources"
WHERE contains(tags, "resource") AND (contains(tags, "competitor") OR resource_kind = "competitor_report")
SORT date_published DESC
```

### Advisory & reports
```dataview
TABLE issuer, date_published
FROM "resources"
WHERE contains(tags, "resource") AND (contains(tags, "advisory") OR resource_kind = "advisory_report")
SORT date_published DESC
```

## Health checks

### Resources with no linked derived notes (needs extraction)
```dataview
LIST
FROM "resources"
WHERE contains(tags, "resource") AND (linked_notes = null OR length(linked_notes) = 0)
```

### Recently added (last 14 days)
```dataview
LIST
FROM "resources"
WHERE contains(tags, "resource") AND created >= date(today) - dur(14 days)
SORT created DESC
```

### Resources by status
```dataview
TABLE length(rows) as "Count"
FROM "resources"
WHERE contains(tags, "resource")
GROUP BY status
```

## How to use this dashboard

- Click any resource to open its `__meta.md` companion
- The meta-file points to the actual artifact via the `artifact` frontmatter field
- The "Sections to deep-read for current work" inside each meta-file tells you exactly what to rebrowse
- For 90% of queries, the **derived notes** in `/notes` are enough. Open the original artifact only when you need exact wording for a memo or a citation.
