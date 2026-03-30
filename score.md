---
name: score
description: "Thumbtack Pro profile scorer. Trigger when user types /score or asks to 'score a profile', 'audit a profile', or 'check a pro's profile'."
---

# /score 🎯

> **What this does:** Scores a Thumbtack Pro's customer-facing profile on a 100-point rubric. Returns a full breakdown, flags missing elements, and gives the top 3 highest-impact recommendations to improve the profile.
>
> **Who it's for:** Thumbtack Account Managers — use it before calls to identify coaching opportunities, or to benchmark a Pro's profile health at a glance.
>
> **Requirements:** Claude Code with the Chrome browser tool enabled.
>
> **How to install (team members):** Copy this file into your `~/.claude/commands/` folder. Then type `/score` in any Claude Code session.

---

## Step 1 — Collect inputs

Ask the user two questions (can ask both at once):

1. "Paste the Thumbtack Pro profile URL:"
2. "Is this in a licensable category? (Yes / No)"

> **Licensable category examples:** HVAC, plumbing, electrical, general contracting, roofing, pest control, tree services, and most home services that require a state license to operate. When in doubt, ask — or check the Pro's profile for a license badge.

Wait for both answers before proceeding.

---

## Step 2 — Scrape the profile

Use the Chrome tool to navigate to the provided URL and read the full page content.

Look for each of the following criteria:

| # | Criterion | What to look for on the page |
|---|-----------|------------------------------|
| 1 | Profile photo | A circular profile/headshot image for the Pro (not a service photo) |
| 2 | Background check | "Background checked" badge in the Overview section |
| 3 | Business name | Business name displayed on the profile |
| 4 | Year founded | "X years in business" or "In business since [year]" |
| 5 | Number of employees | Employee count or team size listed in Overview |
| 6 | Payment methods | Any payment methods listed (cash, card, Venmo, Zelle, etc.) |
| 7 | Social media | Any social media links (Facebook, Instagram, etc.) |
| 8 | Introduction | A written About/bio section |
| 9 | Reviews | Customer reviews present on the profile |
| 10 | Photos/videos | Click into "Projects and media" — count photos/videos present |
| 11 | Featured project | Click into "Projects and media" — look for any photo that has a **cost/price attached** (e.g. "Approx. $150"). If pricing is shown alongside a photo, that's a featured project. |
| 12 | FAQs | Click the FAQs tab — count answered FAQ questions (click "Show more" to expand all). **Note: the FAQs tab only appears when at least 1 FAQ is present. If the tab is not visible, score 0/10.** |
| 13 | Most recent review | In the Reviews section, click the sort dropdown ("Most relevant") and select "Newest first" — note the date of the first review shown |
| 14 | Professional license | License number or badge visible (only check if licensable category = Yes) |

---

## Step 3 — Score each criterion

### Scoring rubric

**Binary criteria (present = full points, missing = 0):**
- Profile photo: /2
- Background check: /2
- Business name: /2
- Year founded: /2
- Number of employees: /2
- Payment methods: /5 — any present = 5, none = 0
- Social media: /5 — any present = 5, none = 0
- Professional license: /10 — present = 10, missing = 0 (**only score if licensable category = Yes, otherwise exclude entirely**)

**Scaled criteria:**
- Introduction /20:
  - Missing = 0
  - Short (under 50 words) = 10
  - Medium (50–99 words) = 15
  - Detailed (100+ words) = 20

- Reviews /15:
  - 0 reviews = 0
  - 1–2 reviews = 3
  - 3–5 reviews = 7
  - 6–9 reviews = 10
  - 10+ reviews = 15

- Photos/videos /15:
  - 0 = 0
  - 1–2 = 3
  - 3–5 = 7
  - 6–9 = 10
  - 10+ = 15

- Featured project /10:
  - Present with cost attached = 10
  - Present but no cost = 5
  - Missing = 0

- FAQs /10:
  - Each answered FAQ = ~1.4 points
  - 7+ FAQs answered = 10

### Max score

- **Non-licensable category:** Max = 90 (rubric sums to 90)
  - Normalize non-licensable scores: `Final Score = round((raw score / 90) × 100)`
- **Licensable category:** Max = 100 (license adds 10 points on top — no normalization needed)

---

## Step 4 — Output the scorecard

```
🎯 PROFILE SCORE — [Business Name]
[Profile URL]
Category: [Licensable / Non-licensable]

━━━━━━━━━━━━━━━━━━━━━━━━━━━
OVERALL SCORE: [X]/100
━━━━━━━━━━━━━━━━━━━━━━━━━━━

BREAKDOWN:
✅ Profile Photo          2/2
✅ Background Check       2/2
✅ Business Name          2/2
✅ Year Founded           2/2
✅ Employees              2/2
✅ Payment Methods        5/5
✅ Social Media           5/5
⚠️ Introduction          X/20
✅ Reviews               X/15
✅ Photos/Videos         X/15
✅ Featured Project      X/10
✅ FAQs                  X/10
✅ License               X/10   ← omit if non-licensable

━━━━━━━━━━━━━━━━━━━━━━━━━━━
MISSING / INCOMPLETE:
❌ [Criterion] — [brief note on what's missing]
⚠️ [Criterion] — [brief note if partially complete]

TOP 3 RECOMMENDATIONS (highest impact):
1. [Specific action to improve score]
2. [Specific action to improve score]
3. [Specific action to improve score]
```

---

## Output Footer

Always end every scorecard output with these two lines in italics:

*I can't view business insurance status*
*Most recent review: [Month Day, Year]*

---

## Error Handling
- If the URL doesn't load or is not a Thumbtack profile: ask the user to check the link and try again
- If the page is behind a login wall: note which criteria couldn't be verified and score only what's visible
- If a criterion is ambiguous (e.g., can't tell if photo is a profile photo vs. service photo): give benefit of the doubt and mark as present
- If the user answers "No" to licensable category: exclude license from scoring entirely — do not show it in the breakdown
