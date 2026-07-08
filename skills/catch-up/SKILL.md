---
name: catch-up
description: Session sweep — bring the Kai-RE Organizer up to date with what happened while the user was away, then hand them a short decision-first digest. Reads email, the spreadsheet, and the calendar; writes internal captures freely (flagged for review) and never sends or invites without an explicit yes.
when-to-use: The user says "catch me up", "what did I miss?", "any new contracts?", "run the sweep", or opens a session after time away.
---

# Catch-Up — the session sweep

Run this when the user wants to know what changed since last time. You have no always-on inbox — you see new email only when a session runs — so this sweep IS how the user finds out what happened. Be thorough in the reads, ruthless in the digest: collect a lot, surface only what needs a decision.

Read the safety spine in `AGENTS.md` first if you haven't this session. The two rules that govern every step here: **internal captures** (writing rows to the spreadsheet, filing a PDF into the Kai-RE folder, adding a row to the `Contacts` tab) you may do on your own — but tag each with `validation_status = needs_validation` and list it back for the user to confirm. **Outward or irreversible actions** (sending an email, creating a calendar invite that emails attendees, sharing a file, deleting, overwriting) you NEVER do without showing the exact draft and getting an explicit yes, every time.

## The sequence

Do these in order. Each lane's watermark advances only after that lane's findings are written or confirmed — a lane that fails keeps its old watermark so nothing gets silently skipped.

### 0. Read the watermarks

Open the **Meta** tab of the "Kai-RE Organizer" spreadsheet. There is one watermark row per email lane: `lane`, `last_processed_date`, `last_message_id`. The four lanes are **contracts**, **parties**, **deadline_evidence**, **replies**.

- A lane that has a watermark → search that lane's email since `last_processed_date`.
- A lane with **no** watermark yet → this is effectively a first run for it. Use a lookback floor of **the last 2–4 weeks** (start at 3 weeks). The onboarding baseline covers anything older; you are not responsible for the full history.

Never invent a watermark date. If the Meta tab or the spreadsheet won't open, stop and say plainly that you can't reach the organizer and how to reconnect it — don't guess dates and don't proceed as if it were empty.

### 1. Search each email lane

Search Gmail once per lane, restricted to messages since that lane's watermark (or the lookback floor). You can read email bodies and subjects; you **cannot** download attachments — so when a lane turns up a contract, the plan is always to have the user download the PDF (step 2), never to try to pull it from the message.

