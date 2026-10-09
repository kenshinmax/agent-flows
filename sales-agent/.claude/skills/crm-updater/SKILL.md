---
name: crm-updater
description: Turn call notes, meeting notes, or transcripts into a structured HubSpot note logged on the right contact (and deal, if one exists). Use when the user pastes interaction notes, says "log this call", "add a note to HubSpot", or runs /crm-updater.
---

# CRM Updater — Log Notes to HubSpot

Turn raw interaction notes into a clean HubSpot note on the correct contact record. Never write to HubSpot without the user's explicit approval.

## Step 1: Identify the contact
- Get the contact's name or email from the user's notes. If neither is present, ask.
- Search HubSpot contacts for a match.
  - **One match:** confirm name + email with the user in the preview (Step 3).
  - **Several matches:** list them (name, email, child/athlete name if present) and ask which one.
  - **No match:** stop and tell the user. Do not create a new contact unless they ask.
- Also look up any open deals associated with the contact so the note can be attached to them too.

## Step 2: Structure the note
Use only facts stated in the user's notes. Do not invent or infer details; if something important is unclear (e.g., which camp session), list it under **Open Questions** rather than guessing.

Format the note body like this (omit empty sections):

```
[Interaction type] — [Date]

Summary:
1–2 sentences on what happened.

Athlete Details:
- Name / age / grade / skill level / position (only what was mentioned)

Interests & Needs:
- Programs, sessions, clinics, or skills training discussed

Concerns / Objections:
- e.g., price, schedule, transportation

Next Steps:
- [Action] — [Owner] — [Due date if mentioned]

Open Questions:
- Anything unclear that needs follow-up

Upsell Signals:
- Interest in additional programs, siblings, teammates, skills training
```

- **Interaction type:** Call, Meeting, Email, In-person, or Other.
- **Date:** use the date in the notes; if none is given, use today's date and say so in the preview.

## Step 3: Preview and get approval
Show the user:
1. The matched contact (name + email) and any deal(s) the note will be attached to
2. The full formatted note
3. Any assumptions you made (e.g., defaulted date)

Then ask: **"Log this note to HubSpot?"** Do not proceed until the user clearly says yes. Apply any edits they request and re-preview.

## Step 4: Create the note in HubSpot
After approval, create a HubSpot **NOTE** object:
- `hs_note_body`: the formatted note (HubSpot accepts basic HTML; use `<br>` for line breaks and `<b>` for section headers)
- `hs_timestamp`: the interaction date (ISO 8601)
- Associate it with the contact, and with each deal identified in Step 1

If unsure about the exact tool parameters, check the HubSpot connector's tool guidance before calling.

## Step 5: Confirm
Report back: the contact name, which records the note was attached to, and the note's ID. If the write fails because of permissions (e.g., `REQUIRES_REAUTHORIZATION`), tell the user to disconnect and reconnect the HubSpot connector in Settings → Connectors and approve write access, then offer to retry.

## Guardrails
- Never log a note without explicit approval in chat.
- Never modify or delete existing notes or contact properties — this skill only creates new notes.
- Never put information in the note that wasn't in the user's input or the HubSpot record.
- Process one contact per run. If the notes cover several people, ask which to log first.
