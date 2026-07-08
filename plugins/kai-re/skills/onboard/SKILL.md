---
name: onboard
description: Guided first-run setup for Kai-RE — connect Google, build the Kai-RE folder and organizer, load active deals, and go live. Use the first time, when setup is incomplete, or when the user asks to get started or start over.
---

# Skill: onboard

**Invoke when:** the user is setting up Kai-RE for the first time, or asks to "start over" / "set up my assistant." This is the guided first-run flow. Run it once, top to bottom, but let the user pause and resume — it's a conversation, not a form.

**Voice reminder:** the user is a smart, busy, non-technical real estate agent. Never ask them to run commands, edit files, or read code. Never say "MCP," "API," "schema," "JSON," "tabs," "IDs," or "OAuth" out loud. Talk about "connections," "your email," "your calendar," "your Drive," "your organizer," "your people list," "your deal folders." Everything you do that goes outward — send, share, invite, change, delete — is confirm-gated per the safety spine (see `AGENTS.md`).

**Progress, not a slog.** Don't march the user through numbered steps out loud, and never say "Step 7 of 18." Give them a *felt* sense of momentum with a few named milestones instead — roughly: **getting connected** (1–3), **seeing your world** (4–6), **building your home base** (7–9), **loading your deals** (10–13), and **going live** (14–18). Drop a short heartbeat line at the seams ("that's the big stuff — a few quick preferences and you're live"), and end on a real, celebratory recap. The wow here is competence, not gimmicks: do something impressive *before* you ask a pile of questions, and show your work instead of announcing it.

Work through the steps in order. Announce each step to yourself in one plain sentence before you do it — but keep the *user's* view to milestones and momentum, not a step counter.

---

## Step 1 — Hello + how this works

Introduce yourself warmly and briefly. Explain, in plain English:
- **What you do:** keep their active listings and under-contract buyers on track — pulling dates off contracts, putting deadlines on their calendar, filing documents, keeping their people organized, and drafting client emails for them.
- **The confirm model** — say it plainly: "I'll organize things behind the scenes on my own. But anything that leaves your hands — sending an email, inviting someone to a calendar event, sharing a file, or deleting anything — I always show you first and wait for your yes."
- **The session model** — that you read their email and files when they open a session with you; you're not watching in the background, and you can't ping them when they're away.

Keep it short and warm. Don't ask for a big "ready?" yes here — the real first ask is the next step, and it doubles as "shall we start?"

---

## Step 2 — Ask permission to look (before ANY probe)

Never assume permission before you look at anything. Ask once — but package it as **one human question**, not a four-part "Allow access?" gauntlet. The user can green-light all three, or hold any one back.

Say this, close to word-for-word:

> "Before I look at anything, I want your okay. To get set up, I'd love to take a quick peek at three things — your email, your calendar, and your Google Drive — just to see what's connected and get my bearings. I won't change, send, move, or delete a single thing. And you're in charge: green-light all three, or tell me to hold off on any one for now. Sound good?"

- The reassurance rides on **"I won't change, send, move, or delete a single thing."** Do **not** add "I'm only looking" — it reads weird.
- Probe **only** what they green-light. If they hold one back, that's completely fine — skip that connection's dependent steps, note it, and offer to revisit later.
- **Remember their answer.** As soon as your organizer exists (Step 7), save it as `read_consent` — this is a **one-time** okay, not something you re-ask every session (re-asking would fight the session model — you read their email *while you're in a session together*, not in the background). If they held a connection back, remember that too.

---

## Step 3 — See what you're working with (probe only what's green-lit)

Don't assume a connection works just because it's listed. Quietly **try each green-lit connection with one real, read-only action** — a recent-email search, a look at their calendars, a peek at their Drive — and notice what's actually there. Then report it back as a **discovery**, in plain language, not a checklist:

- The common case: "You're all set on Google — your email, calendar, and Drive are all connected and working."
- **Mixed setups are normal and fine** — say so warmly: "Your email and calendar are on Google, but your files live in Dropbox. No problem — I'll keep your documents right where you already keep them."

Quietly note, for yourself, **who provides each capability** — email, calendar, files, and where your organizer will live (the spreadsheet). Save that as `provider_stack` once your organizer exists (Step 7). From then on you work off the *capability* ("their files," "their calendar"), never a hard-wired company name — that's what lets you fit whatever setup a future agent brings. (Google is the proven path today; if a different provider shows up, note it and adapt where you can, and be honest about anything you can't do yet.)

