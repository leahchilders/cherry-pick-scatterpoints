# Cherry-Pick UI

Extremely small local tool for hand-picking a visually pleasing subset of points from a scatter plot and exporting it as a CSV that mirrors the input. No install, no server, no internet, just a singleself-contained `index.html` that runs in your browser.

## Why?

I had a bunch of scatterplots whose data I accumulate after each new run, in order to to try to equally space the points *visually* on the curve even as the deriative changes severely. LLMs were unhelpful and the algorithms I sampled still produced ugly plots. The human eye is really good at picking points that look visually appealing, so now it's a UI instead.

## Run it

Double-click `index.html`, or from a terminal:

```sh
xdg-open index.html  # Linux
```

## Load data

- Click **Load CSV…** or **drag a CSV file anywhere** onto the window.
- An import dialog appears where you set:
  - **Delimiter** (auto-sniffed: comma / tab / semicolon)
  - **First row is a header** (auto-detected; headers become axis labels)
  - **X column** and **Y column** (default to the 1st and 2nd columns)
  - **X / Y scale** — linear or log (use log for data spanning several decades)
  - **Start with** all points kept, or none kept
- If some rows are malformed (non-numeric/empty X or Y, wrong column count), the dialog warns you and lists the first few; they're dropped on import.
  <img width="799" height="181" alt="image" src="https://github.com/user-attachments/assets/ed7ff415-4cc9-4e70-a296-ae997f6c1e05" />
Works with any CSV, headers or not, two or more columns. Only the chosen X/Y columns are plotted, but **every** column is preserved on export.

## Pick points

Two panels stay in sync via one shared "kept" flag per point:

- **All data** (left): every point. Kept = solid blue, dropped = hollow grey.
  **Click a point to toggle** it in/out of the subset.
- **Kept subset** (right): only kept points. **Click a point to remove** it.
  (Removed points reappear in the left panel, where you can add them back.)

Navigation: **drag** to pan, **scroll** to zoom, **Reset view** to refit. *Shared zoom* (on by default) keeps both panels framed identically.

Below the panels:

- **X / Y column** and **X / Y scale** can be changed anytime.
- **Default new** sets what *Reset* returns to.
- **Select all / Clear all / Invert / Reset** — bulk actions; destructive ones ask first.

## Export

Click **Export ▾**:

- **Order** — keep original order, or sort by X or Y (ascending/descending).
- **Set** — the kept subset, or the dropped complement.
- **File** — output name (defaults to `<original>_subset.csv`).

The exported CSV matches the input exactly: same delimiter, same header (or none), all original columns, and the original cell text — so numeric precision is preserved. Only the rows you kept are written, in the order you chose.
