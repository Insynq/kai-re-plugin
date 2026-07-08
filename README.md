# Kai-RE

**Kai-RE** is a transaction-coordination assistant for solo real estate agents. It runs inside your coding-agent app (OpenAI Codex or Claude Code) and helps you stay on top of your active listings and under-contract deals: reading contracts, tracking deadlines, filing documents, keeping people organized, and drafting client emails — all inside **your own** Google Workspace (Gmail, Drive, Sheets, Calendar, Docs, Contacts, Maps).

You bring your Google account; Kai brings the workflows. Nothing Kai does leaves your workspace without showing you first and getting your okay.

---

## Install

### Claude Code

Paste these two lines into Claude Code:

```
/plugin marketplace add insynq/kai-re-plugin
/plugin install kai-re@kai-re-plugin
```

Then just talk to Kai — e.g. *"help me get set up"*, *"catch me up"*, or *"process the Birch Court contract."*

### OpenAI Codex CLI

```
codex plugin marketplace add insynq/kai-re-plugin
```

Then enable **kai-re** from `codex plugin`.

On first use, Codex will ask you to **review and trust** Kai's session-start hook (the one that loads the safety rules). Approve it once — that's what lets Kai load its safety spine every session. You'll be asked again only if the hook definition changes.

> **Optional hard guarantee (Codex):** because plugin hooks are trust-gated, if you want the safety rules loaded unconditionally on every session in every project, copy the plugin's `AGENTS.md` to `~/.codex/AGENTS.md`. Codex always loads that file automatically.

> **Note:** Codex's plugin/marketplace system is new and its official Plugin Directory / self-serve publishing are still rolling out. If the marketplace command or manifest format has changed, check the current docs at https://developers.openai.com/codex/plugins — the plugin content (skills, references, safety rules) is unaffected either way.

---

## What you get

Seven workflows (skills):

| Skill | What it does |
|---|---|
| **onboard** | Guided first-run setup — connects Google, builds your Kai-RE folder + organizer, loads your active deals, goes live. |
| **new-deal** | Starts tracking a new transaction — creates the deal record, its document folder, and seeds the task checklist. |
| **process-contract** | Reads a downloaded contract/counter/amendment and pulls terms, deadlines, and parties onto your calendar and organizer. |
| **catch-up** | Session sweep — brings the organizer up to date with what happened while you were away, then a short decision-first digest. |
| **briefing** | The morning briefing — the top 3 things that actually need you today. |
| **pipeline** | Your book of business — all deals, what's next on one deal, or mark a task done. |
| **draft-email** | Drafts client and coordination emails — always draft-and-confirm, never sent without your yes. |

Plus the shared knowledge every skill relies on: the **safety spine** (`AGENTS.md`) and the reference library (`references/` — sheet schema, deadline taxonomy, document families, compliance guardrails, and task templates).

---

## How Kai behaves (the safety spine)

Kai obeys four non-negotiable rules defined in [`AGENTS.md`](AGENTS.md):

1. **Two-tier autonomy** — internal captures (organizing your data) happen freely; anything outward or irreversible (sending, sharing, inviting, deleting) is shown to you first and waits for an explicit yes.
2. **Always read the source fresh** — never answer a deal fact from memory; re-read the live record.
3. **Verify after every write** — read it back before claiming it worked.
4. **Fail loudly** — never paper over a broken connection or a step that didn't run.

Plus deadline-safety rules: Kai never auto-closes a protective deadline (inspection, loan, appraisal, title objections) on its own.

A `SessionStart` hook loads `AGENTS.md` into context at the start of every session (including after resume, clear, and compaction), so the safety rules are always in force. On **Claude Code** this runs automatically; on **Codex** you approve the hook once (see install note above).

---

## Updating

The owner ships updates by pushing to this repo and bumping the `version` in the manifests.

- **Claude Code:** `/plugin marketplace update` then `/plugin update kai-re`
- **Codex:** re-run the marketplace refresh / update from `codex plugin`

---

## Requirements

- ChatGPT Plus / Codex access **or** Claude Code.
- A Google account with Gmail, Drive, Sheets, and Calendar connected in your app's connector settings.

---

_Maintained by Insynq. Source of truth is a private repository; this repo is the packaged, public plugin._
