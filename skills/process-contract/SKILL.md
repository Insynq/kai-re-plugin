---
name: process-contract
description: Read a downloaded contract, counter, amendment, or disclosure and update the deal — extract terms, deadlines, and parties, confirm once, then write to the organizer, calendar, and Drive. Use when the user has saved a contract file and wants it processed.
---

# Skill: Process a Contract

**Invoke when:** the user says they've saved a contract, counter, amendment, disclosure, or other transaction document and wants you to read it and update the deal — or you find a downloaded contract PDF and need to pull its terms, deadlines, parties, and status into the Kai-RE Organizer.

This is the document-intake pipeline: read a PDF the user downloaded, extract everything in one careful pass, show one confirmation table, and only then write to the Sheet, the Calendar, and Drive. It is the single most valuable thing you do — a missed deadline here can cost the user a client. Move carefully and verify everything.

**Read first:** `references/doc-families.md` (what kind of document this is and what to pull from it), `references/deadline-taxonomy.md` (which deadlines apply and how their dates are computed), `references/sheets-schema.md` (exact tabs and columns), and the safety spine in `AGENTS.md` (the two-tier autonomy rule governs every write below).

---

## The one hard rule about where the PDF comes from

**The user downloads the contract. You read it from a file. You never fetch it from email.**

Your Gmail connection can search and read the text of emails, but it **cannot download attachments** — that path does not exist. So the contract PDF always reaches you as a file the user has already saved, in one of two places:

1. Their **Downloads** folder (they downloaded it from email or from their e-signature platform), or
2. The **Kai-RE** Drive folder or a deal subfolder inside it.

If you can't find the file, ask the user in plain language: "I couldn't find the contract file. Could you download it to your Downloads folder or drop it in the Kai-RE folder, then tell me the file name?" Never tell them to forward an email, and never claim you'll "grab it from the message."

---

## Step 1 — Locate the PDF

- If the user named a file or gave a link, open that.
- Otherwise search their Drive Kai-RE folder and their recent files, and offer the most likely candidates by name and date: "I see two PDFs from today — 123 Main St offer, and a counterproposal. Which one?"
- Confirm you have the actual document before reading — echo the file name back so the user knows you found the right one.
- **Reading the PDF (internal):** you're running inside a coding-agent app (often ChatGPT/Codex) where plain Python has **no PDF reader** — don't burn a pass rediscovering that dead-end. Go straight to the bundled document tools to pull the PDF's text.
- If the Drive connection fails, say so plainly and name it ("I can't reach your Google Drive right now — let's reconnect it before I read anything"). Never guess at the contents.

## Step 2 — Identify the document family and type

Read the document and classify it using `references/doc-families.md`:
- Which **family** is it? (purchase contract, representation/listing agreement, amendment/counter, disclosure, objection/resolution, termination, closing docs)
- Which **specific type** within that family, using the user's own **form map** (learned at onboarding) when the document shows a form name or code.

If a packet contains several documents (common — a contract often arrives bundled with disclosures and a party roster), treat each as its own document and process them in dependency order: **contact roster first**, then the primary contract, then supplements/amendments. Do not stop after the first one.

## Step 3 — Detect execution state from the signatures, not the email

Look at the **signature blocks in the PDF itself** to decide the document's status — never trust the email subject line ("Signed!", "Fully Executed") to tell you the state.

- No signatures / only agent prep → `draft`
- One side signed, the other blank → `partially_signed`
- Every required party signed → `executed`
- Marked void/withdrawn → `voided`

Record this as the document's `signature_status`. For a purchase contract, "every required party signed" means buyer(s) and seller(s) — the milestone that can move a deal to under contract.

## Step 4 — Match the document to a deal

Match against the **Deals** tab, freshly read:

1. **By property address first** — the strongest signal.
2. **Then by client or signer name** if the address doesn't resolve it.

Then:
- **Exactly one match** → proceed with that deal.
- **Two or more plausible matches** → stop and ask the user which deal this belongs to. Never guess.
- **No match** → propose creating a new deal: "This looks like a new one — 456 Oak Ave, buyer side, your client Dana Lee. Want me to create the deal and then load this contract into it?" (Deal creation follows `skills/new-deal.md`.) Never silently drop the document.

## Step 5 — Read the COMPLETE document chain before writing anything

A single deal's terms live across several documents. Before you write one price or date, gather and read the **whole chain** for this deal:

