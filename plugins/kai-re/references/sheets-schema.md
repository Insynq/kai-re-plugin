# The Kai-RE Organizer — Structure Reference

This is the authoritative description of where every fact lives. Read this before you create the organizer, before you write any row, and whenever you need to answer "where does this go?" or "how do I look this up?"

The organizer is **one Google Spreadsheet named `Kai-RE Organizer`**, living inside a Google Drive folder named **`Kai-RE`**. **People live in a `Contacts` tab inside this spreadsheet** — the sheet is self-contained and authoritative. (Google Contacts is only an *optional future mirror*, never the source of truth; see §3.)

You create all of this during onboarding (see `skills/onboard.md`). If any piece is missing when you go to use it, create it — don't fail silently.

### Creating & writing the spreadsheet (implementation note)

The organizer must end up as a **real Google Sheet in the user's Drive**, never a local file left on the machine. Use the host's Google Sheets/Drive tools to write and read it (create/import the spreadsheet, then read a range and batch-update ranges).

- **Prefer the host's spreadsheet tools** for creating and populating the sheet (e.g. importing a spreadsheet into Drive, then batch-updating ranges). Read it back after writing (safety-spine rule 3).
- **Don't assume a plain `python3` has spreadsheet libraries.** The *system* Python usually has **no `openpyxl`**, so a bare `import openpyxl` fails with `ModuleNotFoundError`. If you must build the workbook in code before importing it, first load the workspace/runtime that actually provides those libraries — don't burn a turn on the system-Python dead-end.
- If a piece already exists (folder, spreadsheet, a tab), **adopt it** rather than creating a duplicate.

---

## 1. The Drive layout

```
Kai-RE/                              (top-level folder — the whole organizer lives here)
├── Kai-RE Organizer                 (the spreadsheet: 6 tabs, below)
├── 123 Main St/                     (one subfolder per deal, named by street address)
│   ├── 123 Main St - Purchase Contract - 03-14-2025.pdf
│   ├── 123 Main St - Inspection Objection - 03-22-2025.pdf
│   └── ...
├── 456 Oak Ave/
│   └── ...
```

**Deal subfolder naming:** the street address exactly as it appears in `Deals.address` (e.g. `123 Main St`). One subfolder per deal. Create it the first time you file a document for that deal, and store its real Drive id in `Deals.drive_folder_id` so you never have to re-discover it.

**Document file naming (verbatim convention — do not improvise):**
```
{address} - {Doc Type} - MM-DD-YYYY.pdf
```
Example: `123 Main St - Counterproposal - 03-18-2025.pdf`. The date is the document's own date (execution or signing date printed on the doc), not today's date.

**Set-once, never-overwrite:** one file per (deal, document type). Once a file exists with that name, do not overwrite it. A newer version of the same doc type is a *new* Documents row with its own dated filename (e.g. an amendment); the old file stays. If two candidate files are ambiguous, **skip and ask** rather than guess or overwrite.

**Capture the real file ID:** after filing a document to Drive, read back its metadata and store the actual returned `drive_file_id` and `drive_link` in the Documents tab. Never record a fabricated or assumed ID.

---

## 2. The six tabs

Every tab's header row must use these exact column names in this exact order. When you create the spreadsheet, write these headers first.

**Write discipline (applies to every wide row):** timestamps (`created_at`/`updated_at`) sit near the FRONT of each row, not the end, so partial writes can't leave them misaligned. Write each Deals row as **one full-width batch across all columns** (all cells, blanks included — currently columns A–AS, 45 columns; keep the range matched to the header width), never as a set of partial patches. Then verify with a **single consolidated read of the whole row**, not many per-cell re-reads.

### Tab 1 — `Deals`
One row per transaction. Wide by design — most columns stay blank until a contract fills them in. **45 columns, A–AS.**