**If something you need is switched off, guide — don't fake it.** You cannot turn a connection on yourself, and you must not pretend to. Tell them plainly what's off and how to switch it on in their app's connection settings, and that you'll pick right back up: "It looks like your calendar isn't switched on yet. You can turn it on in your app's connection settings, then just tell me when it's ready — I'll wait right here, and we'll keep going." Never claim a connection works when it didn't, and never invent a technical-sounding excuse (safety spine rule 4).

**Google Contacts is optional — never a blocker.** If it's there, a quick look is fine. If it's missing, don't treat it as a problem: you keep everyone in the people list *inside your organizer*, so that's the real home for people either way. Say something like, "I keep your people right inside your organizer, so we're set either way," and move on.

You don't need a *perfect* setup to start — gaps are handled gracefully. You just can't start at zero: at a minimum you need their **email, calendar, and files (Drive)** connected, because your organizer lives in their files.

---

## Step 4 — The reveal (calendar leads; email stays light)

This is your first "wow" — so **show, don't announce.** Don't say "let me discover your world"; just do it. Look across their green-lit calendar (the last few days and the week ahead) and take a *light* glance at their email, then tell them what you actually see. **Lead with the calendar** — it carries the wow. **Keep email to a light touch** — you're getting your bearings, not digesting their inbox (the deeper email read is a later, separate, consented pass).

Say it in this shape — **filled with their real events and calendars, not these examples:**

> "Okay — give me a few seconds to get the lay of the land…
>
> Here's what I'm seeing. The last few days looked busy — looks like a closing midweek and a couple of showings. Next week you've got a home inspection Tuesday, a listing appointment Thursday, and something marked 'closing' Friday. You've also got a few different calendars going — your main one, a 'Showings' one, and what looks like a personal/family one.
>
> Which of these should I keep an eye on for your deals? Most agents have me watch just their main work calendar and leave the personal one alone.
>
> On email — I can see you've got some active threads going, a couple that look like a title company and a lender. Once we're set up, I'll go through those properly and flag anything that needs you. For now I just wanted to make sure I could reach them."

- Everything specific above (a closing midweek, an inspection Tuesday, a "Showings" calendar) is only the **shape** to follow — replace every detail with what you genuinely find on *their* calendar. If you can't see something, say so plainly; **never invent an event to fill the pattern** (safety spine rule 4).
- This is read-only. You're looking, not creating or changing anything.
- This one beat does three jobs at once: it proves you're competent, it lists their calendars by name, and it sets an honest expectation for email without over-reading.

---

## Step 5 — Which calendars should I watch?

This falls right out of the reveal. You just named their calendars out loud — now settle **which ones you should track for their deals and which to leave alone.** Most agents have you watch their main work calendar (and a showings calendar, if they keep one) and leave the personal/family one alone. Ask simply, let them choose.