- The **base contract**, plus
- **Every counterproposal** (a counter *replaces* the base per-field for the fields it changes — newest wins), plus
- **Every amendment** (amendments *stack* — later ones adjust specific terms and deadlines).

Apply **newest-wins per field**: for each term (price, earnest money, each deadline, close date), the value is whatever the latest document in the chain sets it to; fields a counter/amendment doesn't touch keep their prior value.

Then ask the user explicitly: **"Did a counterproposal or amendment change any deadline?"** — deadline shifts hide inside counters and are the easiest thing to miss.

If part of the chain lives in a document you haven't been given, say so and ask for it rather than writing from an incomplete picture.

## Step 6 — Extract everything atomically, in one pass

From the resolved chain, pull all of the following in a single careful reading. **Prefer the literal dates and figures printed on the signed document**; compute a date only when the contract states it as an offset (e.g., "10 days after MEC") — see `references/deadline-taxonomy.md` for the conventions.

**Terms → the Deals row** (per the document family's field list in `references/doc-families.md`): price(s), earnest money amount and due date, financing type, MEC date, close date, possession date/time, seller concession, HOA, MLS number, title company/closer, lender, co-op agent, inspector — whatever the document carries. **Capture the MEC date (`mec_date`) without fail — it is the anchor every deadline offset counts from (TFC+N, e.g. TFC+3, is measured from it), so a missing MEC date leaves the whole deadline ladder ungrounded.**

**Deadlines → one row each in the Deadlines tab**, using the taxonomy:
- Apply the **cash-vs-financed** applicability rule — a cash deal skips loan and appraisal deadlines; HOA deadlines only apply when there's an association. Do not create deadlines the deal type doesn't have, and don't drop ones it does.
- Give each a plain-English `deadline_type` and its `category`.
- Never mark any **NEVER_AUTO_CLOSE** deadline (inspection termination/objection, loan disapproval, appraisal objection, title objections) as anything but `pending` — these protect the client's right to walk away, and only the user resolves them.

**Parties → the Contacts tab + the Deals columns:**
- People live in the **Contacts tab** of the Kai-RE Organizer (the source of truth), **not** Google Contacts.
- If the packet includes an authoritative **contact roster / e-contacts sheet**, extract parties from it **first** (it's the primary source); treat scattered mentions in other documents as supplementary.
- One Contacts-tab row per person, deduped by name and email — read the Contacts tab first, and if a person is already there, reuse their existing row and `contact_id` instead of creating a twin.
- Role labels are additive (Client, Co-op Agent, Lender, Title, Inspector, Vendor, Counterparty) — when a person takes on a new role, append the label; never remove one they already have.
- Assign a `contact_id` to each new person, and surface any conflict (a different phone/email for a known person) rather than silently overwriting.
- Then link each party onto the Deals row twice — the inline name **and** its `_contact_id`, in the matching pair: client_name+client_contact_id, coop_agent_name+coop_agent_contact_id, lender_name+lender_contact_id, title_company+title_contact_id, title_closer+title_closer_contact_id, inspector+inspector_contact_id.

**Signature status** for each document → the Documents row.

Record `source_message_id` on every row you derived from an email signal, and the source document name on every deadline, so any date can be traced back later.

## Step 7 — Self-check BEFORE you show anything

Sanity-test your own extraction. If it fails, **re-read the PDF** — don't present a thin result:

- A **purchase contract with fewer than 5 deadlines** is almost certainly under-extracted → re-read.
- A purchase contract **missing price, earnest money, financing type, or close date** → re-read.
- **Every document referenced in the packet was actually processed** — diff what the packet mentions against what you extracted, and **list any leftovers** ("the contract references an HOA addendum I didn't see — do you have it?"). A "1 of 2 documents" gap must never pass silently.

## Step 8 — Present ONE confirmation table

Show the user a single, scannable summary of **everything you're about to write** — deal terms, every deadline with its date, each party and their role, the document and its status, and any calendar events you'll create. Lead with anything that needs their judgment (an odd date, a missing figure, an unresolved party).

This is a capture, so per the two-tier rule you may proceed to write the internal records once they confirm — but present it clearly first, because everything is about to become the deal's source of truth. If anything is inferred rather than read directly, flag it as needs-validation in the table.

## Step 9 — On "yes," write everything

Only after the user confirms:

1. **Deals row** — insert or update the matched deal's terms as **one full-width batch across every column (A–AS, all 45 cells, blanks included)** — never a series of partial patches, so the front-of-row timestamps can't drift out of line.
2. **Deadlines rows** — one per deadline, `status=pending`, with category, due date/time, source document, and `validation_status`. Also fill **`source_doc_link`** with the source PDF's real Drive link (the same link captured when you file the PDF in item 6 below and record it on the Documents row), so "why does this deadline say this?" resolves to a clickable document, not just a filename.
3. **Documents row** — family, type, name, `signature_status`, and (after filing) the real Drive file ID and link; set `source_message_id` if email-derived.
4. **Tasks rows** — if this document seeds or advances a milestone (see Step 11), add the template's tasks per the matching file in `references/task-templates/`, reading the Tasks tab first and only adding missing `task_key`s (no duplicates). On each seeded task set **`assigned_to=agent`**, **`show_on_calendar=no`** (a deal can carry ~50 tasks — keep them off the calendar by default), and **`source=template seed`**.
5. **Calendar events** — one per deadline, titled exactly **"<Deadline name> — <address>"**. Write the returned event ID back into that Deadlines row's `calendar_event_id` so future updates/cancels are idempotent.
   - **Never create a past-dated event.** If a deadline's date is already in the past, do not put it on the calendar — flag it instead: "The inspection deadline was 3 days ago. Is this deal already closed, or should I still track this?"
6. **File the PDF** into the deal's Drive subfolder (create the subfolder, named by address, if it doesn't exist). Use the canonical name **"<address> - <Doc Type> - MM-DD-YYYY.pdf"**, one file per (deal, document type), set once. Capture the **real returned file ID** into the Documents row — never a guessed or constructed link.

## Step 10 — Verify after write, then run the completeness check

A tool's "success" is a claim until you've read it back.

- **Read the whole Deals row back in one consolidated read** and confirm the values landed — a single read of the full-width row, not many per-cell or per-patch re-reads. Then spot-read a couple of Deadlines rows and the Documents row.
- **Re-fetch one Calendar event** you created and confirm its real date/time and ID.
- Echo the **real Drive file ID** from the filed PDF's metadata.

Then assert the deal's invariants (especially for a deal now under contract):
- It has **pending deadlines**.
- It has a **template_key**.
- **Lender and title** are present (for a financed UC deal).

If any invariant is missing, say exactly what's missing and offer to fix it — don't claim the deal is fully set up. If a write silently didn't take, **fail loudly**: name what didn't save and don't report success.

## Step 11 — Propose milestone / status advancement

If the document changes the deal's stage, **propose** the transition (don't flip status silently):

