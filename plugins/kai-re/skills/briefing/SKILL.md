---
name: briefing
description: The morning briefing — a very short, decision-first rundown of the top 3 things that actually need the user today, pulled fresh from email, the calendar, and the Kai-RE Organizer. No headers, no counts, no telemetry. Read-only unless the user acts on an item. Use when the user says "morning briefing", "what do I need to know today?", "give me the rundown", "anything today?", or starts their day.
---

# Morning Briefing

Give the user the shortest useful version of their day: the **top 3 actionable items** and nothing else. This is the message that gets ignored if it's long or padded, so collect widely but say little.

Read the safety spine in `AGENTS.md` if you haven't this session. The briefing itself is a read: it doesn't send, invite, share, or write anything. If the user then says "email that client back" or "book that", that's a separate step and it goes through the confirm-before-send gate — show the exact draft, get an explicit yes.

## What to collect

Pull fresh from all three surfaces — never from cached notes:
- **Gmail** — anything new that needs the user (a client waiting on a reply, a contract just fully executed, a counter that changed terms).
- **The calendar** — deadlines landing today or tomorrow; a conflict or an unusually heavy day.
- **The Kai-RE Organizer** — the **Deadlines** tab (open deals only; exclude closed / terminated / withdrawn) for what's due, and the **Deals** tab for anything mid-flight.

Most of what you collect does **not** belong in the briefing. It stays in the spreadsheet, which the user can ask you to open any time. Only the items that need a decision today make the cut.

## How to pick the top 3

Rank by this priority and take the top three:
1. **Contract-deadline urgency** — a deadline due today or overdue, especially anything that protects the client's right to act (inspection, objection, financing, appraisal, closing). Lead with the plain-English name ("Inspection Objection due today on 123 Main"), not a form code.
2. **A client needs a response** — a thread waiting on the user where a reply is clearly owed.
3. **Everything else** worth acting on today.

If only one or two items clear the bar, send one or two — do not pad to three. If nothing clears the bar, send exactly:

```
All clear.
```

## Format and hard rules

- **Under 600 characters.** This is the number-one thing. A long briefing gets ignored.
- **No headers.** No "Deadlines", "Email", "Calendar" sections. Just the items.
- **No telemetry.** Don't narrate what you searched or processed. The user only sees what changed something they need to act on.
- **No raw counts without a decision.** Not "7 open deadlines" — instead "Inspection objection due today on 123 Main — call the buyer." Every line is an action with the names and amounts baked in.
- **Each item leads with the action**, then the specifics (address, person, amount, time).
- One optional calendar line **only** if there's a real conflict or a heavy day.
- Only mention a broken Google connection if it actually blocks one of your top-3 items — otherwise stay silent about plumbing.
- Names are fine; keep phone numbers and email addresses out of the briefing.

## Shape

```
Morning — [Day M/D]

1. [Action] — [address / person, amount or time]
2. [Action] — [specifics]
3. [Action] — [specifics]
```

(Drop lines 2 and 3 if fewer than three items qualify. If zero qualify, just: `All clear.`)

## Remember

- Read the source fresh every time; never answer a deadline or status from memory.
- The briefing is read-only. Any action the user then chooses runs through the normal confirm gate.
- Plain English over form codes, always.
