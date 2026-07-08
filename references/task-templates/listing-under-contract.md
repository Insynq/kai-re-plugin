# Task Template — Listing Under Contract

- **template_key:** `listing_uc`
- **When it applies:** A seller-side deal from offer accepted through closing and post-close. Use it after an offer is accepted on a `listing_launch` deal; set the deal `status` to `under_contract`.
- **Milestones (in order):** Offer Accepted → Inspection → Appraisal → Closing Date (plus a Pre-Acceptance group for multi-offer situations and a Post-Close group).

## How to use this file
Seed one row per task below into the **Tasks** tab, keyed by `task_key` (the stable idempotency key — keep exactly as written, never duplicate for one deal). Mark tasks that do not apply as `skipped`, not `complete`. If a needed task has no key, propose a new snake_case key and ask the user before adding it.

Two safety rules govern this template specifically:
- **Deadline gate:** the `extract_deadlines` task runs `skills/process-contract.md`. Read the complete document chain (contract + counters + amendments, newest value wins per field) before writing any date, and record `source_message_id` on email-derived rows.
- **Never auto-close decisions:** the inspection-objection and appraisal-objection tasks are decision points. Never resolve them on the user's behalf — surface the decision and wait. Any "all clear" statement must cite counts from reads that actually ran.

Outward steps (any "email/send" or Calendar-invite-with-attendees task) require showing the exact draft and getting an explicit yes first.

---

## Pre-Acceptance (multi-offer only)
Skip all three for a single-offer deal.

| task_key | title | notes / when |
|---|---|---|
| `create_offer_comparison` | Create an offer-comparison summary | When multiple offers arrive. |
| `update_offer_comparison` | Add offers to the comparison as they arrive | Ongoing until the client selects. |
| `select_winning_offer` | Client selects the winning offer | Triggers the acceptance flow below. |

---

## Offer Accepted → Inspection
Acceptance, distribution, title order, inspection coordination.

| task_key | title | notes / when |
|---|---|---|
| `confirm_seller_acceptance` | Confirm the seller's acceptance of the offer | **Milestone: Offer Accepted.** |
| `extract_deadlines` | Extract all contract deadlines to the Calendar | Run `skills/process-contract.md`. Read the full document chain first; counter-proposal dates override contract defaults. Do not create past-dated deadlines — if any date is already in the past, flag "is this deal already closed?" instead. |
| `add_signature_boxes` | Add signature boxes to the contract | In your e-signature platform. |
| `send_contract_sigs` | Send the contract to the sellers for signatures | Outward — confirm first. |
| `distribute_executed_contract` | Send the executed contract to the buyer's agent | Outward — confirm first. After all signatures are collected. |
| `update_mls_status` | Update the MLS status | Pending / Under Contract, as applicable. |
| `send_seller_congrats` | Send a congratulations email to the sellers | Outward — confirm first. Attach documents for their records. |
| `send_title_order` | Send the title order | Outward — confirm first. Include the contract, source-of-water letter (if your state uses one), and closing instructions. |
| `send_distribution_email` | Send the distribution email to title, buyer's agent, and lender | Outward — confirm first. |
| `file_contract_to_drive` | File the contract and related docs in the deal's Drive folder | |
| `update_crm_pending` | Update your CRM status to Pending | Only if you use a CRM. |
| `obtain_em_receipt` | Obtain the earnest-money receipt from title | |
| `obtain_dd_docs` | Obtain due-diligence docs from the sellers | Send to the buyer's agent. |
| `ask_off_title_items` | Ask the sellers about off-title items | Send to the buyer's agent. |
| `confirm_inspection_showing` | Confirm the inspection date and time | Notify the seller via your showing service. |
| `obtain_title_commitment` | Obtain the title commitment and HOA docs from title | Send to the seller and the buyer's agent. |

