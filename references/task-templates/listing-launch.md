# Task Template — Listing Launch

- **template_key:** `listing_launch`
- **When it applies:** A new seller listing from first contact through going live on the market. Use it when you create a seller-side deal that is not yet under contract (status `lead`, `pre_listing`, or `active`). When an offer is accepted, the deal graduates to the `listing_uc` template (`listing-under-contract.md`).
- **Milestones (in order):** Start → Fully Executed → Coming Soon → Go Live → Active

## How to use this file
When a deal is created with this template, seed one row per task below into the **Tasks** tab (columns: task_id, deal_id, address, template_key, task_key, title, milestone, status, due_date, completed_at, source, notes). The `task_key` is the stable idempotency key — never seed the same key twice for one deal, and keep these keys exactly as written. Tasks flow left to right: a milestone is reached only when its tasks are complete or skipped.

Mark a task `skipped` (not `complete`) when it does not apply to this deal. If a task genuinely needed here has no key below, propose a new snake_case key and ask the user before adding it — do not invent one silently.

Outward or irreversible steps (any "email/send" task) follow the safety spine: show the exact draft and get an explicit yes before sending. Internal captures (Sheet rows, Drive filing, Contact adds) are written freely but marked `needs_validation` with a short confirm list.

---

## Start → Fully Executed
Contract, disclosures, and signatures.

| task_key | title | notes / when |
|---|---|---|
| `receive_intake` | Gather listing details from the client | A simple intake conversation — address, price expectations, timeline, and the property facts (beds, baths, square footage, year built, lot). No form required. |
| `create_drive_folder` | Create the deal's Drive folder | A subfolder inside the "Kai-RE" folder, named by the property address. |
| `confirm_property_facts` | Confirm property facts with the client | Verify beds, baths, square footage, year built, and lot with the client. Replaces pulling a county assessor report. |
| `save_prior_listings` | Save any prior listing history | If the property was listed before, keep it for reference. Skip if none. |
| `confirm_fsbo_removed` | Confirm any for-sale-by-owner listing is removed | Only if the property was previously listed by the owner. |
| `confirm_list_price` | Confirm the list price with the client | Gates the listing contract — do not prepare the contract until this is set. |
| `prepare_listing_contract` | Prepare the listing contract | In your e-signature platform. |
| `prepare_spd` | Prepare the Seller's Property Disclosure | Send to the sellers to complete. |
| `prepare_state_disclosures` | Prepare the state-required disclosures | Your state's list, captured at onboarding (may include square-footage, source-of-water, or pre-1978 lead-paint disclosures depending on your state and the home). |
| `prepare_wire_fraud` | Prepare the wire-fraud awareness disclosure | Wire-fraud acknowledgment for the sellers. |
| `review_listing_package` | Review the full listing package before sending | Confirm the contract and every disclosure are complete before anything goes to the client. |
| `send_seller_intro_email` | Send an introduction email to the sellers | Outward — confirm the draft first. A personal touch, separate from the signature request. |
| `send_listing_contract_sigs` | Send the listing contract and disclosures for signatures | Outward — confirm first. Via your e-signature platform, after your own review. |
| `confirm_listing_executed` | Confirm the listing contract is fully executed | **Milestone: Fully Executed.** File the executed copy in the deal's Drive folder. |

---

## Fully Executed → Coming Soon
Vendor scheduling and listing prep.

| task_key | title | notes / when |
|---|---|---|
| `schedule_photographer` | Schedule the photographer | Coordinate with seller availability. |
| `schedule_stager` | Schedule the stager | If applicable. |
| `schedule_carpet` | Schedule carpet cleaning or stretching | If applicable. |
| `schedule_window_cleaning` | Schedule window cleaning | If applicable. |
| `schedule_sign_lockbox` | Schedule sign and lockbox installation | Coordinate the date with the seller. |
| `confirm_lockbox_key` | Confirm the lockbox key arrangement with the seller | How the key will be placed. |
| `send_vendor_calendar` | Email the sellers the calendar of appointments | Outward — confirm first. Vendor names and dates. |
| `send_close_overview` | Email the sellers a listing-to-close overview | Outward — confirm first. Prepares them for the post-offer steps. |

---

## Coming Soon → Go Live
MLS preparation, marketing, and launch readiness.

| task_key | title | notes / when |
|---|---|---|
| `draft_listing_description` | Draft the listing description | For your MLS and marketing materials. |
| `prepare_mls_listing` | Prepare the MLS listing | Enter beds, baths, and square footage — validate against the confirmed property facts. |
| `create_marketing_materials` | Create or order marketing materials | Flyers and digital assets. |
| `schedule_social_launch` | Schedule the social-media launch posts | Coordinate with the go-live date. |
| `confirm_open_house` | Confirm the open-house date and time | |
| `confirm_showing_service` | Confirm settings on your showing service | Dates, times, and showing instructions. |
| `finalize_mls_listing` | Finalize the MLS listing | Add photos, disclosures, open-house info, and showing info before go-live. |
| `request_broker_remarks` | Add broker / agent remarks to the MLS | The agent-only remarks field. |

---

## Go Live → Active
Launch and active management.

| task_key | title | notes / when |
|---|---|---|
| `mls_go_live` | Make the MLS listing live | **Milestone: Go Live.** Confirm the listing is fully ready before you flip it live. |
| `send_live_link_sellers` | Email the sellers the live listing link | Outward — confirm first. |
| `setup_showing_service_active` | Set up showing-service instructions and contact info | |
| `file_to_drive` | File the listing documents in the deal's Drive folder | All executed docs saved and re-read to confirm. |
| `begin_showing_updates` | Begin regular showing updates to the sellers | Ongoing — marks the transition to the **Active** milestone. |

---

## Template transition
When an offer is accepted: create a fresh set of tasks from `listing-under-contract.md` (`listing_uc`), change the deal `status` from `active` to `under_contract`, and start with confirming seller acceptance.