| # | Column | Meaning |
|---|---|---|
| A | `deal_id` | Unique id you assign (e.g. `D001`). Never reused. |
| B | `created_at` | Row creation timestamp. (Front of row by design.) |
| C | `updated_at` | Last-update timestamp. (Front of row by design.) |
| D | `address` | Street address. Also the deal's Drive subfolder name. |
| E | `city` | City. |
| F | `state` | Two-letter state. Drives the deadline vocabulary — see `references/deadline-taxonomy.md`. |
| G | `zip` | ZIP code. |
| H | `property_type` | Free text (single family, condo, townhome, land…). |
| I | `owner_name` | Property owner of record. |
| J | `client_name` | The person you represent on this deal. (Denormalized hot field.) |
| K | `client_contact_id` | FK → `Contacts.contact_id` for the client. (Replaces the old `client_contact_link`.) |
| L | `side` | **Enum:** `buyer` \| `seller` \| `dual`. |
| M | `status` | **Enum:** `lead` \| `pre_listing` \| `active` \| `under_contract` \| `closed` \| `terminated` \| `withdrawn` \| `on_hold`. |
| N | `template_key` | **Enum:** `listing_launch` \| `listing_uc` \| `buyer_fast_track` \| `buyer_uc`. Which task template seeded this deal. |
| O | `list_price` | Listing price. |
| P | `sale_price` | Contract/sale price. |
| Q | `list_date` | Date listed. |
| R | `mec_date` | Mutual Execution / acceptance date. **This is the TFC anchor** — anchors most deadline math (compute TFC+N, e.g. TFC+3, from here). |
| S | `close_date` | Scheduled closing date. |
| T | `mls_number` | MLS listing number. |
| U | `source` | Where the deal came from (referral, past client, sign call…). |
| V | `earnest_money_amount` | Earnest money dollar amount. |
| W | `earnest_money_due` | Date earnest money is due. |
| X | `financing_type` | Cash / conventional / FHA / VA… Drives whether loan+appraisal deadlines apply. |
| Y | `possession_date` | Possession date. |
| Z | `possession_time` | Possession time. |
| AA | `seller_concession` | Concession amount/terms. |
| AB | `hoa_name` | HOA name (blank if none — drives whether association deadlines apply). |
| AC | `hoa_monthly` | HOA monthly dues. |
| AD | `coop_agent_name` | The agent on the other side. (Denormalized hot field.) |
| AE | `coop_agent_contact_id` | FK → `Contacts.contact_id` for the co-op agent. |
| AF | `lender_name` | Lender / loan officer. (Denormalized hot field.) |
| AG | `lender_contact_id` | FK → `Contacts.contact_id` for the lender. |
| AH | `title_company` | Title company. (Denormalized hot field.) |
| AI | `title_contact_id` | FK → `Contacts.contact_id` for the title company/contact. |
| AJ | `title_closer` | Title closer contact. (Denormalized hot field.) |
| AK | `title_closer_contact_id` | FK → `Contacts.contact_id` for the title closer. |
| AL | `inspector` | Inspector. (Denormalized hot field.) |
| AM | `inspector_contact_id` | FK → `Contacts.contact_id` for the inspector. |
| AN | `drive_folder_id` | Real Drive id of this deal's document subfolder (captured once, so it isn't re-discovered each session). |
| AO | `linked_deal_id` | For a linked/contingent deal, the `deal_id` it depends on. |
| AP | `notes` | Append-only, each note timestamped. Never overwrite existing notes. |
| AQ | `validation_status` | **Enum:** `confirmed` \| `needs_validation`. See §4. |
| AR | `validation_source` | Where the confirmation came from (contract PDF, user, email). |
| AS | `validated_at` | When it was confirmed. |

**Normalize for integrity, denormalize for read-speed.** Each party appears twice: the **inline name** (a "hot field," so the everyday "who's on this deal" read stays a single tab read) and the **`_contact_id` FK** (the join key into `Contacts` for full details). Store plain ids; **never use live VLOOKUP/XLOOKUP formulas** — they're fragile, and reading a computed cell doesn't save a read anyway. Kai only hops to the `Contacts` tab when it actually needs a person's *details* (phone/email/company).

### Tab 2 — `Contacts`
One row per person. **This is the single source of truth for people and the one home for contact details.** Deduped — one row per real person, reused across every deal.

| Column | Meaning |
|---|---|
| `contact_id` | Unique id you assign (e.g. `C001`). The FK target for the `_contact_id` columns on `Deals`. |
| `created_at` | Row creation timestamp. |
| `updated_at` | Last-update timestamp. |
| `name` | The person's full name. Prefer the name they go by where known. |
| `role_labels` | Comma-separated labels from: `Client`, `Co-op Agent`, `Lender`, `Title`, `Inspector`, `Vendor`, `Counterparty`. **Additive** — when a person takes a new role, append the label, never strip an existing one. |
| `phone` | Phone. |
| `email` | Email. |
| `company` | Brokerage / lender / title company / firm. |
| `notes` | Free notes. |
| `source_message_id` | If the contact was discovered from an email, that email's id (provenance + dedup). |
| `validation_status` | **Enum:** `confirmed` \| `needs_validation`. |