### Inspection objection flow (skip if buyer accepts as-is)
| task_key | title | notes / when |
|---|---|---|
| `receive_inspection_objection` | Receive the buyer's inspection objection | |
| `review_inspection_objection` | Review the objection with the seller | Decide: accept, counter, or reject. Never auto-resolve. |
| `respond_inspection_objection` | Respond to the objection | Via your e-signature platform, per the seller's decision. |
| `negotiate_inspection` | Negotiate the resolution | May go back and forth. |
| `confirm_inspection_resolution` | Confirm the resolution agreement is signed | **Milestone: Inspection.** |

---

## Inspection → Appraisal
| task_key | title | notes / when |
|---|---|---|
| `confirm_appraisal_scheduled` | Confirm the appraisal is scheduled | If it is not scheduled within ~10 days of going under contract, check with the lender. |
| `confirm_appraisal_results` | Confirm the appraisal results | Notify the seller. |
| `handle_appraisal_objection` | Handle any appraisal objection | If it comes in low: accept a lower price, contest, or negotiate. Never auto-resolve. **Milestone: Appraisal.** |

---

## Appraisal → Closing Date
| task_key | title | notes / when |
|---|---|---|
| `confirm_stager_removal` | Schedule staging removal | Skip if no staging. |
| `remind_seller_utilities` | Remind the seller to notify utilities | Of the buyer taking over. |
| `verify_last_mortgage` | Verify the seller's last mortgage-payment details | |
| `confirm_docs_filed` | Confirm all docs are signed and filed in the deal's Drive folder | |
| `coordinate_closing_appt` | Coordinate the closing appointment with title and seller | Add to the Calendar with the address. Get a yes before creating an invite with attendees. |
| `coordinate_final_walkthrough` | Coordinate the final walk-through with the buyer's agent | Notify the sellers. |
| `confirm_resolution_complete` | Confirm inspection-resolution items are complete | Skip if there was no objection. |
| `confirm_title_insurance` | Confirm the title insurance / commitment is satisfactory | |
| `coordinate_closing_gifts` | Coordinate closing gifts and thank-you cards | |
| `review_commission_statement` | Review your commission / closing statement | Verify it matches the listing contract before it goes to title. |
| `schedule_sign_lockbox_removal` | Schedule sign and lockbox removal | |
| `send_settlement_sellers` | Send the settlement statement to the sellers for review | Outward — confirm first. Review it yourself first. |
| `confirm_seller_moved_out` | Confirm the seller has moved out / staging removed | |
| `confirm_key_transfer` | Confirm the key-transfer plan and info to the buyer side | Manuals, remotes, etc. |
| `confirm_closing_details_4day` | Confirm closing details 4 days before close | Title, lender, sellers, buyer's agent. |
| `confirm_possession` | Confirm the possession date and time | |

---

## Post-Close
| task_key | title | notes / when |
|---|---|---|
| `confirm_escrow_closed` | Confirm close of escrow with title | Title confirms recording. |
| `update_mls_sold` | Update the MLS to Sold | Confirm you are listed as the listing agent. |
| `send_listing_congrats_final` | Send a congratulations email to sellers, title, and buyer's agent | Outward — confirm first. |
| `file_closing_docs_drive` | File the closing statement and executed contract in the deal's Drive folder | **Closed-deal completeness:** both the closing statement AND the contract must be present before this task is complete. |
| `confirm_sign_lockbox_removed` | Confirm the sign and lockbox are removed | |
| `claim_sale_review_platform` | Claim the sale on your review platform | Only if you use one. |
| `update_crm_closed` | Update your CRM status to Closed | Only if you use a CRM. |
| `deposit_commission` | Receive and deposit the commission check | |
| `schedule_listing_followup` | Schedule a 1-week post-close follow-up | Check whether the sellers need anything. |
| `add_client_followup_list` | Add the client to your follow-up list | Your long-term relationship list. |

---

## Template transition
After post-close is complete, the deal reaches the `closed` status. If the seller is also buying, link the two deals via `linked_deal_id`.
