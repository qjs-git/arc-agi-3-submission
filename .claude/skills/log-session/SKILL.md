---
name: log-session
description: Run at the END of a working block/session. Writes a memory log (memory/YYYY-MM-DD-slug.md) covering everything since the last memory log, then adds a review trail to the right TODO.md â€” listing every file Claude ADDED, and every file + lines/sections Claude EDITED this session â€” skipping anything already reviewed. Finishes by suggesting a ready-to-paste git commit message (never runs git). Use when the user is wrapping up, ends the session, or asks to log the session / write a session log / leave a review trail.
---

# Log session â€” memory log + review trail

Run this at the **end of a block of work**. It produces two things:
1. a **memory log** of everything since the last memory log, and
2. a **review trail** added to the right `TODO.md` so Jerry can check what Claude changed.

Follow the hub conventions: `memory/README.md` (log format + index), the root `AGENTS.md`
"refresh habit" and "depth guardrail". **Git is human-only â€” never run any git command.**

## Step 1 â€” Find the boundary (the last log)
- Read `memory/README.md` and open the most recent `memory/YYYY-MM-DD-*.md`. Note its date and its
  "Open items / next time" (some may have been resolved this session).
- Everything done **after** that log is in scope. The reliable record of what changed is **this
  session's transcript** â€” the files you (Claude) created and edited. Do **not** use git to diff
  (human-only). You may read files or check modification times to confirm, but the transcript is the
  source of truth.

## Step 2 â€” Gather what changed this session
Build two lists from your own actions this session:
- **Added** â€” files you created (full path).
- **Edited** â€” files you changed, each with the **section name(s)** + an approximate **line range**,
  and a one-line note on what changed. Prefer section/anchor names â€” line numbers drift.

Group by project/area. Note the key **decisions and the reasoning** behind them as you go.

Also gather **what did NOT touch a file** â€” discussion, things explored or explained, questions
answered, ideas raised, dead ends. Keep it to a **short summary**. This is secondary: it becomes a
quiet footnote at the bottom of the memory log (Step 3), not part of the Added/Edited lists. The
Added/Edited items stay the main point.

## Step 3 â€” Write the memory log
Create `memory/YYYY-MM-DD-short-slug.md` (today's date; short kebab slug for the session's theme).
Match the existing logs' shape and the README convention. **Write tight: minimum words for maximum
signal** â€” explain as simply as possible in clear, concise steps; scannable, not blow-by-blow. Don't
restate what a file already says or pad with filler. If a line doesn't help Jerry act or recall, cut it.
- **Goal of the session**
- **Key decisions (and why)**
- **What was created/changed** (the added/edited lists, condensed)
- **Open items / next time**
- **Footnote â€” also this session** (small, at the very bottom): the Step-2 non-file summary (what was
  discussed/explored but didn't change a file). Keep it brief; it's context, not the headline.

If a log already exists for today, create a new slug for this block (or merge if it's clearly the same
block â€” ask if unsure). Then add a one-line pointer to the **Index** in `memory/README.md`.

## Step 4 â€” Add the review trail to the right TODO
The hub has **many nested `TODO.md` files** â€” route each changed file to the correct one:
- **Project-scoped** changes â†’ that project's own `TODO.md` (or its `PLAN.md`/`REQUIREMENTS.md` if it
  uses those instead), under the project's **Human** section.
- **Hub-wide** changes (root files, `.claude/`, cross-project) â†’ the root `TODO.md` **Human** section.
- **If you're unsure which TODO a file belongs to, ASK the user** before writing.

Under the chosen TODO, add a dated subsection. **Every file is a full-path Markdown link** (see the
path rule below):
```
### Review â€” AI changes from session YYYY-MM-DD (slug)
- [ ] Added: [full/path/from/hub-root/new-file.md](full/path/from/hub-root/new-file.md) â€” <one-line what it is>
- [ ] Edited: [full/path/from/hub-root/file.md](full/path/from/hub-root/file.md) â€” review <Section name> (~lines Nâ€“M): <what changed>
- [ ] Git (human-only): <commit / git init reminder, if any new files/repos>
```
List **every** added file and every edited file (file + lines/sections). Group tidily â€” a few grouped
bullets are fine â€” but don't drop any file, and each file still carries its own full path.

