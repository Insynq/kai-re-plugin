# Reference: Calendar Gotchas

Google Calendar has a few behaviors that bite you *after* the fact if you don't know them going in. They were each learned the hard way, so this file captures them once — read it before you create, invite to, or write a deadline onto any calendar, so a future session doesn't rediscover them mid-task.

None of this changes the safety spine in `AGENTS.md`: anything that reaches another person (inviting an attendee, sharing) is an **outward action** and always needs the user's explicit yes first. This file just explains *why* those gates matter on the calendar and how to keep transaction events clean.

---

## 1. Adding attendees emails them — and can double-display the event

**Attendees get emailed.** The moment you add an attendee to an event, Google emails that person an invitation. That makes adding an attendee an **outward action**: show the exact event (date, time, and who you'd invite) and get an explicit yes before you do it. Never add the client, the co-op agent, or anyone else to an event on your own initiative.

**Auto-accept / an auto-attendee copy can double-display.** If events are set to auto-accept, or the calendar owner is themselves added as an attendee, the same event can show up **twice** — once on the primary calendar and once on the transaction (deadlines) calendar. That clutters the user's day with duplicates.

**Default: keep transaction events un-accepted.** Leave deadline and transaction events living *only* on the transaction calendar — don't auto-accept them onto the primary calendar. This is the `pref_auto_accept_events = no` default in the `Meta` tab. Only invite the client to a specific event when the user opts in for that event (the `pref_invite_client_default` preference records their standing choice, but you still show and confirm each real invite).

## 2. To write to a secondary calendar, use its ID — not its name

A transaction/secondary calendar is addressed by its **calendar ID**, not by the name a person sees ("Kai-RE Deadlines"). If you try to write using the display name, the event won't land where you expect.

- Resolve name → ID with `list_calendars`, then use that ID to create or update events.
- **The ID must be `primary` or an email-like address containing `@`** (a secondary calendar's ID ends in `@group.calendar.google.com`). Some tools hand back an **opaque/base64-looking token** for a calendar — passing that directly to a read/write call fails with "calendar_id must be 'primary' or an email-like Google Calendar ID containing '@'". If the value you have has no `@`, it isn't a usable calendar ID: get the email-like form, or fall back to `primary`. Store the email-like ID (the one with `@`), never the encoded token.
- Store the resolved ID in the `Meta` tab (`deadlines_calendar_id`, and `task_calendar_id` if the user surfaces tasks) so you don't re-discover it every session.
- **Kai cannot create a calendar.** The user creates the transaction calendar in Google Calendar themselves; Kai's job is to capture its ID once and reuse it.

## 3. Deadline "hold" events can auto-attach a Google Meet link

When you put a deadline on the calendar as a **hold**, Google may automatically attach a Google Meet video link to it. A deadline hold is a marker on the calendar, **not a meeting** — nobody is dialing in. Don't leave a Meet link on it: avoid attaching one, and remove it if it appears, so the event reads as the deadline it is.

---

These behaviors were confirmed during onboarding's calendar side-effect probe — see `skills/onboard.md` (the no-stakes test event that checks whether attendee-events email people before any real invite goes out).
