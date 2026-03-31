# Claude Skills — Thumbtack AM Toolkit

Claude Code skills for Thumbtack Account Managers. Install any skill in seconds and run it as a slash command in any Claude Code session.

---

## `/score` — Pro Profile Scorer

Scores a Thumbtack Pro's customer-facing profile on a **100-point rubric**. Returns a full breakdown, flags what's missing, and gives the top 3 highest-impact recommendations to improve the profile.

**Built for:** Account Managers who want to identify coaching opportunities before a call, or benchmark a Pro's profile health at a glance.

**Requires:** Claude Code with the Chrome browser tool enabled.

### What it scores

| Criterion | Points |
|-----------|--------|
| Profile photo | /2 |
| Background check | /2 |
| Business name | /2 |
| Year founded | /2 |
| Number of employees | /2 |
| Payment methods | /5 |
| Social media | /5 |
| Introduction (scaled by length) | /20 |
| Reviews (scaled by count) | /15 |
| Photos & videos (scaled by count) | /15 |
| Featured project with pricing | /10 |
| FAQs answered (scaled by count) | /10 |
| Professional license *(licensable categories only)* | /10 |

### Sample output

```
🎯 PROFILE SCORE — Seamless Exterior Solutions
https://thumbtack.com/...
Category: Licensable

━━━━━━━━━━━━━━━━━━━━━━━━━━━
OVERALL SCORE: 74/100
━━━━━━━━━━━━━━━━━━━━━━━━━━━

BREAKDOWN:
✅ Profile Photo          2/2
✅ Background Check       2/2
✅ Business Name          2/2
✅ Year Founded           2/2
✅ Employees              2/2
✅ Payment Methods        5/5
❌ Social Media           0/5
⚠️ Introduction          10/20
✅ Reviews               15/15
✅ Photos/Videos         15/15
✅ Featured Project      10/10
⚠️ FAQs                  4/10
❌ License                0/10

━━━━━━━━━━━━━━━━━━━━━━━━━━━
TOP 3 RECOMMENDATIONS:
1. Add a professional license number — worth 10 points in a licensable category
2. Expand the introduction to 100+ words — currently too short to score full points
3. Answer at least 7 FAQs — only 3 answered, leaving 6 points on the table
```

### Install

Run this one line in your terminal:

```bash
curl -o ~/.claude/commands/score.md https://raw.githubusercontent.com/daemon-dias/claude-skills/main/score.md
```

### Use

In any Claude Code session, type:

```
/score
```

Claude will ask for the Thumbtack Pro profile URL and whether the category is licensable, then run the full audit.

---

## More skills coming soon

- `/prep` — Pre-call meeting brief for a Pro
- `/ketchup` — Catch up on emails, Slack, and calendar after time away
- `/onboarding` — Track onboarding progress for new Pros

---

*Built by Daemon Dias · Thumbtack Account Management*