**Always give the full path, as a clickable link â€” never a bare filename.** Every file named in a
review item (and in any forward-looking action item) must carry its **full repo-relative path from the
hub root**, written as a Markdown link so Jerry can click straight to it â€”
e.g. `[builds/input-looper/INITIAL_PLAN.md](builds/input-looper/INITIAL_PLAN.md)`, **not** just
`INITIAL_PLAN.md`. This holds **even when the item lives in that project's own `TODO.md`**: bare names
like `AGENTS.md`, `README.md`, or "this `TODO.md`" are ambiguous â€” the hub has dozens of files with
those exact names, and Jerry (often opening the TODO straight from his IDE) can't tell which folder
from the filename alone. If the action targets one spot, add the section/anchor too. The only item that
may omit a path is a pure offline decision touching no file â€” say so explicitly rather than leaving the
location blank.

This applies to forward-looking action items, not just the review trail: if a task can only be *done*
by touching a file, link that file; for a task whose result lands in a file (e.g. "confirm channel
name/branding" â†’ recorded in the publish package), link that destination file too.

## Step 5 â€” Skip what's already reviewed
Before adding an item, scan existing review trails (this TODO's prior `### Review â€”` sections and the
last memory log). **If a file/change is already listed â€” checked or unchecked â€” do not add it again.**
Only log changes that are new since the last memory log.

## Step 6 â€” Suggest a commit message for the repo you're in (do NOT run git)
A single session's changes can land in **more than one git repo**: the hub (which versions only its
context files + `profile/` + `memory/` + `.claude/`) and one or more **git-ignored subrepos** beneath
it. Each repo is committed separately, from inside itself â€” so first work out **which repo you're
currently in** (the working directory's repo).

- **The repo you're currently in** â†’ end the report with **one ready-to-paste commit message**, scoped
  to *that* repo's changed files. Jerry's already inside it, so he can commit now. Match the repo's
  style (concise imperative summary; optional body bullets). Do **not** append any `Co-Authored-By:` /
  Claude attribution line.
- **Every other repo that got changes** â€” the **hub when you're inside a subrepo**, a subrepo when
  you're in the hub, or a second subrepo â†’ **do not print its commit message in the report.** Instead
  add a `- [ ]` checkbox to *that* repo's own `TODO.md` (a "Pending commits (run inside this repo)"
  section near the top â€” create it if absent, or use wherever that repo already collects commit
  reminders, e.g. an inline "Git (human-only)" line in its review trail), linking the changed files
  (full paths) and including the ready-to-paste message. It surfaces the next time Jerry is working
  *inside that repo* â€” which is when he'll actually commit it. The report just points at it in one line.

Same for a brand-new git-ignored folder: add a `git init` + first-commit checkbox in that folder's own
`TODO.md`. Rule of thumb: **the current repo's commit goes in the report; every other repo's commit
goes in that repo's TODO.**

## Step 7 â€” Report back
Tell the user: the memory log path, which TODO(s) received review items, the **ready-to-paste commit
message for the repo you're in**, a one-line pointer to any **other repo's pending-commit checkbox**,
and anything you had to ask about routing. Remind them git is theirs to run.

## Constraints
- Never run git (add/commit/init/branch/etc.) â€” leave it to the user.
- Keep the memory log tight â€” minimum words for maximum signal; capture *why*, not every keystroke.
- Non-file discussion goes in the bottom footnote only; Added/Edited stay the main content.
- Don't duplicate already-reviewed items.
- Every file-touching TODO item links the file by its **full repo-relative path from the hub root**
  (Markdown link, + section/anchor if it targets one spot) â€” never a bare filename, even in a project's
  own `TODO.md`. No actionable item left without a clickable location.
- Respect each project's `AGENTS.md`; keep any edits surgical.
- Always end with a ready-to-paste commit message **for the repo you're currently in**; every other
  repo's commit goes as a checkbox in *that* repo's own `TODO.md` (pointed at from the report), not as a
  second message. Never run git yourself.
- For any change in a repo you're **not** currently in â€” a subrepo when you're in the hub, **or the hub
  when you're inside a subrepo** â€” the commit (or `git init`) goes in as a `- [ ]` checkbox in *that*
  repo's own `TODO.md` (file links + paste-ready message), so it surfaces when Jerry is next inside that
  repo. The report just points at it.
