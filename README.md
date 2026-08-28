# campaign-planner
# Road to Camp — Campaign Tracker

An interactive milestone tracker for the **FJC × Bald Fall 2026 campaign**, replacing the
`FJC_x_Bald_Campaign_Planner` spreadsheet. It answers three questions at a glance: what's on
track, what FJC owes us, and what breaks if something slips.

**One file. No server, no hosting, no accounts.** Double-click
`road-to-camp-tracker.html` and it opens in your browser.

---

## The files

| File | What it is |
| --- | --- |
| `road-to-camp-tracker.html` | The whole tracker — data, styling and behaviour in one file. This is the thing you open, keep, and send. |
| `data.json` | The planner content on its own. A backup, and the file to edit if you'd rather bulk-change dates in a text editor than click through the UI. |
| `README.md` | This. |

---

## How you keep it current

The planner data lives *inside* the HTML file. Editing works like a document, not a website:

1. Open the file and click **Edit planner**.
2. Change dates, statuses, add deliverables or checkpoints.
3. Click **Download updated tracker**.

You get `road-to-camp-tracker-YYYYMMDD.html` — the same tracker with your changes baked in.
That downloaded file is now the current version: replace your old copy with it, and send it on
to whoever needs it. It can save itself again the same way, indefinitely.

**Send it to the client** by emailing or Dropbox-ing the file. They open it and see everything,
read-only in practice — nothing they do reaches your copy. When dates move, send the new file.

> One thing to know: browsers block local storage for files opened straight from disk, so on a
> double-clicked file your edits won't survive a refresh. Download before you close the tab.
> (If you ever put the file on a shared drive or intranet that serves it over http, drafts
> persist normally.)

### Editing `data.json` instead

For bulk changes — shifting a whole phase by a week, say — edit `data.json` in a text editor,
then **Edit planner → Import data** and pick the file. **Export data** goes the other way and
gives you a fresh `data.json` to keep as a backup.

---

## What's in the tracker

**Dashboard.** A completion bar across all deliverables, four status tiles (Complete, On Track,
At Risk, Delayed) that double as one-click filters, and two focus cards — how many actions sit
with FJC and when the next is due, plus the next checkpoint and how far away it is.

**Milestones.** Every deliverable grouped under its dated checkpoint. Collapsed: deliverable,
workstream, version, due date, what FJC needs to do, status. Expanded: what we're sharing, what
we need from them, what gets locked, what it unlocks, the client note, and links into the
dependency chain.

**Filters.** Search across every field, plus chips for what's needed (Approval / Feedback /
Client Input / No action), workstream (Campaign, Messaging, Web, Assets & Toolkit,
Rollout & Launch), and a hide-complete toggle.

**FJC actions.** Everything still owed by the client, sorted by deadline — what they'll receive,
what they need to do, what it locks, how many days out. Past due is outlined in red.

**Dependencies.** The nine dependency links (name → identity, UX → UI, UI → development, and so
on), each listing the milestones attached to it, navigable in both directions.

**How the work moves.** The web and asset/toolkit workflow tables, planned revision rounds, and
plain-English definitions of every status and action type.

Checkpoint status is never typed in — it's calculated from the deliverables inside it, worst
status wins. Update a deliverable and the checkpoint follows. Light and dark themes follow the
reader's system setting, and it works on a phone.

---

## `data.json` reference

**`meta`** — client, campaign name, intro copy, the notes list, and the two glossaries
(`actionKey`, `statusKey`). `updated` is set for you on every download.

**`checkpoints[]`** — the dated review moments.

```json
{ "id": "c4", "date": "2026-09-03", "label": "Final Wireframes",
  "fjcAction": "Final approval on wireframes + web copy before UI begins.",
  "isNew": true }
```

`isNew: true` draws the small "Added" tag.

**`items[]`** — the deliverables.

```json
{ "id": "i11", "cp": "c4", "cat": "Web",
  "deliverable": "Web Wireframes + Copy — Final", "version": "Final",
  "sharing": "Final wireframes + web copy",
  "needs": "Final approval before UI progresses",
  "action": "APPROVAL", "deadline": "2026-09-04",
  "locks": "Page structure, functionality, hierarchy, ~99% of web copy",
  "unlocks": "Web UI V1 (9/10)",
  "notes": "Added review round to fully lock copy before UI begins.",
  "status": "ON TRACK", "deps": ["d6"] }
```

| Field | Accepts |
| --- | --- |
| `cp` | a `checkpoints[].id` |
| `cat` | `Campaign`, `Messaging`, `Web`, `Assets & Toolkit`, `Rollout & Launch` |
| `action` | `APPROVAL`, `FEEDBACK`, `CLIENT INPUT`, `NO ACTION` |
| `status` | `COMPLETE`, `ON TRACK`, `AT RISK`, `DELAYED` |
| `deadline` | `YYYY-MM-DD`, or `null` for none |
| `deps` | array of `dependencies[].id` — drives the cross-links |
| `critical` | optional `true`, flags the row as a critical lock |

All dates are `YYYY-MM-DD` and display as `9/3`.

**`dependencies[]`, `revisions[]`, `workflows[]`** — the *Dependencies* and *How the work moves*
tabs. Add to `dependencies[]` and the new link becomes selectable on every milestone.

Check your edits before importing:

```bash
node -e "JSON.parse(require('fs').readFileSync('data.json','utf8')); console.log('valid')"
```

---

## Changing the look

Open the HTML in a text editor. The `<style id="app-css">` block starts with the full token set:
the light palette on bare `:root`, then two blocks redefining the same tokens for dark. Change a
colour in both and it propagates everywhere.

```css
--accent:#F05417;                     /* active filters, progress bar, approvals, links */
--ok / --warn / --bad / --done        /* status colours, kept separate from the accent */
--paper / --surface / --ink / --line  /* grounds, text, hairlines */
```

Type is **Archivo** (headings), **IBM Plex Sans** (body), **IBM Plex Mono** (dates, labels,
numbers). They're pulled from Google Fonts, so opening the file offline falls back to system
faces — everything still works and reads fine, it just looks a little plainer.

---

## Notes

- Nothing leaves your machine. No tracking, no analytics, no accounts, no network calls except
  the font stylesheet. The file works fully offline.
- Anyone you send the file to has the whole planner, including internal notes. It's a document —
  treat it like one.
- Needs a current browser (Chrome, Safari, Firefox, Edge — 2022 or later; it uses `<dialog>`).
- Source: `FJC_x_Bald_Campaign_Planner_V3_updated.xlsx` — 24 deliverables across 13 checkpoints,
  8/20 through 10/19.
