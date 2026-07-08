---
name: draft-email
description: Draft client and coordination emails — new-lead first contact, status updates, deadline-coordination notes, review requests, referral thank-yous — always draft-and-confirm, never send without an explicit yes. Use when the user asks to write, send, follow up on, or reply to an email to a client, lender, title company, co-op agent, or lead.
---

# Draft Email

Use this skill whenever the user asks you to write, send, follow up on, or reply to an email — to a client, a lender, a title company, a co-op agent, or a lead. You **draft**; the user **approves**; only then do you **send**. This is the outward-facing gate: an email leaves the user's name in someone else's inbox and cannot be recalled, so it is never sent without an explicit yes.

## Triggers
- "Draft an email to [person]" / "email [client] about [thing]"
- "Follow up with [lender / title / co-op agent] about [deadline]"
- "Send [client] a status update on [address]"
- "Reach out to [new lead]"
- "Ask [client] for a review" / "thank [person] for the referral"
- "Reply to [thread]"

## The send gate — non-negotiable
1. **Always show the full draft before sending**: the complete recipient list (real email addresses, not just names), the subject line, and the entire body — exactly as it will go out. No summaries, no "want me to send it?" without the text visible.
2. **Wait for an explicit yes.** "Looks good," "send it," "yes" — a clear go. Silence, a thumbs-up on something else, or "ok thanks" mid-conversation is **not** approval. If unsure, ask plainly: "Want me to send this as written, or change anything first?"
3. **Edits loop back through the gate.** If the user asks for a change, revise and show the full draft again. Approval is on the version they last saw, never a stale one.
4. **Every draft passes the compliance floor first.** Before you show any draft, silently run it through `references/compliance.md`. If a check trips, fix it in the draft (or flag it and offer a compliant version) — never show, and never send, copy that fails the floor.
5. **After sending, verify and log** (see below). Then tell the user it's sent, in one line.

If you cannot send (Gmail connection is down), say so plainly — name the connection and how to reconnect it — and never claim the email went out. Leave the draft on screen so nothing is lost.

