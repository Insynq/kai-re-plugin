# Deadline Taxonomy

This is your reference for reading deadlines out of a real estate contract and knowing what each one protects. Use it every time you extract dates from a signed contract, and every time an email hints that a deadline has been met.

The **categories, conventions, and safety rules in this file are universal** — they hold in every U.S. state. The **specific deadline slots** are not: each state's standard contract names and numbers its deadlines differently. This file ships with Colorado's contract as a fully worked example (clearly marked at the bottom). During onboarding you replace that worked example with the user's own state contract vocabulary, learned from their first real contract.

Never invent a deadline. If the contract doesn't print a date and doesn't give you an offset to compute one, the deadline is not set — say so.

---

## 1. The 8 universal deadline categories

Every residential purchase contract organizes its deadlines into roughly these eight families. Learn what each one *protects* — that is what tells you whether it is briefing-critical and whether you are ever allowed to close it on your own.

### 1. Title
**Protects:** the buyer's right to receive clean, marketable ownership — no undisclosed liens, easements, or ownership defects.
**Typical deadlines inside it:**
- Record title delivered (seller/title company provides the title commitment)
- Buyer's deadline to object to anything in the record title
- Off-record matters delivered (easements, encroachments, use restrictions not in public record)
- Buyer's deadline to object to off-record matters
- Title resolution — the final date by which raised title objections must be cured or the deal can terminate

### 2. Association / HOA
**Protects:** the buyer's right to review homeowners-association or condo governing documents, budgets, dues, and rules before being bound to them.
**Typical deadlines inside it:**
- Association documents delivered
- Buyer's right to terminate based on those documents
- Buyer's deadline to object to association matters

### 3. Seller Disclosure
**Protects:** the buyer's right to receive the seller's written disclosure of known property conditions and defects.
**Typical deadlines inside it:**
- Seller's property disclosure delivered to the buyer

### 4. Loan & Credit
**Protects:** the buyer's financing contingency — their ability to walk away (usually with earnest money returned) if they cannot secure the loan.
**Typical deadlines inside it:**
- Loan application deadline
- Loan terms / rate-lock documentation
- Loan availability or underwriting-approval deadline
- The buyer's deadline to notify of loan disapproval (the walk-away date if financing falls through)
- For assumptions/existing loans: transfer-approval deadlines

### 5. Appraisal
**Protects:** the buyer's right not to overpay — the ability to renegotiate or terminate if the property appraises below the purchase price.
**Typical deadlines inside it:**
- Appraisal completed / received
- Buyer's deadline to object to a low appraisal
- Appraisal resolution deadline

### 6. Survey
**Protects:** the buyer's right to know the exact boundaries, easements, and improvements location before closing.
**Typical deadlines inside it:**
- New survey delivered
- Buyer's deadline to object to survey findings
- Survey resolution deadline

### 7. Inspection & Due Diligence
**Protects:** the buyer's right to inspect the physical condition of the property and terminate or renegotiate over defects. This is the most commonly exercised contingency.
**Typical deadlines inside it:**
- Inspection termination deadline (the outer date the buyer can walk for any inspection reason)
- Inspection objection deadline (deliver a written list of items to the seller)
- Inspection resolution deadline (items must be agreed/resolved or the deal can end)

### 8. Closing & Possession
**Protects:** the completion of the sale and the handoff of the keys.
**Typical deadlines inside it:**
- Closing date
- Possession date
- Possession time
- (Often paired with the acceptance/offer-expiration deadline that opened the contract)

---

## 2. Deal-type applicability matrix

Not every category applies to every deal. Before you extract, know the deal's financing type and property type, and skip the categories that don't apply. Skipping is correct — do not manufacture a loan deadline for a cash deal.

| Category | Cash | Conventional | FHA / VA | Loan Assumption |
|---|---|---|---|---|
| Title | All | All | All | All |
| Association / HOA | Only if HOA or condo | Only if HOA or condo | Only if HOA or condo | Only if HOA or condo |
| Seller Disclosure | All | All | All | All |
| Loan & Credit | **Skip all** | All | All | Partial (transfer/assumption approval only) |
| Appraisal | Optional — often skipped | All | All | Optional |
| Survey | Only when a survey is called for | Only when called for | Only when called for | Only when called for |
| Inspection & Due Diligence | All | All | All | All |
| Closing & Possession | All | All | All | All |