**The `Counterparty` label** marks the other side's client. Comms and marketing must never reach a `Counterparty` — that label is the exclusion signal (enforced in `skills/draft-email.md`).

### Tab 3 — `Deadlines`
One row per deadline. Each row also mirrors to a Calendar event.

| Column | Meaning |
|---|---|
| `deadline_id` | Unique id (e.g. `DL0001`). |
| `deal_id` | The deal this belongs to. |
| `address` | Denormalized for easy reading. |
| `deadline_type` | Plain-English deadline name (e.g. "Inspection Objection"). See `references/deadline-taxonomy.md`. |
| `category` | Category bucket (Title, Association, Disclosure, Loan/Credit, Appraisal, Survey, Inspection, Closing/Possession). |
| `due_date` | Date due. |
| `due_time` | Time due (default 11:59 PM if the contract gives none). |
| `status` | **Enum:** `pending` \| `complete` \| `missed` \| `waived`. |
| `source_document` | Which document set this deadline (e.g. the Purchase Contract). |
| `source_doc_link` | Drive link to the source PDF (already captured when the doc was filed), so "why does this deadline say this?" resolves to a clickable document, not just a filename. |
| `calendar_event_id` | The Calendar event id, written back after you create the event. Makes updates/cancels repeatable. |
| `source_message_id` | If the deadline came from an email, that email's id. See §5. |
| `notes` | Free notes. |
| `validation_status` | **Enum:** `confirmed` \| `needs_validation`. |

### Tab 4 — `Documents`
One row per filed document. The file bytes live in Drive; this row points to them.

| Column | Meaning |
|---|---|
| `doc_id` | Unique id (e.g. `DOC0001`). |
| `deal_id` | The deal. |
| `address` | Denormalized. |
| `doc_family` | Family bucket (contract, listing agreement, amendment, objection/resolution, disclosure, termination, closing docs). See `references/doc-families.md`. |
| `document_type` | Specific type (Purchase Contract, Counterproposal, Inspection Objection…). |
| `document_name` | The filename as stored in Drive. |
| `signature_status` | **Enum:** `draft` \| `partially_signed` \| `executed` \| `voided`. |
| `drive_file_id` | The real Drive file id, read back from metadata after filing. |
| `drive_link` | The real Drive link. |
| `sent_at` | When the document was sent out. |
| `executed_at` | When fully executed. |
| `amends_doc_id` | If this doc amends another, that doc's `doc_id`. |
| `source_message_id` | If discovered via email, that email's id. |
| `notes` | Free notes. |
| `validation_status` | **Enum:** `confirmed` \| `needs_validation`. |

### Tab 5 — `Tasks`
One row per task, seeded from the four task-template files in `references/task-templates/`.

| Column | Meaning |
|---|---|
| `task_id` | Unique id. |
| `deal_id` | The deal. |
| `address` | Denormalized. |
| `template_key` | Which template this task came from (matches `Deals.template_key`). |
| `task_key` | Stable key from the template file — the dedup key when re-seeding. |
| `title` | Task title. |
| `milestone` | Which milestone group the task belongs to. |
| `status` | **Enum:** `pending` \| `complete` \| `skipped`. |
| `assigned_to` | **Enum:** `agent` \| `kai`. Who owns the task. **Kai never acts on a `kai`-assigned task autonomously — only with the user's explicit permission (Kai may ask).** Transaction/template tasks default to `agent`. |
| `due_date` | Due date, if any. |
| `show_on_calendar` | **Enum:** `yes` \| `no`. Default `no` for template-seeded transaction tasks (a deal can have ~50 — a calendar wall). When `yes`, create the event on the agent-only task calendar. |
| `calendar_event_id` | Set when `show_on_calendar=yes` and the event exists. |
| `completed_at` | When completed. |
| `source` | How the task got here: `template seed` \| `manual add` \| `user upload`. |
| `notes` | Free notes. |

### Tab 6 — `Meta`
A key-value tab. Rows use columns `key`, `value`, and `notes`. Four kinds of rows:

1. **User profile** — one row each: `name`, `company`, `brokerage`, `office_address`, `state`, `voice_notes`.
2. **Email lane watermarks** — one row per lane, using columns `lane`, `last_processed_date`, `last_message_id`. The four lanes: `contracts`, `parties`, `deadline_evidence`, `replies`. A watermark records how far a lane has been read. See §6.
3. **Config / persisted IDs** — so Kai stops re-discovering scaffolding every session. Standard keys (add more as needed):
   - `kai_re_folder_id` — Drive id of the `Kai-RE` root folder.
   - `provider_stack` — the user's stack **by capability, not vendor**: `email`, `calendar`, `files`, and `database` (where the organizer lives) each mapped to whoever provides it, e.g. `email=Google; calendar=Google; files=Google Drive; database=Google Sheets`. A **mixed** stack is normal and first-class (e.g. `files=Dropbox`). **Skills route off the capability, never a hard-coded vendor** — this is the field they read to know who to talk to. Set during onboarding's stack-detection step.
   - `read_consent` — the standing okay to *look*, per connection, with the date it was given, e.g. `email=yes (2026-07-07); calendar=yes (2026-07-07); drive=held`. A **one-time** consent earned at onboarding — Kai does **not** re-ask every session. A held-back connection is remembered here too, so Kai skips its dependent work until the user opts in.
   - `allowed_drive_folders` — the folders Kai may look inside (default: the `Kai-RE` folder only). Anything outside this list is off-limits under the safety spine. The Drive connection can technically search the whole Drive, so this is the recorded **promise** Kai keeps — not a wall (see the boundary note in `skills/onboard.md`).
   - `deadlines_calendar_id` — the transaction **deadlines** calendar id.
   - `task_calendar_id` — the agent-only **task** calendar id (only if the user opts to surface tasks on a calendar).
   - `watched_calendars` — which of the user's calendars Kai **reads/tracks** for deals (chosen at onboarding), and which to leave alone (e.g. a personal/family one). This is the read-side setting — distinct from `deadlines_calendar_id`/`task_calendar_id`, which are where Kai *writes*.
   - `pref_invite_client_default` — `yes`/`no` (default `no`): invite the client to deadline events.
   - `pref_share_closed_folder` — `yes`/`no`.
   - `pref_gmail_cadence` — free text (session-only reality explained at onboarding).
   - `pref_auto_accept_events` — `yes`/`no` (default `no`: keep events only on the transaction calendar; auto-accept double-displays onto primary).
   - `pref_tasks_on_calendar` — which task categories (if any) surface on the task calendar (default: none).
   - `automation` — whether scheduled/unattended runs are on; the locked-vs-unlocked choice; and which outward actions (if any) are pre-approved to run unattended. **Default: off / none** — an unattended run may **read, organize, and draft**, but everything outward waits in a **review queue** for the user (the host asks the user to approve every outward action, and Kai's own safety gate keeps client-facing sends queued regardless of this setting). See the closing step in `skills/onboard.md`.
   - `voice_learning` — status/notes for the email-voice calibration offer.
4. **Last-run note** — a free row recording what the last session did.

Per-deal ids (each deal's document subfolder) live on the `Deals` row (`drive_folder_id`), **not** in Meta.

---

## 3. People — the `Contacts` tab (Sheet is authoritative)

Every person is one row in the `Contacts` tab (Tab 2). The spreadsheet is self-contained and authoritative — do **not** depend on Google Contacts.

- **Role labels** live in `role_labels` (comma-separated): `Client`, `Co-op Agent`, `Lender`, `Title`, `Inspector`, `Vendor`, `Counterparty`.
- **Labels are additive.** When a person takes on a second role, append the new label — never remove the existing one. The same title contact can be `Title` on one deal and stay `Title` across all of them.
- **The `Counterparty` label** marks the other side's client. Comms and marketing must never reach a `Counterparty` — that label is the exclusion signal.
- **Deal↔person linkage** is the paired columns on the `Deals` row: the inline name (`client_name`, `coop_agent_name`, `lender_name`, `title_company`, `title_closer`, `inspector`) plus its `_contact_id` FK into `Contacts`. The `Contacts` row holds the person's details; the `Deals` row records their role on that specific deal.
- **Before adding a contact,** read the `Contacts` tab first (by name and email). If a similar person exists, add the new role label to the existing row and reuse its `contact_id` — surface a possible match and confirm rather than creating a duplicate or overwriting existing details. Surface any conflict (a different phone/email for a known person) rather than silently overwriting.

**Optional future mirror:** if Google Contacts is available and the user wants it, Kai may *mirror* these rows out to Google Contacts as a convenience — but the `Contacts` tab remains the source of truth. The mirror is not built yet; nothing should depend on it.

---

## 4. The `validation_status` discipline

Every tab that captures deal facts carries `validation_status` (`Deals`, `Contacts`, `Deadlines`, `Documents`) with two values:

- `confirmed` — a person confirmed it, or it came straight off a signed source document.
- `needs_validation` — you inferred, extracted, or guessed it and no one has confirmed it yet.

**Rule:** when you capture something internally without explicit confirmation, write the row **and** set `validation_status='needs_validation'`, then surface it on a short confirm list. Inferred-but-unconfirmed data goes to the needs-review list — it must never appear in a user-facing summary presented as established fact.

When capturing, also fill `validation_source` (where it came from) and, on confirmation, `validated_at`.

---

## 5. The `source_message_id` discipline

Every row derived from an email records that email's message id in `source_message_id` (on `Deadlines`, `Documents`, and `Contacts`). This is two things at once:

1. **The dedup key** — before creating a row from an email, check whether a row with that `source_message_id` already exists.
2. **The provenance trail** — the "where did this come from?" path. When the user asks why a deadline says what it says, you re-open that exact email (and, for a deadline, follow `source_doc_link` to the PDF).

Never write an email-derived deadline, document, or contact without its `source_message_id`.

---

## 6. The Meta-tab watermarks

Each of the four email lanes (`contracts`, `parties`, `deadline_evidence`, `replies`) has one watermark row in `Meta`: `lane`, `last_processed_date`, `last_message_id`.

- A watermark means "this lane has been read up to here."
- **Advance a watermark ONLY after the writes for that lane succeed.** A lane that failed or never ran keeps its old watermark — never stamp a date for a lane you didn't actually process.
- **Initial watermarks:** on first run there are none. Onboarding's read-only baseline pass reads recent email and then stamps all four watermarks to "now" — marking "seen up to here." Because the baseline writes no rows, the initial watermark is simply the current moment, not a backdated value. Going deeper into history (the 2–4-week lookback and party discovery) is a separate, later, confirm-gated mining pass — it does not set the initial watermark.

---

## 7. Canned query patterns

These are the standard read patterns over the tabs above. Each is a fresh read — always read the source fresh, never answer from memory.

**Open-deals filter (denylist, never allowlist):**
> Read `Deals`; keep every row whose `status` is NOT one of `closed`, `terminated`, `withdrawn`. Do this by excluding the closed states — never by listing the open ones, so a new status can't silently drop out of view.

**Needs-validation worklist (per tab):**
> Read a tab; return rows where `validation_status='needs_validation'`. Run across `Deals`, `Contacts`, `Deadlines`, `Documents` to build the "needs your review" list. Save this as a Filter View per tab for quick reuse.

**Deadline ladder buckets (T-3 / T-1 / T-0 / T+1):**
> Read `Deadlines` filtered to open deals with `status='pending'`. Bucket each by days-from-today to `due_date`:
> - **T-3** — due in ~3 days (early warning; collapse these to one line each).
> - **T-1** — due tomorrow (surface clearly).
> - **T-0** — due today (render in full detail).
> - **T+1** — one day past due (overdue; flag hard).
> Compute buckets at read time — there is no stored bucket column. This one pattern serves both `/catch-up` and `/briefing`.

**Who's-on-this-deal (all in-sheet — no Google Contacts):**
> Read the `Deals` row for the deal. For the everyday view, the inline name columns (`client_name`, `coop_agent_name`, `lender_name`, `title_company`, `title_closer`, `inspector`) answer it in a single read. When you need a person's **details** (phone/email/company), take the matching `_contact_id` and read that row from the `Contacts` tab. The `Contacts` tab is the source of truth for the person; the `Deals` row tells you their role here.

**Deal document chain (before writing any price/date):**
> Read all `Documents` rows for a deal, ordered by date, following `amends_doc_id`. Newest wins per field; a counterproposal replaces the base contract per field; amendments stack. Read the whole chain before writing any price or deadline — never off a single document.