## Personalize from the record — always read fresh
Before drafting, pull the real details; never write from memory or from what was said earlier in the session:
- **Look up the recipient in the Contacts tab** (match by name or email) for their correct name, email, and role. The Contacts tab is the source of truth for people. Use the name they actually go by (if the contact is "Nathan" but everything says "Nate," use Nate).
- **Read the deal row** (Deals tab) for the property address, side, status, key dates, and the named parties (lender, title company/closer, co-op agent, inspector). Match the facts in the email to what the sheet actually says right now.
- **Check role labels** (read the recipient's `role_labels` on their row in the Contacts tab). A recipient labeled **Counterparty** is the other side's client — you never send them marketing, status updates, review requests, or referral asks. The only contact with a counterparty is coordination the user explicitly directs, and even then it goes through the send gate. If a drafting request would send marketing-style copy to a Counterparty-labeled contact, stop and flag it.

Never put prices, commissions, earnest money, or other financial figures in a **subject line** — subjects are visible in previews and notifications. Financial specifics belong in the body, and only when the recipient is entitled to them.

## Voice
There is **one voice profile** for the user, stored in the Meta tab (`voice_notes`). Read it and write in that voice by default — it captures how this agent sounds (warmth, formality, brevity, whether they use first names, sign-off style). There is no per-recipient tone table; it's one person's voice, applied everywhere.

The user can ask you to (re)learn their voice anytime — if they do, walk through the voice-learning offer from onboarding and refresh `voice_notes` in the Meta tab (confirm before writing).

Handle two kinds of adjustment on top of the base voice:

**Plain-English overrides from the user.** If they say "make it warmer," "more formal," "shorter," "less stiff," "add a bit more personality" — apply it to this draft. If they want the change to stick, offer to update the voice note in the Meta tab (confirm before writing it).

**Context shifts you apply on your own judgment:**
- **Urgent** (a deadline is close, something needs a same-day answer): more direct and briefer. Lead with the ask, drop the warm-up, keep it to a few lines.
- **Emotional or sensitive** (a deal falling through, an inspection scaring a buyer, a delayed closing, bad news): warmer and less blunt. Acknowledge how it feels before the logistics. Never rush a worried client.

Keep the base voice underneath either shift — you're bending it, not replacing it.

## After sending — verify and log
1. **Verify it actually sent.** Confirm Gmail returned a real sent message, not just a success string you're assuming. If you can't confirm it, don't claim it.
2. **Log the sent message and thread ID** so replies can be matched back later. Write it where the deal lives:
   - If the email is tied to a specific deal, append a short timestamped line to that deal's **notes** (Deals tab) — what was sent, to whom, and the message/thread ID.
   - If it's a general lead or referral not yet attached to a deal, record it in the **Meta** tab last-run note or the deal notes once a deal exists.
   This is how "awaiting replies" tracking works — an email you sent but never logged is invisible to the next catch-up.
3. **Tell the user in one line**: "Sent to [name]." Don't re-paste the whole email back.

## Workflows

Each workflow below gives a **purpose**, a **structure**, and an **example skeleton**. Skeletons are starting shapes, not scripts — fill them from the real record and the user's voice. Bracketed `[…]` items are pulled from the Contacts tab / the Deals row, never guessed.

### 1. New-lead first contact
**Purpose:** Warm, no-pressure opening to someone who just came in — usually a referral or sphere contact. Goal is to start a relationship and offer a next step, not to sell.
**Structure:** Greet by name → how you connected / who referred them → one line of what you can help with → an easy, low-commitment next step (a quick call or coffee) → no-pressure close.
**Example skeleton:**
> Subject: Great to connect, [First name]
>
> Hi [First name],
>
> [How you were connected — e.g., "Jane mentioned you're starting to look at homes on the north side."] I'd love to help however I can, whenever the timing feels right for you.
>
> If it's useful, I'm happy to hop on a quick call this week to hear what you're looking for — no pressure at all either way.
>
> [Sign-off in the user's voice]

### 2. Client status update
**Purpose:** Keep a client oriented on where their deal stands and what's next. Reassuring, concrete, and current.
**Structure:** Greet → where the deal is right now (from the Deals row status + latest activity) → the next milestone or deadline and who's handling it → anything you need from them → warm close. Financial figures in the body only, never the subject.
**Example skeleton:**
> Subject: Update on [Address]
>
> Hi [Client first name],
>
> Quick update on [Address]: [current status in plain English — e.g., "we're through inspection and moving toward appraisal."] The next thing on the calendar is [next deadline] on [date], which [who's handling it] is taking care of.
>
> [If anything is needed: "When you get a chance, could you [specific ask]?"] Otherwise you're in good shape — I'll keep you posted as things move.
>
> [Sign-off]

### 3. Deadline-coordination note (to lender / title / co-op agent)
**Purpose:** Move a transaction detail forward with another professional — confirm a date, request a document, check status. Professional and specific.
**Structure:** Greet by name → the property and deal context in one line → the specific thing you need and by when → a clear ask → thanks. Keep it short; these readers are busy.
**Example skeleton:**
> Subject: [Address] — [what you need, e.g., "appraisal status"]
>
> Hi [Name],
>
> Checking in on [Address] ([client name], closing [date]). [The specific ask — e.g., "Are we still on track for the appraisal to be back by [deadline]? Wanted to make sure nothing's outstanding on our end."]
>
> Thanks so much — let me know if you need anything from me.
>
> [Sign-off]

*Note:* if this note is to a co-op agent, they represent the other side — keep it strictly to coordination and facts, never share your client's strategy, motivation, or numbers beyond what's already contractually shared.

### 4. Review request
**Purpose:** Ask a happy client, after closing, to leave a review. Short, genuine, easy to act on.
**Structure:** Greet → warm thanks for trusting you with [Address] → one sincere sentence on why reviews matter to a solo agent → a direct link → keep it brief.
**Example skeleton:**
> Subject: A quick favor
>
> Hi [First name],
>
> It was such a pleasure helping you with [Address] — congratulations again. If you have two minutes, a short review would mean the world to me and really helps other folks find me.
>
> Here's the link: [review link]. No worries at all if you're busy — just grateful for the chance to work with you.
>
> [Sign-off]

### 5. Referral thank-you
**Purpose:** Acknowledge someone who sent you a new client. Genuine appreciation; reassure them the person they sent is in good hands.
**Structure:** Greet → thank them by name for sending [referred person] → reassure that you'll take good care of them → warm close.
**Example skeleton:**
> Subject: Thank you for the referral
>
> Hi [First name],
>
> Thank you so much for connecting me with [Referred person] — it means a lot that you'd trust me with someone you know. I'll take great care of them.
>
> Grateful for you.
>
> [Sign-off]

## Rules recap
- Never send without the full draft shown and an explicit yes.
- Every draft passes `references/compliance.md` before it's shown.
- Read the Contacts tab + the Deals row fresh for every draft; personalize, never generic.
- No financial figures in subject lines.
- Never send marketing, updates, review asks, or referral thank-yous to a Counterparty-labeled contact.
- One voice profile from the Meta tab; apply plain-English overrides and urgent/emotional context shifts on top.
- After sending, verify it sent and log the message/thread ID to the deal notes (or Meta) for reply tracking.
- If Gmail is down, say which connection failed and how to reconnect — never claim a send that didn't happen.
