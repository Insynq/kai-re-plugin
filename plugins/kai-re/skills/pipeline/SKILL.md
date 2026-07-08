---
name: pipeline
description: Read-and-report over the user's deals, tasks, and deadlines — the full pipeline, what's next on one deal, or marking a task done. Use when the user asks to see their deals, book of business, or where a deal stands.
---

# Skill: pipeline

**Invoke when** the user asks to see their book of business or drill into one deal: "show me my deals," "what's my pipeline," "how are my listings doing," "what's next on Birch Court," or "mark the inspection scheduled task done." This is the read-and-report surface over the Deals, Tasks, and Deadlines tabs plus Calendar.

Obey the safety spine in `AGENTS.md`. Everything in this skill except marking a task complete is **read-only** — always **query the source fresh** (re-read the tabs and Calendar every time; never answer from a cached figure in notes). Marking a task done is an internal capture: write it, then read it back before reporting. If any Google connection fails, name it plainly and say the report is incomplete — never invent numbers or claim "all clear" on a read that didn't run.

This skill has three modes. Detect which the user wants:
- **A. Pipeline report** — the whole book ("show me my deals").
- **B. What's next on one deal** — a single address ("what's next on 4820 Birch Court").
- **C. Mark a task done** — ("mark 'order inspection' done on Birch Court").

---

## A. Pipeline report — "show me my deals"

### A1. Read the open deals

Read the "Deals" tab. Filter to **open** deals using the **denylist**: exclude any deal whose `status` is one of `closed`, `terminated`, `withdrawn`. Keep everything else (`lead`, `pre_listing`, `active`, `under_contract`, `on_hold`). Never build the filter as an allowlist of open statuses — a status you forget to list would silently vanish from the report.

### A2. Join task progress and the next deadline for each deal

For every open deal:
- **Task progress** — read the "Tasks" tab filtered to this `deal_id`. Count `done = tasks with status = complete`, `total = tasks not status = skipped`. Render as `done/total`.
- **Next deadline** — read the "Deadlines" tab filtered to this `deal_id`, keep rows with `status = pending`, and pick the one with the soonest `due_date` — **past or future** — so an overdue (past-due) pending deadline, which sorts first, is never skipped in favor of a later future one. Cross-check the Calendar event (via `calendar_event_id`) so the date you show is the live one. Compute **days-to-due** = due_date − today; if it is negative the deadline is **overdue**, and A3 renders it as overdue in this same column — surface it, don't hide it.

### A3. Render counts by status, then the per-deal table

Lead with a one-line count by status, e.g.:

> **7 open deals** — 3 active listings, 2 under contract (buyers), 1 pre-listing, 1 on hold.

Then a table, one row per deal:

| Address | Side | Status | Client | Tasks | Next deadline |
|---|---|---|---|---|---|
| 4820 Birch Court | buyer | under_contract | Nathan Ruiz | 9/17 | Inspection Objection — in 2 days |
| 118 Elm St | seller | active | Dana Cole | 22/40 | — (none scheduled) |

Rules for the table:
- Plain-English deadline names first (e.g. "Inspection Objection"), not form codes.
- Show days-to-due next to the date: "in 2 days," "today," or "**3 days overdue**" flagged in bold.
- If a deal has no pending deadline, show "—".

### A4. Escalation ladder summary

After the table, summarize deadlines across ALL open deals bucketed by urgency. Read the Deadlines tab + Calendar once, bucket pending deadlines by days-to-due:

- **Overdue** — past due. Flag these first and loudly; never wait for a briefing to raise them.
- **T-0 (due today)** — render in full: deal, deadline name, time.
- **T-1 (due tomorrow)** — render in full.
- **T-3 (due within 3 days)** — collapse to one line per deal.

Mark any deadline that is on the **NEVER_AUTO_CLOSE** list — inspection termination/objection, loan disapproval, appraisal objection, title objections — with **"needs your decision."** You never mark these complete or waived on your own; you only surface them. (See `references/deadline-taxonomy.md` for the full list.)

Example:

> **Deadlines ladder**
> - ⚠️ Overdue: 118 Elm St — Seller Property Disclosure was due 2 days ago.
> - Today: 4820 Birch Court — Earnest money due by 5:00 PM.
> - Tomorrow: 4820 Birch Court — **Inspection Objection** — needs your decision.
> - Within 3 days: 92 Cedar Ln (appraisal deadline).

### A5. Honesty on failed reads

State the counts you actually computed from reads that ran. If the Deadlines read or a Calendar call failed, say so by name ("I couldn't reach your Calendar, so the deadline column may be incomplete — reconnect Calendar and ask again") rather than showing a clean report that hides the gap. Only say "you're all clear" when every read ran and returned nothing due.

---

## B. What's next on one deal — "what's next on <address>"

1. Resolve the address to a `deal_id` in the Deals tab (fuzzy match on address; if two deals share the address — e.g. a dual-rep pair — ask which side).
2. Read the "Tasks" tab for that deal, keep `status = pending`, and order them by the template sort order (the order they appear in the matching `references/task-templates/` file), earliest milestone first.
3. Show the **top 5** pending tasks: title, milestone, and due_date if set.
4. Offer to mark one complete: "Want me to check any of these off?" If the user names a task, go to mode C. On a fuzzy or partial name match, confirm the exact task before writing.

---

## C. Mark a task done — "mark <task> done"

1. **Find the task.** Read the "Tasks" tab for the deal, match the user's phrase against pending task `title`s. 
2. **Confirm the match.** If the match is exact and unique, you may proceed. On any fuzzy, partial, or multiple-candidate match, show the specific task and ask "Did you mean **'Order home inspection'** on 4820 Birch Court?" — wait for yes. Never guess which task when more than one could fit.
3. **Write it.** Set that Tasks row `status = complete` and `completed_at = today's date/time`. This is an internal capture — write it, then re-read the row to confirm it saved.
4. **Report the next task.** After confirming, tell the user what's next: read the remaining pending tasks for the deal and name the next one in sort order. "Done — 'Order home inspection' is checked off. Next up: 'Schedule inspection walkthrough.'"
5. **Check milestone completion → propose status advance.** Look at the milestone the just-completed task belongs to. If every task in that milestone is now `complete` (ignoring `skipped`), and that milestone maps to a later deal status, propose advancing the deal — but never flip `status` on your own:
   - Fully-executed / offer-accepted milestone complete → propose `under_contract` (and offer to seed the UC template tasks via `new-deal`'s seeding step / `process-contract`).
   - Go-live / listing-live milestone complete → propose `active`.
   - Closing milestone complete → propose `closed`.
   Ask: "That was the last task in the **Under Contract** milestone — want me to move 118 Elm St to **under_contract**?" On yes, update the Deals row `status` (internal capture, read it back) and refresh `updated_at`. Advancing to `under_contract` should also pull in the under-contract task checklist if it isn't seeded yet.

Never mark a NEVER_AUTO_CLOSE deadline complete through this flow — those live in the Deadlines tab, not Tasks, and are always the user's decision.
