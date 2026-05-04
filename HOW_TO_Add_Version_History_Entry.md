# Adding a Version History Entry to a Workflow Card

When you change anything in a card on `Diagram_Beta_Pilot_Manual.html` or `Workflow_Macro_Visual.html`, add a corresponding entry to that card's `data-versions` attribute. Future-you and the team need to know what changed, when, and who decided.

Both docs use the same schema, so this guide applies to both.

## The Pattern

Each card has a `data-versions` attribute containing JSON. It looks like this:

```html
<div class="sc sc-pdm"
     data-who="..."
     data-what="..."
     data-why="..."
     data-versions='[{"v": "1.0", "date": "April 2026", "change": "Initial documentation", "owner": "Anne"}]'
     onclick="openCard(this)">
```

## Adding a New Entry

When making a change, append a new entry to the JSON array. Bump the version number using semver-ish rules:

- **v1.x → v2.0** for major rewrites (card scope changes, role ownership flips)
- **v1.0 → v1.1** for terminology updates, clarifications, language tightening
- **v1.0 → v1.0.1** for typo fixes (optional — usually not worth a history entry)

### Example — adding a v1.2 entry

Before:
```json
[{"v": "1.0", "date": "April 2026", "change": "Initial documentation", "owner": "Anne"},
 {"v": "1.1", "date": "May 2026", "change": "Renamed from Execute carve-out...", "owner": "Anne"}]
```

After (added Henrique's v1.2 entry):
```json
[{"v": "1.0", "date": "April 2026", "change": "Initial documentation", "owner": "Anne"},
 {"v": "1.1", "date": "May 2026", "change": "Renamed from Execute carve-out...", "owner": "Anne"},
 {"v": "1.2", "date": "June 2026", "change": "Added clarification on Hedge sizing for sub-25hr requests", "owner": "Henrique"}]
```

## Field Conventions

| Field | Format | Example |
|---|---|---|
| `v` | major.minor (or major.minor.patch) — **no `v` prefix; the badge adds it** | `"1.1"`, `"2.0"` |
| `date` | Month YYYY | `"May 2026"` |
| `change` | Past tense, what changed and (optionally) why | `"Renamed from Execute carve-out — language updated to Approval Checkpoint terminology"` |
| `owner` | First name | `"Anne"`, `"Henrique"` |

## Description Quality

Good descriptions answer "what would future-me need to know to understand this change":

✅ `"Split DM-owned 5-step mechanic into PDM (epic + scope) and DM (child tickets, Hedge) per pilot retro feedback"`

❌ `"Updated card"`

✅ `"Renamed Hedge column to Buffer to match billing terminology agreed with Tina"`

❌ `"Wording change"`

If a change is small and self-explanatory (typo fix, formatting), it usually doesn't need a history entry at all.

## JSON Quoting Gotchas

The attribute uses **single quotes** for the outer wrapping:
```html
data-versions='[...]'
```

So inside the JSON, double quotes work normally:
```html
data-versions='[{"v": "1.0", "change": "Standard text"}]'
```

If your description needs a literal double quote (e.g. quoting a renamed term), escape it with `\"`:
```html
data-versions='[{"change": "Renamed from \"Execute carve-out\""}]'
```

If your description needs an apostrophe (e.g. "DM's responsibility"), use the HTML entity `&#39;` or rephrase:
```html
data-versions='[{"change": "Updated DM responsibility section"}]'
```

## Verifying Your Edit

Before pushing to GitHub, open the file in a browser and click the affected card. The Version History toggle should expand cleanly when you click it. If it doesn't expand or shows nothing, your JSON didn't parse — check the browser console (Cmd+Opt+J on Mac) for the error.

## Where the History Lives

- **Inline in each card** — the `data-versions` attribute is the source of truth
- **Rendered in the modal** — collapsible "Version History" section at the bottom of the Who/What/Why popup
- **Not synced anywhere else** — Git commit history is separate; this is the operational log for the team

---

*Doc created May 2026, updated to align Beta Pilot and Future-State workflow card schemas.*