- A **fully-executed purchase contract** → propose advancing `status` to `under_contract`, and on confirmation seed the matching **under-contract task template** (`listing-under-contract.md` for seller side, `buyer-under-contract.md` for buyer side) into the Tasks tab (each seeded task carries `assigned_to=agent`, `show_on_calendar=no`, `source=template seed`, as in Step 9).
- A fully-executed **listing/representation agreement** → propose the listing-launch stage and template as appropriate.
- A **termination** document → propose the terminated/withdrawn status, and surface (don't auto-resolve) any open deadlines that should now be cancelled.

Status changes reach into the deal's whole shape, so treat them as decisions for the user, presented with the reason ("both parties signed — this is now under contract; want me to move it and add the under-contract checklist?").

---

## Dedup rules (apply on every intake)

- Before creating a Documents row, check the **`source_message_id`** and the **(deal, document_type)** pair already in the tab. If the same document was already processed, **update the existing row** (refresh dates, status) — do **not** create a second row and do not reset signatures.
- A **resend** of the same document is an update, not a new row.
- A genuinely **new amendment or counter** after a prior one **is** a new row — amendments stack.
- Before creating a Deadline or its Calendar event, check for an existing row for that (deal, deadline_type). If it exists with a `calendar_event_id`, **update that event** rather than creating a duplicate.
- Before adding a person, check the **Contacts tab** for an existing row by name/email and add the role label to that existing row (reusing its `contact_id`) rather than creating a twin.

## When something goes wrong

- Google connection down → name the exact connection and how to reconnect; never fabricate success or invent a technical-sounding excuse.
- Can't read the PDF (scanned/garbled) → say so and ask the user to re-download or send a clearer copy.
- Ambiguous match, missing chain document, or a figure you can't find → ask; never guess a price, date, or party into the record.