**contracts lane** — new or updated contracts and signature activity. **Open a thread only when it clears the gate — this is the precision fix.** A message is worth opening only if it has EITHER:
- **(a) an address token** that matches a tracked deal's address — the street number + name from an `address` you already track in the **Deals** tab — OR
- **(b) a known sender or domain** — one of the e-signature / transaction-coordination platforms the user named at onboarding (e.g. eContracts / CTM eContracts, DocuSign, dotloop, the brokerage's contract system).

The bare word "contract" on its own is **not** a reason to open a thread — it sweeps in invoices, service agreements, and other non-real-estate mail (a past run opened ~21 threads chasing it, which is the slowdown this fixes). The generic signature-notification wording — "sent for signature", "signed by", "fully executed", "completed", "all parties have signed", "ready to sign" — is a **secondary** signal only: once a thread is open it tells you *what kind* of contract activity it is, but on its own it must still clear the address-or-known-sender gate above before you open the thread.

Each real contract signal that clears the gate → carry it to step 2 (route to the contract-reading skill). "Fully executed" is the important one: it usually means a deal just went under contract or a milestone was hit.

**parties lane** — people you don't have yet. Inside threads that belong to a deal you already track, look for **unknown senders** who are clearly the lender, title company/closer, co-op agent, or inspector on that deal (their signature block, their role in the message). For each: **propose adding them as a new row in the `Contacts` tab** of the Kai-RE Organizer — assign a fresh `contact_id`, set the right role label in `role_labels` (Lender, Title, Co-op Agent, Inspector, Vendor, Counterparty), and record where they came from in `source_message_id`. Then link them to the deal on the **Deals** row: fill the inline name (client_name / coop_agent_name / lender_name / title_company / title_closer / inspector) and its paired `_contact_id` so the deal points back at the new Contacts row. Adding a Contacts row is an internal capture — you may write it, flagged `needs_validation`, and list it for confirmation. **Labels are additive:** if the person already has a Contacts row, append the new label to their existing `role_labels` and reuse their `contact_id` — never strip a label they already have, and never create a duplicate row. Cross-check the `Contacts` tab first (by name and email) and surface any conflict (a different phone/email for a name you already have) rather than overwriting.

**deadline_evidence lane** — signals that a tracked deadline was satisfied. Read for plain-English evidence like: a **clean inspection** / inspection resolved, the **appraisal came in at (or above) price**, **clear to close** / clear-to-close issued, the **title commitment delivered**, a **disclosure received** (seller's property disclosure, source-of-water, etc.), **HOA documents delivered**, survey delivered. For each, find the matching row in the **Deadlines** tab and **propose updating** its status to `complete`.
- **NEVER auto-close protection:** deadlines whose type is an inspection **termination or objection**, **loan disapproval**, **appraisal objection**, or a **title objection** protect the client's right to walk away. You **surface** evidence about these — you never mark them complete or waived on your own, no matter how clear the email reads. List them under "Needs you," not as done.

**replies lane** — threads waiting on the user. Find deal-related threads where the last message is inbound and no reply has gone out. **Surface** these as "awaiting your reply"; the user decides and closes them. Do not draft or send anything here unless the user asks.

Rules that apply to every lane:
- **Same gate, every lane (it keeps the noise down everywhere, not just contracts):** only open and act on an email you can tie to a **specific known deal** — either a **known sender/domain** (a party already on the deal, or a platform the user named at onboarding) or the **property address** appears in it. A generic keyword on its own is never enough to open a thread. If two or more deals could match, ask which; don't guess.
- Every row you write from an email records the message's `source_message_id`. Before writing, check that id isn't already recorded (dedup) so a re-run or a re-sent notification doesn't create duplicates.
- Use dates printed **in the document**, never the email's timestamp, for any deadline or price.

### 2. Route new contracts to the contract-reading skill

For each genuine new or updated contract the contracts lane found, hand off to `skills/process-contract.md`. Because you can't pull the attachment yourself, **remind the user to download the PDF** — to their Downloads folder or straight into the deal's folder inside "Kai-RE" — and tell them which property it's for. Read the complete document chain (contract + any counterproposal + amendments, newest wins per field) before anything gets written to price or date fields.

### 3. Deadline ladder sweep

Independent of email, read the **Deadlines** tab for every open deal (exclude deals whose status is closed / terminated / withdrawn — filter by "not closed," don't try to list the open statuses) and cross-check the calendar look-ahead. Bucket each deadline by how far out it is and render like this:

- **Overdue** (past due, still pending) — **always list every one**, in full, at the top. But if a deal is clearly already closed and these are stale past dates, don't wall the user with fake-overdue items — flag "is this deal closed?" instead.
- **T-0 (due today)** — list in full detail: property, plain-English deadline name, time.
- **T-1 (due tomorrow)** — one line each.
- **T-3 (due in ~3 days)** — **collapse to a single line** with a count ("3 deadlines due within 3 days"); only enumerate if there's just one.

Lead with the plain-English name ("Inspection Objection", "Appraisal deadline"), form code secondary if at all. Never surface a NEVER-auto-close type as resolved.

### 4. Advance the watermarks — only for lanes that actually ran

For each lane whose findings you successfully wrote or confirmed, update its Meta row: set `last_processed_date` to the newest processed message's date and `last_message_id` accordingly. **A lane that failed or that you skipped keeps its old watermark** and gets reported by name in the digest with the exact error text you saw — never stamp a date on an unrun lane (that would silently skip the mail it never read). If a Google connection was down, say which one and how to reconnect it; never claim a lane ran when it didn't.

### 5. The digest

Present a short, decision-first summary. Every item leads with **the decision the user needs to make** plus the concrete names and amounts. Use these blocks, and drop any block that's empty:

```
Catch-up — [M/D]

New contracts:
- [Address] — [buyer/seller], under contract $[price] ([financing type]), [N] deadlines added → please confirm. Download the PDF and I'll finish reading it.

Deadlines needing you:
1. [Address] — [plain-English deadline] due [date] ([overdue / today / tomorrow])
2. ...

Awaiting replies:
- [Address / person] — waiting on your reply since [date]

Needs you:
1. [Specific decision — add [name] as the lender on [address]? / is [address] closed?]

Bottom line: [one line — what to do first, or "all clear"]
```

Rules for the digest:
- Only say **"all clear"** when **every lane actually ran and returned nothing** — and state the counts so the claim is checkable ("3 active listings, 2 under-contract buyers, 0 deadlines in the next 3 days, 0 threads awaiting reply"). If any lane failed, you are not all-clear: name that lane and its error instead.
- Don't re-dump items the user already resolved; show new-vs-still-open.
- Plain English first, form codes second.
- Nothing in this digest is a sent message or an invite — it's a list of things you've captured (flagged for review) and things that need the user's yes.

## Verify before you claim it's done

After any write in this sweep, read it back before reporting it: re-read the spreadsheet range you appended, re-fetch a calendar event by its id, echo the real Drive file id from the file's metadata. A "success" message from a tool is a claim, not proof — and the user can't spot a fabricated "done." If a read-back doesn't match, say so and fix it; don't paper over it.

## What this skill never does

- Never sends an email, creates an attendee invite, shares a file, or deletes/overwrites anything without showing the exact draft/event and getting an explicit yes.
- Never marks a NEVER-auto-close deadline (inspection termination/objection, loan disapproval, appraisal objection, title objection) complete or waived on its own — surface only.
- Never advances a lane's watermark unless that lane ran and its findings landed.
- Never caches record facts in notes or memory — re-read the spreadsheet (including its `Contacts` tab) and calendar fresh each run.
