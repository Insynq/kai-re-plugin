# Task Template — Buyer Under Contract

- **template_key:** `buyer_uc`
- **When it applies:** A buyer-side deal from mutual agreement (all parties signed) through closing and post-close. Use it after the listing agent confirms acceptance of the buyer's offer; set the deal `status` to `under_contract`.
- **Milestones (in order):** Mutual Agreement → Inspection → Appraisal → Closing Date (plus a Post-Close group).

## How to use this file
Seed one row per task below into the **Tasks** tab, keyed by `task_key` (stable idempotency key — keep exactly as written, never duplicate for one deal). Mark tasks that do not apply as `skipped`, not `complete`. If a needed task has no key, propose a new snake_case key and ask the user before adding it.

Two safety rules govern this template specifically:
- **Deadline gate:** the `extract_deadlines` task runs `skills/process-contract.md`. Read the complete document chain (contract + counters + amendments, newest value wins per field) before writing any date; counter-proposal dates override contract defaults. Record `source_message_id` on email-derived rows.
- **Never auto-close decisions:** the inspection-objection and appraisal-objection tasks are decision points, including the inspection-termination path below. Never resolve them on the user's behalf — surface the decision and wait. Any "all clear" statement must cite counts from reads that actually ran.

Outward steps (any "email/send" or Calendar-invite-with-attendees task) require showing the exact draft and getting an explicit yes first.

---

## Mutual Agreement
| task_key | title | notes / when |
|---|---|---|
| `confirm_mutual_agreement` | Confirm mutual agreement (all parties signed) | **Milestone: Mutual Agreement.** |
| `save_docs_drive` | Save all documents to the deal's Drive folder | |
| `send_wire_fraud_uc` | Send a transaction-specific wire-fraud reminder to the buyers | Outward — confirm first. Specific to this deal's wiring instructions. |
| `extract_deadlines` | Extract all contract deadlines to the Calendar | Run `skills/process-contract.md`. Read the full document chain first; counter-proposal dates override contract defaults. Do not create past-dated deadlines — flag "is this deal already closed?" instead. |
| `update_crm_pending` | Update your CRM status to Pending | Only if you use a CRM. |

---

## Mutual Agreement → Inspection
| task_key | title | notes / when |
|---|---|---|
| `send_buyer_congrats` | Send a congratulations email to the buyers | Outward — confirm first. Include the contract, earnest-money info, inspection info, and the deadline calendar. |
| `send_distribution_buyer` | Send the distribution request to lender, title, and listing agent | Outward — confirm first. |
| `confirm_em_receipt` | Confirm the earnest money is deposited | Send confirmation to the listing agent. |
| `lender_doc_followup` | Follow up with the lender — confirm all buyer docs received | An early check that the lender has everything needed to underwrite. |
| `review_loan_estimate` | Review the Loan Estimate from the lender | Financed deals only — confirm the terms match the contract (rate, loan amount, type). Skip for cash. |
| `schedule_inspection` | Schedule the home inspection | |
| `send_inspection_confirmation` | Send the inspection confirmation to the buyers | Outward — confirm first. Cost, date, time. |
| `obtain_hoa_dd_docs` | Obtain HOA documents and due-diligence materials | Send to the buyers. |
| `send_spd_to_buyers` | Send the Seller's Property Disclosure to the buyers | Outward — confirm first. If not already obtained. |
| `obtain_title_commitment_buyer` | Obtain the title commitment | Review it, then send to the buyers. |

### Inspection decision (skip objection tasks if accepting as-is)
| task_key | title | notes / when |
|---|---|---|
| `check_inspection_objection` | Check with the client before the inspection-objection deadline | Decide: accept, object, or terminate. Never auto-resolve. |
| `deliver_inspection_objection` | Deliver the inspection objection | Via your e-signature platform, if objecting. |
| `negotiate_inspection_buyer` | Negotiate the resolution | May go back and forth with the listing agent. |
| `confirm_inspection_resolution_buyer` | Confirm the resolution agreement is signed | **Milestone: Inspection.** |

> **Inspection-termination path:** if the buyer terminates under the inspection deadline, coordinate the earnest-money return with title, mark the downstream tasks `skipped`, and set the deal `status` to `terminated`. Flag this to the user immediately — do not act on the termination alone.

