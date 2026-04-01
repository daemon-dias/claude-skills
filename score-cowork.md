---
name: score-cowork
description: "Cowork version of /score — scores the Thumbtack Pro profile currently open in the browser tab."
---

# /score (Cowork Edition)

> Scores the Thumbtack Pro profile **currently open in the active browser tab** on a 100-point rubric. Returns a full breakdown, flags missing elements, and gives the top 3 highest-impact recommendations.

---

## Step 1 — Validate the page

Read the current page content. Confirm the URL is a Thumbtack Pro profile (contains `thumbtack.com` and a service path). If not, tell the user to open a Thumbtack Pro profile first.

Then ask one question:

"Is this a licensable category? (Yes / No)"

> **Licensable category examples:** HVAC, plumbing, electrical, general contracting, roofing, pest control, tree services, and most home services that require a state license to operate.

Wait for the answer before proceeding.

---

## Step 2 — Read the profile

Read the full page content from the **currently open tab**. Do NOT navigate away — the profile is already loaded.

Gather each of the following criteria:

| # | Criterion | What to look for on the page |
|---|-----------|------------------------------|
| 1 | Profile photo | Any image in the circular profile photo position (headshot, logo, or team photo all count) |
| 2 | Background check | "Background checked" badge in the Overview section |
| 3 | Business name | Business name displayed on the profile |
| 4 | Year founded | "X years in business" or "In business since [year]" |
| 5 | Number of employees | Employee count or team size listed in Overview |
| 6 | Payment methods | Any payment methods listed (cash, card, Venmo, Zelle, etc.) |
| 7 | Social media | Any social media links (Facebook, Instagram, etc.) |
| 8 | Introduction | A written About/bio section |
| 9 | Reviews | Customer reviews present on the profile |
| 10 | Photos/videos | Click into "Projects and media" — count photos/videos present |
| 11 | Featured project | Click into "Projects and media" — look for any photo that has a **cost/price attached**. If pricing is shown alongside a photo, that's a featured project. |
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
  - Medium (50-99 words) = 15
  - Detailed (100+ words) = 20

- Reviews /15:
  - 0 reviews = 0
  - 1-2 reviews = 3
  - 3-5 reviews = 7
  - 6-9 reviews = 10
  - 10+ reviews = 15

- Photos/videos /15:
  - 0 = 0
  - 1-2 = 3
  - 3-5 = 7
  - 6-9 = 10
  - 10+ = 15

- Featured project /10:
  - Present = 10
  - Missing = 0

- FAQs /10:
  - Each answered FAQ = ~1.4 points
  - 7+ FAQs answered = 10

### Max score

- **Non-licensable category:** Max = 90 (rubric sums to 90)
  - Normalize: `Final Score = round((raw score / 90) x 100)`
- **Licensable category:** Max = 100 (no normalization needed)

---

## Step 4 — Output the scorecard

```
PROFILE SCORE — [Business Name]
[Profile URL from the current tab]
Category: [Licensable / Non-licensable]

OVERALL SCORE: [X]/100

BREAKDOWN:
Profile Photo          X/2
Background Check       X/2
Business Name          X/2
Year Founded           X/2
Employees              X/2
Payment Methods        X/5
Social Media           X/5
Introduction           X/20
Reviews                X/15
Photos/Videos          X/15
Featured Project       X/10
FAQs                   X/10
License                X/10   <- omit if non-licensable

MISSING / INCOMPLETE:
[Criterion] — [brief note on what's missing]

TOP 3 RECOMMENDATIONS (highest impact):
1. [Specific action to improve score]
2. [Specific action to improve score]
3. [Specific action to improve score]
```

---

## Output Footer

Always end every scorecard output with these two lines:

*I can't view business insurance status*
*Most recent review: [Month Day, Year]*

---

## Error Handling
- If the current page is not a Thumbtack profile: ask the user to navigate to one first
- If a criterion is ambiguous: give benefit of the doubt and mark as present
- If the user answers "No" to licensable category: exclude license from scoring entirely