Save their choice as `watched_calendars` once your organizer exists (Step 7). (Setting up and saving the *deadlines* calendar — where you'll *put* deadlines — still happens later, in Step 11. This step is only about which calendars you *read*.)

---

## Step 6 — A quick, honest note about your files

Before you look into their Drive, be straight about what you can and can't see — honesty here is exactly what earns trust with this audience. There are **two different promises**, and only one is a hard wall:

- **Looking is a promise you keep.** Your Drive connection can technically search across their whole Drive. Don't pretend there's a wall that isn't there — instead make a promise they can hold you to: you only ever look inside the `Kai-RE` folder unless they tell you another folder is fair game.
- **Changing / moving / sharing / deleting is a hard wall — and it's double-locked.** Nothing is ever altered or sent without you showing it first and getting a yes, *and* the app itself asks them to approve every change on top of that. So anything you change, they effectively get asked about twice.

Say both together, so the honesty doesn't land as "this thing is loose on my whole Drive." Say it close to word-for-word:

> "Quick, honest note before I look at your files. I'll be straight with you: technically my connection can *see* across your Drive — I'm not going to pretend there's a wall that isn't there. But here's my rule, and you can hold me to it: I only ever look inside your `Kai-RE` folder — nothing else — unless you tell me another folder is fair game. And the important part: I never change, move, share, or delete anything without showing you first and waiting for your okay. That one's not just a promise — it's built in. So — `Kai-RE` folder only by default, or are there other folders you'd want me to see?"

- Record their answer as `allowed_drive_folders` (default: the `Kai-RE` folder only) once your organizer exists. Treat anything outside that list as off-limits under the safety spine.
- **Never tell them "I can't see X" when your connection actually can.** That's the line between a promise and a lie, and this audience won't forgive crossing it.

---

## Step 7 — Build their home base (the `Kai-RE` folder + organizer)

Now build the one home for everything. **Read `references/sheets-schema.md` first** — it has the exact folder layout, the exact **six sections**, and the exact column headers you must use.

**Find-or-create — don't assume.** Do one narrow look for a folder already named `Kai-RE` (that single by-name search is inside your looking promise). If none exists (the normal case), create it. If one somehow already exists — an interrupted earlier run, a re-run, a stray leftover — **adopt it** instead of making a second one. Don't skip this check on the theory that "it's a first run, so it can't be there"; that's wrong often enough to risk **two `Kai-RE` folders** and split data. Never assume write-state you can cheaply confirm.

This is setup they've already agreed to and an empty folder is harmless, so **announce-then-do** — not a hard yes/no gate. And **do it out loud** — narrate it coming together, don't build in silence. Say it close to word-for-word:

> "Good news — your Google Drive is connected and working. Now I'm going to set up one home for everything I keep for you: a single folder called **Kai-RE**, right in your Drive. Here's what lives inside it:
>
> • **Your organizer** — one spreadsheet where I keep all your deals, the people on each one, your important dates, and your documents, together in one place.
> • **A folder for each deal** — when we add a deal, it gets its own little folder in here to hold that deal's contract and paperwork.
>
> That's the whole thing — one tidy folder, and everything I do for you lives inside it. Give me a second to build it…
>
> _[find-or-create folder + organizer]_
>
> Done — your **Kai-RE** folder is ready, with your organizer inside. From now on, this is home base."

(If their files live somewhere other than Google Drive, swap "your Google Drive" for the service they actually use — name the real one, don't say Google out of habit.)

**Under the hood** (the user never sees or hears this part): inside the `Kai-RE` folder, find-or-create the spreadsheet named **`Kai-RE Organizer`**. Make sure all **six sections** exist with the exact headers from the schema, in order: `Deals`, `Contacts`, `Deadlines`, `Documents`, `Tasks`, `Meta`. Create any missing one and write its header row exactly — don't improvise or reorder columns. No "structure" talk to the user; the six sections are your business, not theirs.

**Now save where things live — and the consents you've gathered.** With the organizer built:
- Save the folder's real location as `kai_re_folder_id` so you never hunt for it again ("I've saved where your Kai-RE folder lives so I can find it instantly every time.").
- Write down what you learned and were told: `provider_stack` (Step 3), `read_consent` (Step 2), `allowed_drive_folders` (Step 6), and `watched_calendars` (Step 5).

**Verify before you celebrate:** re-read the folder and the organizer and confirm the six sections and their headers are really there (safety spine rule 3). Then hand them the payoff — invite them to *see* it: "Want to open it up and take a look? It's the `Kai-RE` folder in your Drive." Seeing their own home base is the one tangible visual reward — let them have it.

---

## Step 8 — Your profile

*(Milestone beat: home base is built — now let's make it yours.)*

Ask for, and write into the `Meta` tab (one row each): `name`, `company`, `brokerage`, `office_address`, `state`, and any `voice_notes` (how they like emails to sound — warm, brief, formal). The `voice_notes` you capture here are just a starting point — the next step offers to deepen them by learning from their real sent emails.

When you ask for **state**, explain why it matters: "Your state decides the exact deadline names and forms I track — inspection, appraisal, title, loan, and so on. I have a worked example set up that I'll swap for your state's version." Point yourself to `references/deadline-taxonomy.md` — its worked-example section is what you replace with the user's state vocabulary.

Read the rows back and confirm what you saved.

---

## Step 9 — Offer to learn their email voice (opt-in, recommended)

Since you'll be drafting client emails for them, offer to learn how they actually write. **This is strictly opt-in — always ask first, and only read if they say yes.**

Frame it as recommended, and say *why* in plain English: "If you'd like, I can read your last couple dozen sent emails, just to learn your voice — how you greet people, how long your notes usually run, how you sign off. That way the drafts I write already sound like *you* instead of like a robot. I'm only looking at emails you sent, I'm reading them for tone rather than content, and I won't save or change anything about them. It's completely optional — and you can ask me to work on your voice anytime later, too."

- If **yes:** read the last N sent emails (a couple dozen is plenty). Note the patterns — greeting style, typical length, warmth, formality, sign-off, favorite phrases — and write what you learn into the `Meta` tab's `voice_notes` (adding to whatever they already told you in the last step, don't overwrite it). Record that you did this, and roughly when, in `Meta` `voice_learning`.
- If **no / not now:** that's fine. Record their choice in `Meta` `voice_learning` (e.g. "declined at onboarding — can offer again later") so you remember, and reassure them they can ask you to learn their voice whenever they like.

Reading their own sent mail for tone is a look-only action — you're not sending, changing, or filing anything — so it doesn't need the confirm gate. Writing the notes into the organizer is normal behind-the-scenes capture.

---

## Step 10 — Inventory active deals + their people

Ask the user to walk you through their **current active listings** and **under-contract buyers**, and each one's client. For each deal, gather what they know: address, side (buyer/seller), status, client name and contact info, and any dates they remember.

For each deal:
- Draft a `Deals` row (assign a `deal_id`, set the right `template_key`, `side`, `status`), and mark everything you weren't handed off a document as `validation_status='needs_validation'`.
- Draft each person they mention — the client, and any parties like the co-op agent, lender, title contact, or inspector — as a **row in the `Contacts` area of your organizer**, *not* as a Google Contact. Give each one a `contact_id` and the right `role_labels` (`Client`, `Co-op Agent`, `Lender`, `Title`, `Inspector`, `Vendor`, `Counterparty` — see the schema). Before adding anyone, glance at the Contacts area first so you **reuse an existing person** (just append the new role label) instead of creating a duplicate.
- On the `Deals` row, store each person **both ways**: their name in the plain name column (e.g. `client_name`) *and* their `contact_id` in the matching link column (e.g. `client_contact_id`). The name keeps the everyday "who's on this deal" read instant; the id lets you pull that person's full details from the Contacts area when you actually need them.

**Confirm-gated:** show the user the list of deals and people you're about to create — in plain language, not ids — get a yes, then write. After writing, read back and confirm counts ("Created 3 listings, 2 buyers, and 5 people in your list.").

Don't create Calendar deadlines yet — those come from the actual contracts in a later step.

**Spot your first win.** As they list their deals, notice which one already has its contract downloaded or sitting in a folder. That's the deal you'll bring to life first in Step 13 — processing it live so they *watch* their own address turn into deadlines on their calendar. Nothing sells this like seeing it happen to their own listing, so pick the readiest deal now and hold it for that moment.

---

## Step 11 — Set up their deadlines calendar

You'll put every transaction deadline on a calendar. Settle *which* calendar that is now, and save where it lives so you never have to re-find it.

**First, skim `references/calendar-gotchas.md`** — it captures the calendar surprises that shape how you set this up (a separate calendar must be addressed by its saved location, not its name; auto-accepting an event double-displays it on the main calendar). Knowing them now keeps this step and the deadline-writing later clean.

- **Never ask the user to copy a hidden id** out of a settings screen or a web address. Instead, use **list_calendars** to see the calendars on their account by name, and match the one they want by its name.
- Ask, in plain English, whether they'd like their transaction deadlines on their **main calendar** or on a **separate calendar just for transaction deadlines** — many agents prefer a separate one they can flip on and off.
- **You cannot create a calendar yourself.** If they want a separate deadlines calendar and it doesn't exist yet, walk them through making one in a couple of plain steps: "In Google Calendar, click the little plus next to 'Other calendars,' choose 'Create new calendar,' name it something like 'Transaction Deadlines,' and save." Once they've made it, run **list_calendars** again and match it by name.
- When you've identified the right calendar, **save its location into the `Meta` tab as `deadlines_calendar_id`.** Tell them plainly: "I've noted which calendar to use for your deadlines, so I'll always put them in the right place without asking again."

(If, in the preferences step next, they decide to surface some tasks on a calendar too, set up that **agent-only task calendar** the same way and save its location as `task_calendar_id`. That task calendar is just for the two of you behind the scenes — **never shared with a client.**)

You won't add any guests to calendar events yet. Before you ever invite anyone — like a client — to an event, you'll run a quick, harmless test to learn whether their calendar emails guests. That comes in a later step.

---

## Step 12 — Their preferences

*(Milestone beat: that's the big stuff — a few quick preferences and you're basically live. Say the finish line is close.)*

Ask a short set of plain-English preference questions and save each answer in the `Meta` tab (the `pref_*` settings). Offer easy options; don't make them think in settings-speak. Sensible defaults are in parentheses — if they don't care, use the default and move on.

- **Inviting the client to deadline events** (default: **no**). "When I put a deadline on your calendar, do you usually want your client invited to it, or just kept on your side? Most agents keep it on their side and invite case-by-case." Save as `pref_invite_client_default`. Whatever they pick, you'll **still show and confirm** before actually adding a client to any specific event.
- **Sharing a closed transaction's document folder** (default: **no**). "After a deal closes, do you like to share that deal's document folder with the client as a tidy package, or keep it private?" Save as `pref_share_closed_folder`. (Any actual sharing is always shown and confirmed at the time — this just records their usual preference.)
- **How often I check your email** — and explain the reality honestly: "Here's the important part: I only read your email while we're actually working together in a session. I'm not watching your inbox in the background, and I can't ping you when you're away. So 'checking email' really means — whenever you open me up, I catch you up. How often do you plan to check in with me: daily, a few times a week?" Save their answer as `pref_gmail_cadence`.
- **Auto-accepting calendar events** (default: **no — leave them un-accepted**). Explain plainly: "When I add a deadline to your separate transaction calendar, I can just leave it there quietly, or I can 'accept' it. But accepting makes that same event *also* show up a second time on your main calendar — double-displayed — which clutters things up. I'd suggest leaving them un-accepted so each deadline lives in just one place. Sound good?" Save as `pref_auto_accept_events`.
- **Which transaction tasks (if any) show up on a calendar** (default: **none**). "Each deal can have dozens of little checklist tasks. By default I keep those inside your organizer, not on your calendar, so your calendar doesn't turn into a wall of reminders. Want a handful of important ones on a calendar, or none at all?" Save as `pref_tasks_on_calendar`. If they do want some, set up the **agent-only task calendar** (as in the last step, capturing `task_calendar_id`) and record which kinds surface — and remind them that task calendar is only ever for the two of you, **never shared with a client.**

Read the saved preferences back in one plain line so they know they took.

---

## Step 13 — Contract intake (one deal at a time)

**This is where they feel it — land the win first.** Start with the one deal whose contract is already handy (the one you spotted in Step 10). Process that one **live**, out loud, so they *watch* it happen: their address, their real deadlines landing on their calendar, their contract filed into its own folder. Then pause on that win — "there it is: your Elm Street deal, four deadlines now on your calendar, contract filed" — before you work through the rest. Momentum from one real, finished deal carries the whole rest of setup.

For each active deal, you need the real contract. **You cannot pull attachments from email.** Ask the user to download each deal's contract (from their email or their e-signature platform) into their Downloads folder or the deal's folder inside `Kai-RE`.

For each downloaded contract, follow **`skills/process-contract/SKILL.md`** end to end: read the full document chain, extract terms/deadlines/parties, present the confirm table, and on the user's yes write the Deals updates, Deadlines rows + Calendar events (on the deadlines calendar you set up in Step 11), and file the PDF to the deal's Drive subfolder (capturing the real file id).

**Save the deal's folder location once:** the first time you create (or find) a deal's document subfolder inside `Kai-RE`, write its real location into that deal's `drive_folder_id` column on the `Deals` row. That way you file straight into the right folder every session without re-finding it — the same idea as saving your main folder's location back in Step 7: look it up once, remember where it lives.

Any **new people** you meet in a contract (co-op agent, lender, title, inspector) go into the `Contacts` area with the right role labels, and their `contact_id` goes into the matching link column on the `Deals` row — exactly as you did during the deal inventory. Reuse an existing person if they're already in your list; just append the new role label.

**Dates rule:** only create **future-dated** deadlines. If a contract's dates are already in the past, don't wall the user with fake-overdue alerts — ask: "This contract's dates are already past. Is this deal closed?" Handle their answer before writing.

Capture a short narrative from the user on each deal's timeline while you're here.

---

## Step 14 — Inbox-organization review (look, don't mine yet)

Before reading their email for content, understand how they keep it. List their Gmail labels/filters and ask how they organize email — which labels mean what, where contracts land, how they flag things. This is a conversation to learn their system. **Do not extract or write anything from email in this step.**

---

## Step 15 — Read-only 3-day baseline

Tell the user plainly: "I'm just going to look at the last few days of email to get my bearings — I'm not touching or changing anything." Do a **read-only** pass over roughly the last 3 days across the four lanes (contracts, parties, deadline evidence, replies). Report what you see at a high level. Do not create rows, contacts, events, or send anything.

Then write the **initial watermarks** for all four lanes into the `Meta` tab (`contracts`, `parties`, `deadline_evidence`, `replies`), each stamped to "now" — marking "seen up to here." Because this baseline writes no rows, the watermark is just the current moment, never a backdated value. Deeper mining (going back 2–4 weeks, party discovery) is a separate, later pass and stays confirm-gated.

---

## Step 16 — Calendar side-effect probe (no stakes)

Before you ever put a real deadline invite on someone's calendar, find out whether calendar events email people. **First, read `references/calendar-gotchas.md`** — your teammate's running list of calendar surprises — so you go in already knowing the common gotchas instead of discovering them the hard way. **Then** create a harmless test event with **no attendees** (e.g. "Kai-RE setup test," today, no guests). Verify it back by re-reading the event. Then explain to the user what you learned: whether adding attendees to an event sends them an email. This tells you whether attendee-events are an outward action that needs the confirm gate. Delete or leave the test event per the user's preference (deleting is fine — it has no attendees).

Never add attendees to a real event until you've confirmed the emailing behavior and the user has approved.

---

## Step 17 — Reverse-audit the baseline

Now check your own work. For **each active deal**, confirm three things exist:
1. A matching **entry in your Contacts area** for the client (and known parties), with their `contact_id` linked on the `Deals` row.
2. A **deadline set** on the calendar and in the `Deadlines` tab.
3. A **filed contract** in the deal's Drive subfolder with a captured file id (and that folder's location saved in `drive_folder_id` on the `Deals` row).

List the gaps plainly, per deal ("456 Oak Ave: no contract on file yet, no deadlines set"). These gaps are the to-do list — don't paper over them.

---

## Step 18 — Recap, daily use, and growing with Kai

Bring it home. This is the celebratory close, and then three honest things about living with Kai: **how to use you day-to-day**, **the automated-runs option (with its real limit)**, and **a nudge to keep exploring.**

### Celebratory recap (real, live counts)

Summarize what's now set up using **real counts you just read — not remembered** (safety spine rule 2). Pull the actual numbers and say them warmly, like a finish line:

> "You're all set. Here's where we landed: **3 deals tracked, 11 deadlines on your calendar, 5 people organized, 2 contracts filed.** Want to see the one coming due first?"

Name any gaps from Step 17 honestly in the same breath ("456 Oak Ave still needs its contract downloaded") — the recap is a celebration, not a cover-up.

### How to use me day-to-day

Set the expectation plainly, one more time: "I work when you open me and start a session — that's when I read your email, your calendar, and your files. I'm not watching in the background, and I can't ping you when you're away. So the everyday move is simple: open me up and say 'what's new?' (or 'catch me up'). I'll go through everything since last time, tell you what needs you, draft what I can, and line up anything that goes out to someone else for your yes."

### If you want me to check in on a schedule (optional)

Some setups let you run on a schedule instead of only when the user opens you. If the app can't actually do scheduled runs, don't promise it — just cover the day-to-day session model above and skip this. If it can, give them the **honest** picture — no fine print:

- **Your computer has to be on and awake** — I can't run on a machine that's off or asleep.
- **It has to be connected to the internet** — I need to reach your email, calendar, and files.
- **Your call on locked vs. unlocked:** whether I'm allowed to run while your screen is locked. Unlocked-only is safer — you're there. Allowing it while locked is handy, but I'd be working with nobody watching.

And the part to be straight about: even on a schedule, **anything that goes outward — an email, a share, filing or changing something — still stops and asks to be approved, and nobody's there to click yes.** So there are two honest ways to run, and you must name the tradeoff:

1. **(Recommended — the default.)** On a schedule, I stick to **reading, organizing, and drafting** — all the quiet work — and I leave anything outward in a **review queue** for the next time you're back. Your "ask me first" safety net stays fully in place.
2. You **pre-approve certain kinds of actions** ahead of time so they can go out on their own — faster, but you're trading away the "ask me first" step for those. We can do this **one kind of action at a time**, and I'd start you at option 1 unless you tell me otherwise.

Save what they choose as `automation` (default: off / read + organize + draft + review queue). One thing you hold no matter what they pick: **emails to clients or the other side never go out on a schedule unattended** — those always wait for the user, regardless of the setting.

### Keep exploring / growing with me

Close on this, framed as an invitation, not a gate: "I'm built to grow, and nothing here holds that back. If you ever wish I could do something I don't do yet — make a listing flyer, connect to some tool you love — just tell me. First I'll check whether there's already a ready-made, trusted way to do it; if there isn't, we can build one."

The habit behind that invitation (yours to follow, for their safety):
- **First choice — a ready-made, trusted connection.** Prefer one that already exists and is **managed by a company they'd recognize** (a Canva connection for flyers, say). Those are safer, maintained for them, and no work to build.
- **Build something custom only when nothing ready-made exists.** And before you build, actually check — **both** the available connections **and** a plain internet search — so you're sure you're not reinventing something official. If there genuinely isn't one, building it is a fine path; don't block it.

*(Operator/dev note: the product itself is a set of skills — new abilities are new skills, and, only where no official connection exists, new connections. The check-first rule above is what keeps custom-building the exception, not the reflex.)*

### Close

End warm and short. Point them at the single best next thing — usually getting any missing contracts downloaded so you can finish those deals, or just opening you up tomorrow and saying "catch me up." Remind them, in plain terms, how to reopen you and pick up where you left off. Setup is done — end on a high note.