---

## Inspection → Appraisal
| task_key | title | notes / when |
|---|---|---|
| `schedule_resolution_contractors` | Schedule contractors for inspection-resolution items | Only if the seller agreed to repairs. |
| `confirm_appraisal_scheduled` | Confirm the appraisal is scheduled or received | Check with the listing agent or lender. |
| `confirm_appraisal_results_buyer` | Confirm the appraisal results | Notify the buyers. |
| `handle_appraisal_objection` | Handle any appraisal objection | If it comes in low: renegotiate, contest, bridge the gap, or terminate. Never auto-resolve. **Milestone: Appraisal.** |

---

## Appraisal → Closing Date
| task_key | title | notes / when |
|---|---|---|
| `confirm_clear_to_close` | Confirm with the lender: clear to close | ~1 week before the closing deadline. |
| `request_seller_questionnaire` | Request the seller questionnaire from the listing agent | |
| `send_questionnaire_buyers` | Send the questionnaire to the buyers with packing tips | Outward — confirm first. Moving and utility-transfer info. |
| `notify_utility_transfer` | Notify the buyers to transfer utilities | Effective the day of closing. |
| `coordinate_insurance` | Coordinate homeowner's insurance | Financed deals — the policy must be in place before closing (lender requirement). Cash buyers arrange independently. |
| `confirm_docs_filed_buyer` | Confirm all docs are signed and filed in the deal's Drive folder | |
| `coordinate_closing_buyer` | Coordinate closing details with title and the buyer | Include the key-exchange plan. Get a yes before creating a Calendar invite with attendees. |
| `schedule_final_walkthrough` | Schedule the final walk-through | Via your showing service, with the buyers. |
| `confirm_resolution_items_buyer` | Confirm inspection-resolution items are complete | Skip if there was no objection. |
| `coordinate_closing_gifts_buyer` | Coordinate closing gifts and thank-you cards | |

### Settlement review
| task_key | title | notes / when |
|---|---|---|
| `obtain_settlement_buyer` | Obtain the settlement statement from title | Review it yourself first. |
| `verify_settlement_credits` | Confirm all credits are accounted for | Earnest money, seller concessions, etc. |
| `send_settlement_to_buyers` | Send the settlement statement to the buyers for review | Outward — confirm first. |
| `review_commission_statement_buyer` | Review your commission / closing statement | Verify your split and fees before it goes out. |
| `send_settlement_to_title` | Send the reviewed statement to title | Outward — confirm first. Target ~5 days before closing. |
| `confirm_closing_4day_buyer` | Confirm closing details 4 days before close | Title, lender, buyers, listing agent. |
| `send_closing_reminder` | Email a closing reminder to the buyers | Outward — confirm first. |

---

## Post-Close
| task_key | title | notes / when |
|---|---|---|
| `confirm_escrow_closed_buyer` | Confirm close of escrow with title | Title confirms recording. |
| `confirm_mls_sold_buyer` | Confirm the sale is marked Sold in the MLS | Confirm you are listed as the buying agent. |
| `send_final_congrats_buyer` | Send a congratulations email to the buyer | Outward — confirm first. Include a review / referral request. |
| `file_closing_docs_drive_buyer` | File the closing statement and executed contract in the deal's Drive folder | **Closed-deal completeness:** both the closing statement AND the contract must be present before this task is complete. |
| `confirm_key_transfer_buyer` | Confirm the key transfer | If post-occupancy, track possession separately. |
| `claim_closing_review_platform` | Claim the closing on your review platform | Only if you use one. |
| `update_crm_closed_buyer` | Update your CRM status to Closed | Only if you use a CRM. |
| `deposit_commission_buyer` | Receive and deposit the commission check | |
| `schedule_followup_buyer` | Schedule a 1-week post-close follow-up | |
| `add_client_followup_list_buyer` | Add the client to your follow-up list | Your long-term relationship list. |

---

## Conditional-sale note
If the buyer's purchase is conditional on selling their current home, link the two deals via `linked_deal_id` and track both sets of deadlines in parallel. If the sale-side deal falls through, flag the risk to the user immediately.
