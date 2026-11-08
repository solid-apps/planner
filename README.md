# Planner

A day planner with a 24-hour board: plan blocks in one column, record what you actually did in another.

Live: https://solid-apps.github.io/planner/

## Features

- A 24-hour board with two built-in columns, Plan and Did.
- Create a block by dragging on empty column space. Drag a block to move it, including into another column, and drag its top or bottom edge to resize it.
- Zoom levels: 24h, 12h, 6h and Focus. Times snap to 30, 15, 15 and 5 minutes respectively.
- Edit a block's title, start, end, column, notes and done state in an edit sheet.
- "✓ did" copies a block into the Did column, marked done.
- Add your own columns, then rename, recolor, reorder or delete them. Plan and Did can't be deleted, and a column that still has blocks can't be deleted.
- Delete a block with its ✕ button or the Delete/Backspace key. Ctrl+Z (⌘Z on Mac) undoes the last delete.
- Move between days with previous/next buttons and a Today button. When today's now-line is off-screen, a chip scrolls back to it.
- Live updates from the pod, when the server supports them.
- Installable as a PWA (`manifest.json`).

## Your data

- **Signed out:** everything works. Data is saved in the browser's `localStorage` under keys prefixed with `planner:`. There is one key for the column set and one key per day.
- **Signed in with Solid** (via [xlogin](https://unpkg.com/xlogin)): data is read from and written to your pod under

  ```
  <pod origin>/public/planner/
  ```

  `columns.jsonld` holds the column set, and each day is stored in its own `YYYY-MM-DD.jsonld` file. A day is an iCal `Vcalendar` of `Vevent`s in JSON-LD.
- **Private, even though the path is under `/public/`:** before its first write, the app checks for `/public/planner/.acl`. If the server returns 404 or 410, the app creates that file. It is an owner-only ACL (Read, Write, Control for `/profile/card.jsonld#me`), and `acl:default` makes it apply to every file in the folder. If an ACL already exists, the app leaves it alone.
- **On first sign-in:** today's blocks in local storage are copied to the pod, but only if the pod has no file for that day yet. Your custom columns are copied only if the pod has no column file yet. Nothing is copied back from the pod to local storage.

## Run locally

It is a single static page. Serve the folder with any static server, for example:

```
python3 -m http.server
```

Then open http://localhost:8000/.

## Licence

AGPL-3.0-only (as declared in `package.json`).
