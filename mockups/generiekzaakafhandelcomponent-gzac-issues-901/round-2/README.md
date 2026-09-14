# gzac-issues#901 — Lay-out of tasks, round 2

<https://github.com/generiekzaakafhandelcomponent/gzac-issues/issues/901> — 14-09-2026.

Round 1 offered three options and all three were rejected. The reply settled the heading and reopened the
width, so this round varies only the width of the `Taken` column.

Rendered against the real `evenementenvergunning` app (backend + frontend from source, seeded demo data —
"Zwarte Cross"), on the case detail `Gegevens` tab, which is the widget tab with `showTasks: true`. Each option
is shot at two viewport widths because the reporter's complaint names small screens specifically.

**Settled in every option:** the `Taken` heading drops from 24px to 16px/24px — the size
`valtimo-widget-field__title` already uses for `Zaakgegevens` and the other widget headings. The subheadings
`Mijn taken` / `Overige taken` go to 14px so they still read as subordinate.

Measured column widths (task column / case content / one widget column), in CSS pixels:

| | 1440 px viewport | 1100 px viewport |
|---|---|---|
| `current` — fixed 412px | 412 / 676 / 346 | 412 / **336** / 352 |
| `option-a` — one widget column, scales | 363 / 725 / 371 | 374 / 374 / 390 |
| `option-b` — fixed 320px (`WIDGET_WIDTH_1X`) | 320 / 768 / 392 | 320 / 428 / 444 |
| `option-c` — 25% of the content area, floor 280px | 280 / 808 / 412 | 280 / 468 / 484 |

At 1100px the current task column is wider than the entire case content beside it.

Option C also moves the status tag under the task title: below roughly 300px the title and the tag cannot
share a row and the tag truncates to "O..". That is the floor on "iets smaller by default" as the task row is
built today.

Files are never overwritten — round 1 sits in the parent directory and the comment that links it still points
at its original commit.