Rules of thumb:
- **Cash deals skip Loan & Credit entirely** and usually skip Appraisal (there's no lender requiring one — include it only if the contract explicitly keeps an appraisal contingency).
- **Association applies only if the property is in an HOA or is a condo/townhome.** Single-family with no association → skip.
- **Survey applies only when the contract calls for a new survey** — common on single-family and land, rare on condos.

If you're unsure whether a category applies, ask the user rather than guess.

---

## 3. Conventions — how deadline dates work

These hold across states unless the specific contract says otherwise.

- **Most deadlines anchor to mutual execution (MEC).** MEC = the date the *last principal* (buyer or seller) signs the contract. **Brokers/agents signing do not count** — only the buyer and seller. When a deadline is written as "X days after MEC," count from that date.
- **Calendar days, not business days,** unless the contract explicitly says "business days."
- **Weekend/holiday rollover.** If a computed deadline lands on a weekend or a federal/state holiday, it rolls forward to the next business day.
- **No time given → 11:59 PM local time** on the deadline date.
- **A blank deadline means waived.** If the contract leaves a deadline slot empty, that provision is waived — it does not apply to the deal. Do not fill it in with a default.
- **Counter-proposals override the base contract.** If a counter-proposal or amendment changes a deadline, the newer date supersedes the base contract's date. Always read the full document chain (base contract + counters + amendments) and let the newest value win per field before you write anything.
- **Funds and final signatures are typically due about 3 business days before closing.** Lenders and title companies need clear funds ahead of the closing date; flag this cadence even if the contract doesn't print it as its own deadline.

---

## 4. Extraction guidance

When you pull deadlines out of a contract:

1. **Prefer the literal dates printed on the signed contract.** If the contract shows an actual calendar date in the deadline table, use that date exactly as printed. Do not recompute it.
2. **Compute only when the contract gives you an offset** (e.g., "10 days after MEC" with no printed date). Then apply the conventions in §3: count calendar days from MEC, roll weekends/holidays forward, default to 11:59 PM.
3. **Source-document dates, never email timestamps.** The date a deadline is due comes from the contract, not from when an email arrived. An email saying "we're under contract!" does not set the MEC — the signature date on the document does.
4. **Read the complete document chain before writing any date.** Base contract, every counter-proposal, every amendment. Newest value wins per field. A counter that moved the closing date must beat the original.
5. **Record where each date came from.** Every extracted deadline row stores its `source_document`, and if it came from an email lane, its `source_message_id`. This is your "where did this date come from?" trail.
6. **When a deadline isn't set, leave it unset and say so.** Blank in the contract = waived. Missing entirely and no offset given = not determinable — flag it, never guess a default.

---

## 5. Briefing-critical vs. informational

Not every deadline deserves equal alarm. Use this split to decide what leads a briefing and what merely gets recorded.

**Briefing-critical** (these can end a deal or cost the client money if missed — surface them prominently, flag when imminent or overdue):
- Inspection (termination / objection / resolution)
- Appraisal (objection / resolution)
- Loan & Credit (loan disapproval / financing walk-away)
- **Title resolution** (the final cure date)
- Closing date
- Possession date/time

**Informational** (record them, but they don't need to lead a briefing):
- Record title delivery / off-record title delivery (the *delivery* dates — the *resolution* date is critical)
- Association document delivery and objection
- Survey delivery, objection, resolution

When in doubt, treat anything that gives the client a right to terminate or object as briefing-critical.

---

## 6. NEVER_AUTO_CLOSE — deadlines you may never resolve on your own

Some deadlines exist specifically to protect the client's right to **object or walk away**. You may *surface* evidence that one of these appears satisfied, but you must **never mark it complete, waived, or missed on your own**. Only the user, or the client acting through the user, can close these. Always leave them for explicit human decision.

| Deadline | Why it can never be auto-closed |
|---|---|
| Inspection termination | This is the buyer's outright right to walk away over property condition. Auto-closing it could silently strip the client's exit. |
| Inspection objection | Missing it can forfeit the client's leverage to demand repairs or credits. |
| Loan disapproval / financing walk-away | This is the client's financing exit — auto-resolving it could trap a buyer who can't actually get the loan. |
| Appraisal objection | Protects the client from overpaying on a low appraisal; premature closure removes their renegotiation right. |
| Title objection(s) | Protects the client from taking title with undisclosed defects, liens, or encumbrances. |

For all of these: an email that *looks* like good news ("inspection went great!") only ever **proposes** that the deadline is satisfied. You show the proposal and the evidence; the user decides. See §8.

Related honesty rules that always apply:
- **All-clear claims must state counts and only follow reads that actually ran.** Say "0 deadlines due in the next 3 days" only after you actually read the deadlines and calendar. If a Google connection failed, name it — never silently skip and call it clear.
- **Don't create past-dated deadlines on intake.** If a contract's dates are already in the past and there's evidence the deal has closed, don't flood the calendar with fake-overdue alerts — flag "is this deal already closed?" and ask.

---

## 7. WORKED EXAMPLE — Colorado (CBS1 §3.1)

> ⚠️ **THIS SECTION IS A WORKED EXAMPLE ONLY.**
> The slots below are from the Colorado *Contract to Buy and Sell Real Estate* (CBS1), Section 3.1 — one specific state's contract. **During onboarding, you replace this entire vocabulary with the user's own state's contract**, learned from their first real contract (§7b below). If the user is not in Colorado, do not use these slot names or section numbers — they will be wrong. Keep the categories, conventions, and safety rules above; swap out the slots.

The Colorado CBS1 §3.1 defines its deadlines in the eight categories from §1. Representative slots:

**Title**
- Record Title Deadline
- Record Title Objection Deadline
- Off-Record Title Deadline
- Off-Record Title Objection Deadline
- Title Resolution Deadline

**Association (HOA/Condo)**
- Association Documents Deadline
- Association Documents Termination Deadline
- Association Documents Objection Deadline

**Seller's Disclosure**
- Seller's Property Disclosure Deadline

**Loan & Credit** (financed deals only)
- New Loan Application Deadline
- New Loan Terms Deadline
- New Loan Availability Deadline
- Buyer's Credit Information Deadline
- Loan Disapproval Deadline *(NEVER_AUTO_CLOSE)*
- Existing Loan Deadline
- Loan Transfer Approval Deadline
- Seller / Private Financing Deadline

**Appraisal**
- Appraisal Deadline
- Appraisal Objection Deadline *(NEVER_AUTO_CLOSE)*
- Appraisal Resolution Deadline

**Survey**
- New Survey Deadline
- New Survey Objection Deadline
- New Survey Resolution Deadline

**Inspection & Due Diligence**
- Inspection Termination Deadline *(NEVER_AUTO_CLOSE)*
- Inspection Objection Deadline *(NEVER_AUTO_CLOSE)*
- Inspection Resolution Deadline

**Closing & Possession**
- Closing Date
- Possession Date
- Possession Time
- Acceptance Deadline (offer expiration; auto-completes once the deal goes under contract)

Colorado conventions confirmed by this example: deadlines are counted in **calendar days after MEC**, expire at **11:59 PM Mountain Time** when no time is given, a **blank slot is waived**, and a **counter-proposal supersedes** the base CBS1 date.

### 7b. Your state's deadlines (fill in during onboarding)

> The agent populates this section from the user's first real signed contract. Until then it is empty — do not assume it matches Colorado.

When you read the user's first contract, capture their state's actual deadline vocabulary here so future extractions map cleanly:

- **State:** _[fill in]_
- **Standard contract name / form code:** _[fill in — e.g., the user's state association purchase contract]_
- **Deadline slots by category** (record the exact names as printed on their contract, grouped under the 8 universal categories from §1):
  - Title: _[fill in]_
  - Association / HOA: _[fill in]_
  - Seller Disclosure: _[fill in]_
  - Loan & Credit: _[fill in]_
  - Appraisal: _[fill in]_
  - Survey: _[fill in]_
  - Inspection & Due Diligence: _[fill in]_
  - Closing & Possession: _[fill in]_
- **Anchor convention:** _[fill in — does this state count from MEC / acceptance / another event?]_
- **Default expiry time & timezone:** _[fill in]_
- **Any state-specific NEVER_AUTO_CLOSE additions:** _[fill in]_

Once filled in, prefer these names over the Colorado example when extracting and when naming deadlines to the user.

---

## 8. Email-evidence signal table

Between contracts, plain-English emails often hint that a deadline has been met. Use this table to recognize the signal — but remember the hard rule below the table.

| Plain-English email phrase (examples) | Suggests this deadline is satisfied | Category |
|---|---|---|
| "Inspection went great," "no major issues," "buyer is satisfied / removing inspection" | Inspection objection/resolution *(NEVER_AUTO_CLOSE — propose only)* | Inspection |
| "Appraisal came in at value," "appraised at/above contract price" | Appraisal objection/resolution *(NEVER_AUTO_CLOSE — propose only)* | Appraisal |
| "Clear to close," "final loan approval," "underwriting approved" | Loan availability / financing contingency | Loan & Credit |
| "Title commitment attached/delivered," "commitment is out" | Record title delivered | Title |
| "Seller's disclosure attached," "SPD received" | Seller disclosure delivered | Seller Disclosure |
| "HOA docs delivered," "association packet sent" | Association documents delivered | Association / HOA |
| "Survey is in," "survey resolved / no issues" | Survey delivery/resolution | Survey |

**The rule for this entire table: an email signal only ever PROPOSES a deadline update — it never closes one.** You surface the signal, name the deadline it points to, quote the evidence, and let the user confirm before you change any status. This is doubly true for anything marked NEVER_AUTO_CLOSE in §6: those you may only ever propose, never mark done. Source-document dates still govern — an email is a hint to go look, not the record itself.
