# World Models: Publication Dashboard

Live views over the paper notes' frontmatter (`citation_count`, `venue`, `date`, `research_group`). Requires the Dataview community plugin in Obsidian. Tables update when notes change. Papers with a null `citation_count` (not yet re-checked against Semantic Scholar) are excluded from sums and averages.

---

## 1. Citations per group

```dataview
TABLE WITHOUT ID
  research_group AS "Research group",
  length(rows) AS "Papers",
  sum(rows.citation_count) AS "Total citations",
  round(average(rows.citation_count)) AS "Avg citations",
  max(rows.citation_count) AS "Top paper citations"
FROM ""
WHERE research_group
GROUP BY research_group
SORT sum(rows.citation_count) DESC
```

## 2. Papers per group

```dataview
TABLE WITHOUT ID
  research_group AS "Research group",
  length(rows) AS "Papers",
  rows.file.link AS "Papers in group"
FROM ""
WHERE research_group
GROUP BY research_group
SORT length(rows) DESC
```

## 3. Papers per venue

```dataview
TABLE WITHOUT ID
  venue AS "Venue",
  length(rows) AS "Papers",
  rows.file.link AS "Papers"
FROM ""
WHERE venue
GROUP BY venue
SORT length(rows) DESC
```

> Venue strings are free text, so the same venue can appear under several labels (for example `ICLR 2026` and `ICLR 2026 (poster)`). Normalize them in the notes if you want clean totals.

## 4. Papers sorted by citation count

```dataview
TABLE WITHOUT ID
  file.link AS "Paper",
  research_group AS "Group",
  venue AS "Venue",
  citation_count AS "Citations"
FROM ""
WHERE citation_count
SORT citation_count DESC
```

## 5. Papers sorted by date

```dataview
TABLE WITHOUT ID
  date AS "Date",
  file.link AS "Paper",
  research_group AS "Group",
  venue AS "Venue"
FROM ""
WHERE date
SORT date DESC
```

---

## Extras

### Top 10 most cited

```dataview
TABLE WITHOUT ID
  file.link AS "Paper",
  research_group AS "Group",
  citation_count AS "Citations"
FROM ""
WHERE citation_count
SORT citation_count DESC
LIMIT 10
```

### Papers per publication year

```dataview
TABLE WITHOUT ID
  dateformat(date, "yyyy") AS "Year",
  length(rows) AS "Papers",
  sum(rows.citation_count) AS "Citations"
FROM ""
WHERE date
GROUP BY dateformat(date, "yyyy")
SORT dateformat(date, "yyyy") DESC
```

### Papers still needing a citation re-check

Null or zero citation counts. Zero counts are included, so a few real zeros will also show up.

```dataview
TABLE WITHOUT ID
  file.link AS "Paper",
  research_group AS "Group",
  citation_count AS "Citations"
FROM ""
WHERE research_group AND !citation_count
SORT research_group ASC
```

### Papers by venue type (conference vs. arXiv only)

```dataview
TABLE WITHOUT ID
  file.link AS "Paper",
  venue AS "Venue"
FROM ""
WHERE venue AND contains(lower(venue), "arxiv")
SORT venue ASC
```
