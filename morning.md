---
name: morning
description: "Morning brief for Daemon Dias. Trigger when user types /morning or asks for 'morning brief', 'start my day', or 'daily brief'."
---

# /morning ☀️

Daemon's daily morning command. Runs the hub refresh and first-call prep simultaneously, then merges into one scannable brief.

---

## Step 1 — Run hub + calendar in parallel

Run these simultaneously:

### A — Hub refresh (full /hub flow)
Execute all steps from the /hub skill:
- Pull account data from Google Sheets
- Calculate upside accounts
- Find "In Your Court" emails
- Pull onboarding pipeline
- Pull today's calendar
- Update the V3 dashboard with targeted edits
- Commit and push

### B — Identify today's first Pro call
Use `gcal_list_events` for today. Find the first event that looks like a Pro/account call (not internal — look for external attendees or Pro names from the accounts list). Extract:
- Pro name and company
- Time and duration
- Any notes in the invite

Run A and B at the same time. Do not wait for one before starting the other.

---

## Step 2 — Run prep for first call

Once the first Pro call is identified from Step 1B, immediately run the full /prep flow for that contact:
- Query Granola for past meeting notes
- Search Gmail for recent threads
- Search Slack for internal mentions
- Generate the prep brief

If no Pro call is found today, skip this step and note "No Pro calls scheduled today."

---

## Step 3 — Output the combined morning brief

```
☀️ GOOD MORNING, DAEMON — [Day, Date]

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
📊 REVENUE SNAPSHOT
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
Revenue to Date:  $[rev]  ([rev_pct]% of goal)
P2G Projection:   $[p2g]  ([p2g_pct]% of goal)
QTR Goal:         $[goal]
Pace:             [↑ ahead / ↓ behind] by $[delta] ([delta_pct]%)

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
🔺 TOP UPSIDE (Top 3)
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
1. [Account] — $[gap] gap · [pacing]% pacing
2. [Account] — $[gap] gap · [pacing]% pacing
3. [Account] — $[gap] gap · [pacing]% pacing
→ [X] more accounts with upside. Full list in hub.

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
📥 IN YOUR COURT ([X] items)
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
[List each — account name, days waiting, topic]

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
🚀 ONBOARDING ([X] active)
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
Introduction ([n]):      [names]
Creating Profile ([n]):  [names]
Walkthrough ([n]):       [names]

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
📅 TODAY'S SCHEDULE ([X] events)
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
[Time] — [Event name]
[Time] — [Event name]
...

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
📋 FIRST CALL PREP — [Name] | [Company] ([Time])
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
LAST INTERACTION
• [Most recent Granola/email/Slack note — date + key topic]
• [Any open items]

TALKING POINTS
1. [Opener based on last interaction]
2. [Key follow-up item]
3. [Growth or coaching angle]

WATCH-OUTS
• [Any friction, risk, or sensitive context]

SUGGESTED CLOSE
• [Recommended next step or ask]

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
✅ Hub updated · [timestamp]
📂 http://localhost:4400/hub/
```

---

## Error Handling
- If Google Sheets is unavailable: show last known revenue data from hub-state.json and note it's cached
- If no Pro call today: skip prep section, show "No Pro calls scheduled today ✓"
- If prep contact is ambiguous: use best match from calendar invite and note assumption
- If calendar is empty: show "No events found — check your calendar"
