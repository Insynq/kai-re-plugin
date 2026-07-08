---
name: new-deal
description: Start tracking a new transaction — create the deal record, its document folder, and seed the task checklist. Use when the user gets a new listing, buyer, or offer, or asks to set up a deal for a property or client.
---

# Skill: new-deal

**Invoke when** the user wants to start tracking a transaction: "add a new listing," "I just got a buyer," "set up 123 Maple," "I'm now under contract on the Elm St deal," or a voice-note/forwarded message that mentions a property and a person. This skill creates one Deals row, its Drive subfolder, and seeds the matching task checklist. It does NOT extract contract dates — once the deal exists and the user has a signed contract, hand off to `process-contract`.

Everything here obeys the safety spine in `AGENTS.md`. Creating a Deals row, making a Drive subfolder, and seeding Tasks are **internal captures** — do them freely, but stamp `validation_status = needs_validation` and read them back. There is no outward/irreversible action in this skill, so you never send or share anything here. If any Google connection fails, say plainly which one and stop — never pretend the deal was created.

---

## 1. Pin down the property and the client first

The user and the contract are the only authoritative sources for who owns a property and who the client is. **There is no county lookup and no owner/parcel search — never web-search an address, owner name, or parcel record.** If you don't know something, ask the user.

If the request came from a voice note, a forwarded email, or any ambiguous mention, do NOT touch the organizer yet. First read back what you think you heard and confirm two things before any write:
- **The address** — full street address, city, state, zip. Repeat it and ask the user to confirm or correct.
- **The client's name** — the person YOU represent on this deal (not the counterparty).

Example: "Sounds like a new buyer deal for Nate Ruiz on 4820 Birch Court — is that right, and is that the full address?"

Only proceed once the user confirms both.

## 2. Resolve the client in the Contacts tab

Read the **Contacts tab** — the source of truth for people (see `references/sheets-schema.md`) — and search it for the client by the name the user gave. Use nickname intelligence — "Nate" should match "Nathan," "Liz" should match "Elizabeth," "Bob" should match "Robert." Search generously, then confirm the specific person.

- **One clear match** → confirm it: "I found Nathan Ruiz (nate.ruiz@gmail.com) — is that your client?" Reuse that person's existing `contact_id`.
- **Multiple candidates** → list them and ask which one, then use the chosen person's `contact_id`.
- **No match** → create a new row for the client in the Contacts tab. Assign a new `contact_id` (short and readable, e.g. `C001`; must be unique in the tab), set `role_labels` to include **Client** (labels are additive — append the label, never strip an existing one), and capture `phone`/`email` if the user has them handy. Stamp `created_at`/`updated_at`, and set `validation_status = needs_validation` unless the user confirms the details. This add is an internal capture: make it, then read the row back to confirm it saved.

Hold onto the client's `contact_id` — it goes in `client_contact_id` on the Deals row.

## 3. Ask the side and the deal type

Two questions decide which template and which downstream deadlines/tasks apply:

- **Side** — are you representing the **buyer**, the **seller**, or **both** (dual)? Sets `side` = `buyer | seller | dual`.
- **Deal type** — is this **cash** or **financed**? This drives which deadlines and tasks apply later (cash deals skip loan and appraisal work). Record it in `financing_type` (e.g. "cash" or "conventional / FHA / VA — financed"). If the user isn't sure yet, capture what they know and mark the deal `needs_validation`.

## 4. Pick the template and the starting status

Choose exactly one `template_key`, based on side and how far along the deal is:

| Situation | side | template_key | starting status |
|---|---|---|---|
| New listing, not yet under contract | seller | `listing_launch` | `pre_listing` (or `active` if already live on MLS — ask) |
| New buyer, still shopping / writing offers | buyer | `buyer_fast_track` | `active` |
| Seller-side deal already under contract | seller | `listing_uc` | `under_contract` |
| Buyer-side deal already under contract | buyer | `buyer_uc` | `under_contract` |

If the user is unsure whether the deal is under contract, ask: "Is there a signed, mutually-accepted contract yet, or are you still in the listing/shopping stage?" — the answer decides launch vs. UC template.

For a **dual** deal, you will create two rows (see §7): a seller-side row and a buyer-side row, each with its own template_key.

## 5. Create the Deals row

Read the "Deals" tab (`references/sheets-schema.md` has the full column list) so you know every column and its exact order. Fill what you know; leave the rest blank (never guess a value into a cell). At minimum set:

- `deal_id` — a new unique id (short and readable, e.g. `deal_<address-slug>` or a timestamp-based id; must be unique in the tab)
- `address`, `city`, `state`, `zip`, `property_type` (ask if unknown)
- `owner_name` — from the user/contract only
- `client_name`, `client_contact_id` — from §2 (the client's FK into the Contacts tab)
- `side`, `status`, `template_key`, `financing_type` — from §3–§4
- `source` — how the deal originated in plain words (e.g. "referral," "past client," "Zillow lead")
- `validation_status = needs_validation`
- `validation_source` — "user (new-deal intake)"; `created_at`, `updated_at` — today's date/time. Leave `validated_at` **blank** until the user actually confirms the deal — it records the confirmation time, not the capture time (schema §4), so a `needs_validation` row must not carry a `validated_at`.

Leave price/date/party columns (`list_price`, `mec_date`, `close_date`, `coop_agent_name`, `lender_name`, `title_company`, etc.) blank — those get filled by `process-contract` from the signed documents. Do not create any Deadlines rows or Calendar events here; deadlines come from the contract.

**Write the whole row in one pass.** Write the Deals row as a single full-width batch across all 45 columns (A–AS) — every cell in order, including the blanks — never as a set of partial patches. The timestamps (`created_at`/`updated_at`) now sit near the front (columns B and C), so writing the full width in one go keeps every value under its own column and prevents trailing-cell drift.

**Verify-after-write:** re-read the whole row in one consolidated read (the entire A–AS range at once, not cell-by-cell re-reads) and confirm the values landed under the right columns before telling the user the deal exists.

## 6. Create the Drive subfolder

Inside the top-level **Kai-RE** Drive folder, create a subfolder named by the property address (e.g. `4820 Birch Court`). This is where `process-contract` will file the deal's PDFs.

- First search the Kai-RE folder for an existing subfolder with that address — if one already exists, reuse it, don't make a duplicate.
- After creating, read the folder metadata back and keep the real folder id. If the Drive connection fails, say so plainly ("I couldn't reach Google Drive to make the deal folder — reconnect Drive and I'll finish setting up") and don't claim the folder exists.

## 7. Seed the task checklist

Each template has a companion file in `references/task-templates/`:

- `listing_launch` → `references/task-templates/listing-launch.md`
- `buyer_fast_track` → `references/task-templates/buyer-fast-track.md`
- `listing_uc` → `references/task-templates/listing-under-contract.md`
- `buyer_uc` → `references/task-templates/buyer-under-contract.md`

Read the matching template file. It lists tasks each with a stable `task_key`, a `title`, a `milestone`, and a sort order. Then:

1. **Read the "Tasks" tab first**, filtered to this `deal_id`, and collect the `task_key`s already present. (There are no unique constraints in Sheets — this read-before-write is what prevents duplicates.)
2. For every task in the template whose `task_key` is **not** already on this deal, append a Tasks row: `task_id` (new unique id), `deal_id`, `address`, `template_key`, `task_key` (copy **verbatim** from the template — never reword the key), `title`, `milestone`, `status = pending`, `assigned_to = agent` (the default for transaction/template tasks), `due_date` (leave blank unless the template gives a rule and you have the anchor date), `show_on_calendar = no` (the default — a deal's ~50 tasks would swamp the calendar), `calendar_event_id` blank, `completed_at` blank, `source = template seed`, `notes` blank.
3. Skip cash-inapplicable tasks only if the template marks them loan/appraisal-specific AND the deal is cash — otherwise seed everything and let the user skip later.
4. **Report New vs. Existing counts:** "Seeded 40 new tasks (0 already existed)." If some task_keys were already present, say how many were skipped as existing.

**Verify-after-write:** re-read the Tasks tab for this deal and confirm the count matches what you reported.

## 8. Handle dual representation and linked deals

- **Dual representation** — the user represents both sides. Create **two** Deals rows that share the same address: one with `side = seller` / `template_key = listing_uc` (or `listing_launch`) and one with `side = buyer` / `template_key = buyer_uc` (or `buyer_fast_track`). Give each its own `deal_id`. Set `side = dual` only if you truly track it as a single row by user preference; otherwise the two-row split is the default. Seed each row's tasks from its own template.
- **Linked / contingent deal** — e.g. a client selling one home contingent on buying another. Create the second deal normally, then set `linked_deal_id` on **both** rows to point at each other's `deal_id`, and note the contingency in `notes`. Confirm with the user which deal is contingent on which.

## 9. Confirm the summary back

Close with a short plain-English recap and the confirm list of anything you marked `needs_validation`:

> Created your new **buyer** deal for **Nathan Ruiz** at **4820 Birch Court, [City, ST ZIP]** (financed). Set up the Drive folder and seeded **17** buyer fast-track tasks. A few things I marked for you to confirm when you get a chance: property type, and whether financing is conventional or FHA.
>
> When you have the signed contract, download it to your Downloads folder or the deal folder and say "process the Birch Court contract" — I'll pull the dates onto your calendar.

Keep it to what you actually wrote and read back. Never report a folder, row, or task count you didn't verify.
