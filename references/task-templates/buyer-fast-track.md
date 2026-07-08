# Task Template — Buyer Fast Track

- **template_key:** `buyer_fast_track`
- **When it applies:** Onboarding a new buyer client, from first contact up to (but not including) an accepted offer. Use it when you create a buyer-side deal that is not yet under contract. When the buyer's offer is accepted, the deal graduates to the `buyer_uc` template (`buyer-under-contract.md`).
- **Milestones:** This is a **flat checklist** — a single "Buyer Onboarding" group with no left-to-right dependencies. Complete (or skip) every task to prepare the buyer for their first offer.

## How to use this file
Seed one row per task below into the **Tasks** tab, keyed by `task_key` (stable idempotency key — keep exactly as written, never duplicate for one deal). Mark tasks that do not apply as `skipped`, not `complete`. If a needed task has no key, propose a new snake_case key and ask the user before adding it.

**Universal gate:** `confirm_preapproval` must be complete before any offer goes out. No offers without a confirmed pre-approval.

Outward steps (any "send" task) require showing the exact draft and getting an explicit yes first.

---

## Buyer Onboarding

### Setup
| task_key | title | notes / when |
|---|---|---|
| `crm_buyer_setup` | Set up the buyer in your CRM | Only if you use a CRM. |
| `create_buyer_drive_folder` | Create the deal's Drive folder | A subfolder inside the "Kai-RE" folder. |

### Buyer agreement & disclosures
| task_key | title | notes / when |
|---|---|---|
| `send_buyer_package` | Send the buyer welcome package | Outward — confirm first. A relationship piece (intro, lender info) — not the legal agreement. |
| `prepare_buyer_agreement` | Prepare the buyer-representation agreement | In your e-signature platform. |
| `prepare_wire_fraud_buyer` | Prepare the wire-fraud awareness disclosure | Wire-fraud acknowledgment for the buyer. |
| `send_buyer_agreement_sigs` | Send the buyer agreement and disclosures for signatures | Outward — confirm first. After your own review. |
| `confirm_buyer_agreement` | Confirm the buyer agreement is executed | File the executed copy in the deal's Drive folder. |

### Financial preparation
| task_key | title | notes / when |
|---|---|---|
| `lender_intro` | Introduce the buyer to a lender | A warm handoff to your preferred lender. |
| `confirm_preapproval` | Confirm the pre-approval is received | **Gate: no offers without pre-approval confirmed.** |

### Property search setup
| task_key | title | notes / when |
|---|---|---|
| `setup_search_portal` | Set up a property-search portal | Your MLS portal or search tool. |
| `share_search_access` | Share saved-search access | Only if the buyer prefers a third-party site. |
| `send_home_questionnaire` | Send the ideal-home questionnaire | Budget, must-haves, deal-breakers — a simple conversation works. |

### Consultation & first showing
| task_key | title | notes / when |
|---|---|---|
| `schedule_consultation` | Schedule the buyer consultation | |
| `coordinate_first_showing` | Coordinate the first showing | Via your showing service. |
| `confirm_property_facts_buyer` | Confirm property facts for showing properties | Verify beds, baths, square footage, and lot with the client or listing. Replaces a county-report lookup. |

---

## Template transition
When the buyer's offer is accepted: the offer is prepared and signed during the search phase, then sent to the listing agent. Once the listing agent confirms acceptance, create a fresh set of tasks from `buyer-under-contract.md` (`buyer_uc`) and change the deal `status` to `under_contract`.
