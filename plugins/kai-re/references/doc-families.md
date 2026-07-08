```markdown
# Reference: Document Families

This is the taxonomy the agent uses to classify any transaction document, decide what to pull from it, where to file it, and what status action it triggers. Used by `skills/process-contract.md` (Step 2 onward) and `skills/catch-up.md`.

**Two layers:**
1. **Families** (below) are universal — they don't change from state to state or brokerage to brokerage. Classify every document into exactly one family first.
2. **Form names** (the specific document titles and codes a document shows — e.g., a listing contract's official name in your state) are **learned from the user's own documents at onboarding** and recorded in the **Form Map** at the bottom of this file. When you see a form name or code on a page, look it up in the Form Map to get the family and type; if it's not yet mapped, classify by reading the document's content and propose adding it to the map.

Never hardcode one state's form codes as if they were universal. The families are the stable thing.

---

## How to classify

1. Read the document. Identify its purpose from the title, the section headings, and the signature block — **not** from the email subject.
2. Match it to a **family** below.
3. Within the family, name the **specific type** using the Form Map. If unmapped, describe it plainly and offer to add it.
4. Detect **execution state** from the signature blocks: `draft` (no signatures / prep only), `partially_signed` (one side signed), `executed` (all required parties signed), `voided`.
5. Extract the family's field list, file per the family's filing rule, and take the family's status action.

The `document_type` you store on the Documents row should be a stable, plain slug for the type (e.g., `purchase_contract`, `counterproposal`, `listing_agreement`, `inspection_objection`) — consistent across deals so dedup on (deal, document_type) works.

---

## Family 1 — Purchase Contract

The core agreement to buy and sell a property. The richest extraction of any family.

**Signals:** purchase price, earnest money, financing terms, a table of deadlines, buyer and seller signature blocks.

**Extract:**
- Purchase/sale price
- Earnest money amount + due date + form/holder
- Financing type (conventional / FHA / VA / cash / other)
- MEC date (mutual execution — when both principals have signed)
- Close date; possession date + time
- Seller concession (if any)
- MLS number, property type
- **Every deadline** in the contract's deadline section (apply cash-vs-financed applicability per `references/deadline-taxonomy.md`)
- All parties named (buyer, seller, and any co-op agent, lender, title company/closer, inspector referenced) → Contacts + Deals columns
- Signature status from the signature blocks

**Filing:** into the deal's Drive subfolder as `<address> - Purchase Contract - MM-DD-YYYY.pdf`.

**Status action:** a **fully-executed** purchase contract (buyer(s) + seller(s) signed) is the trigger to **propose** advancing the deal to `under_contract` and seeding the under-contract task template. A `draft` or `partially_signed` one is an active offer — record it, don't advance.

**Self-check:** a purchase contract with fewer than 5 deadlines, or missing price / earnest money / financing type / close date, is under-extracted — re-read before presenting.

---

## Family 2 — Representation / Listing Agreement

The agreement establishing that the user represents this client — the listing agreement (seller side) or buyer-representation agreement (buyer side), plus any pre-engagement working-relationship disclosures.

**Signals:** commission/compensation terms, a representation period, client + agent signature blocks; "this is not the contract to buy/sell the property."

**Extract:**
- List price (seller side)
- Commission / compensation rate and structure
- Representation period (start/end)
- Representation mode (agency vs. transaction-brokerage, if the form distinguishes)
- Client name(s) → Contacts (label: Client) + Deals columns
- Signature status

**Filing:** `<address or client> - Listing Agreement - MM-DD-YYYY.pdf` (or `- Buyer Agreement -`).

**Status action:** a fully-executed listing agreement supports moving a seller deal into the pre-listing/listing-launch stage and seeding the listing-launch task template. A buyer-representation agreement establishes an active buyer deal. Representation agreements are just Documents rows — there is no separate table.

---

## Family 3 — Amendment / Counterproposal

A document that changes an existing contract's terms. **Two behaviors — know which:**

- **Counterproposal** → **supersedes** the prior version per field. For each field it names, its value replaces the earlier one (newest wins). A fully-executed counter can itself be the mutual-execution trigger.
- **Amendment / amend-and-extend** → **stacks**. It adjusts specific terms/deadlines on top of the existing contract; earlier terms it doesn't mention stay in force.

**Signals:** "counterproposal," "amend," "extend," references a prior contract by date, changes to price, deadlines, or the close date.

**Extract:**
- **Only the deltas** — the changed price, changed deadlines, changed close/possession, changed fee allocation.
- Which contract it modifies, and whether it's a purchase-side or representation-side change.
- Signature status.

**Filing:** `<address> - Counterproposal - MM-DD-YYYY.pdf` / `- Amendment - MM-DD-YYYY.pdf`. Amendments stack as separate rows; a superseding counter voids the prior counter's row.

**Status action:** re-derive the affected Deals terms and Deadlines with newest-wins, update the corresponding Calendar events (by stored `calendar_event_id`), and **always ask the user "did this change any deadline?"** A fully-executed counter on an active offer can advance the deal to `under_contract`.

---

## Family 4 — Disclosure

Informational documents a party is required to provide or acknowledge (property condition, square footage, water source, lead-based paint, wire-fraud and affiliated-business advisories, etc.).

**Signals:** "disclosure," "advisory," "acknowledgment"; acknowledgment signature blocks; "this is not a contract."

**Extract:**
- The disclosing party and the acknowledging party → Contacts if new
- Any flagged/"yes" issues on a property-condition disclosure (surface these to the user)
- Square footage + source, water provider, etc., as reference data on the deal's notes
- Signature status (usually acknowledgment)

**Filing:** `<address> - <Disclosure Name> - MM-DD-YYYY.pdf`. One Documents row each; these are compliance/reference records.

**Status action:** none on their own — log the document, surface any flagged condition issues. A property-condition disclosure with flagged repairs is worth pointing out to the user.

---

## Family 5 — Objection / Resolution

Notices raising or resolving a contingency — inspection objections, appraisal-value objections, and their resolutions.

**Signals:** "objection," "notice," "resolution"; references an inspection/appraisal deadline; often signed by one side only (objections are usually unilateral).

**Extract:**
- Which contingency (inspection, appraisal, title, etc.)
- The objection items or the agreed resolution items
- Any cost allocation stated in a resolution
- Signature status (objections often single-party)

**Filing:** `<address> - Inspection Objection - MM-DD-YYYY.pdf` / `- Inspection Resolution -` / `- Appraisal Objection -`.

**Status action:** mark the **matching deadline as signaled** — but these touch the **NEVER_AUTO_CLOSE** contingencies. Never mark an objection deadline `complete` or `waived` on your own; surface it and let the user decide. A binding **resolution** may complete the resolution deadline — still confirm with the user first, because it bears on the client's right to walk away.

---

## Family 6 — Termination

A document ending the contract, and its earnest-money release counterpart.

**Signals:** "notice to terminate," "termination," "mutual release," "earnest money release."

**Extract:**
- The terminating party and the effective date
- Earnest-money disposition (who receives it) on a release
- Signature status

**Filing:** `<address> - Notice to Terminate - MM-DD-YYYY.pdf` / `- Earnest Money Release -`. A termination and its EM release are two separate Documents rows on the same deal.

**Status action:** **propose** setting the deal to `terminated` (or `withdrawn`, as applicable), and surface every pending deadline that should now be cancelled — but do the status change and the deadline cancellations only after the user confirms. Never auto-terminate a deal.

---

## Family 7 — Closing Docs

Documents that set up or complete the closing — closing instructions and the settlement/closing statement the user reviews.

**Signals:** "closing instructions," "settlement statement," closing company as a signer, figures for proceeds/costs at closing.

**Extract:**
- Closing/title company and the closer contact → Contacts (label: Title) + Deals columns (title_company, title_closer)
- Closing date/details
- Signature status

**Filing:** `<address> - Closing Instructions - MM-DD-YYYY.pdf` / `- Closing Statement -`.

**Status action:** log the document and fill title company/closer. Completion of the closing milestone supports proposing `closed` — but only the user confirms a deal closed; do not auto-close. This slice does no commission or settlement accounting — a closing statement is filed and its title contacts captured, nothing more.

---

## Contact roster (primary-source-first)

Some packets include an authoritative **contact roster / e-contacts sheet** listing every party in the transaction. When present, this is the **primary source** for party extraction — process it **first**, before the individual documents, and treat other documents' party mentions as supplementary. It is a contacts source, not a filed contract: extract its people to Contacts, but it doesn't need its own tracked Documents row unless the user wants it kept.

---

## Form Map (fill in per user at onboarding — seeded empty)

Record each specific form the user actually works with, mapping its real name/code to a family and a stable `document_type` slug. Learn these from the user's own documents during onboarding; add a row whenever you encounter a new form. **Do not pre-populate with another state's codes.**

| User's form name / code | Family | `document_type` slug | Notes |
|---|---|---|---|
| _(e.g., your state's standard residential purchase contract, whatever your forms call it — illustration only; replace with the user's actual state forms at onboarding)_ | Purchase Contract | `purchase_contract` | Example placeholder — delete at onboarding |
| | | | |
| | | | |

When a document shows a form name that isn't in this table, classify it by reading its content, tell the user which family you placed it in, and offer to add the mapping.
```
