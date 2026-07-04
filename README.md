# arc-agi-3-submission (public)

Public, MIT-0 mirror of Jerry's **ARC Prize 2026 — ARC-AGI-3** submission: an agent that plays
unseen interactive games efficiently.

- Competition: <https://www.kaggle.com/competitions/arc-prize-2026-arc-agi-3>
- License: **MIT No Attribution** (see [LICENSE](LICENSE)) — CC0/MIT-0 is required for any prize.

## Why this repo exists

Two competition rules make a public, open-source repo mandatory:

1. **Prize eligibility** — a prize-eligible solution must be released under CC0 or MIT-0 **before
   private scoring**. A private repo can't win.
2. **Paper Track** — the paper must link to a **working, reproducible, public** code submission.

So the *solution* is open-sourced here, while the day-to-day working repo (strategy, experiment
logs, session memory) stays private. There is no durable secrecy in this competition anyway —
everyone open-sources to win — so nothing is lost by publishing the finalised code, and both rules
above are satisfied.

## What belongs here (and what doesn't)

**In:** only the finalised submission — the agent/package code, the scripts needed to reproduce a
run, tests, the paper, this README, and `LICENSE`.

**Out:** strategy notes, TODO/PLAN, session memory logs, and raw experiment history. Those are the
*how we got here*, not the *solution*, and live only in the private working repo.

## How this repo is maintained (internal note)

This is a curated mirror of a private working repo. The workflow:

- **Files are authored/updated from the private repo.** This `public-repo/` folder physically sits
  inside the private repo's working tree (git-ignored there, with its own `.git`), so the same AI
  assistants that build the solution write straight into it. It therefore needs **no separate AI
  instructions of its own** — it's governed by the private repo's `AGENTS.md`.
- **Jerry does all git here.** To commit/push, Jerry `cd`s into this folder and runs git directly
  against this repo's own remote. Same contract as the private repo: **agents write, Jerry runs git.**
- Sync cadence: populate/refresh when publishing a version — Milestone 2 (Sept 30) and the final
  submission (Nov 2).
