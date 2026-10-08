# Phrom Backlog Demo

> **Demo backlog for [Phrom](https://github.com/MKalder/phrom) (พร้อม)**
> _A read-only CLI that checks GitHub Issues before backlog refinement and drafts improvements for the Product Owner to review._

[Deutsche Fassung](README.de.md)

This repository contains only **synthetic sample data**: epics, stories, tasks and bugs for a fictional self-service customer portal. It is the test backlog for Phrom and contains no real product code or confidential information.

Why code and demo data live in separate repositories is documented in [ADR-002: Separate Repositories for Code and Demo Backlog](https://github.com/MKalder/phrom/blob/main/adr/en/ADR-002-separate-repositories.en.md).

---

## Purpose

1. **Try Phrom without risk:** Phrom only reads from this repository. It needs no write access, and for a public repository it does not even need a token.
2. **A fixed test backlog with known weaknesses:** The issues deliberately contain typical gaps, such as missing acceptance criteria, missing context or items that are too large. Every type also has at least one good control issue that should not be flagged.
3. **Refinement showcase:** `npm run demo` in the main repository shows a formal pre-check, an AI analysis and a before/after draft using these issues.

The expected findings for each issue are not stored here, but in the main repository (`seed/issues.json`). There, `node scripts/eval-seed.js` compares Phrom's results with these expectations. A comparison of different models has not been done yet.

---

## Contents

19 open issues (as of 2026-10-08):

- **#1 to #18** come from the seed file of the main repository: 3 epics, 7 stories, 4 tasks and 4 bugs, each with the `demo-seed` label.
- **#19** "Test Issue without issue type" was created by hand **without a type label** to test Phrom's type detection.

| # | Type | Title | Built as | Result on 2026-10-08 |
| ---: | --- | --- | --- | --- |
| 1 | Epic | Invoice self-service | good control issue | 100/100 🟢 |
| 2 | Story | Download invoice as PDF | good control issue | 100/100 🟢 |
| 3 | Story | Improve login | one-sentence story | 10/100 🔴 |
| 4 | Story | Reset password | product and target group missing | 80/100 🔴 |
| 5 | Story | Manage account settings | too broad | 36/100 🔴 |
| 6 | Epic | Modernize the portal | weak epic | 0/100 🔴 |
| 7 | Story | View invoice overview | good control issue | 100/100 🟢 |
| 8 | Story | Change payment method | epic reference missing | 90/100 🟢 |
| 9 | Task | Database migration to PostgreSQL 18 | weak task | 10/100 🔴 |
| 10 | Bug | PDF download fails on mobile Safari | acceptance criteria missing | 52/100 🔴 |
| 11 | Story | Download invoice | too few acceptance criteria | 90/100 🔴 |
| 12 | Epic | Digital customer experience | too broad | 41/100 🔴 |
| 13 | Task | Database migration to PostgreSQL 16 | good control issue | 100/100 🟢 |
| 14 | Task | Update the database | one-sentence task | 0/100 🔴 |
| 15 | Task | Modernize infrastructure | too broad | 50/100 🔴 |
| 16 | Bug | PDF download fails on mobile Safari (iOS 16) | good control issue | 100/100 🟢 |
| 17 | Bug | Download is broken | one-sentence bug | 0/100 🔴 |
| 18 | Bug | Intermittent login failures across all platforms | complex bug | 57/100 🔴 |
| 19 | – (Task, detected by the model) | Test Issue without issue type | no label | 15/100 🔴 |

The results come from a `phrom run` on 2026-10-08 with the model `qwen3:30b-instruct` and rule set 0.3.1. They can differ with another model or rule version.

- 🟢 means only that an item meets the criteria of the rule set. It is not a sprint commitment.
- #8 is 🟢 despite the missing epic reference because `epic-link` is optional.
- #4 and #11 are 🔴 despite 80 and 90 points because a required criterion fails.

---

## Labels

Phrom uses the type label to choose the criteria that apply:

| Label | Meaning | What Phrom checks |
| --- | --- | --- |
| `type:epic` | Large initiative | Goal, benefit, list of stories or slices, size risk |
| `type:story` | User story | Story format, context, acceptance criteria and their testability, value, size risk |
| `type:task` | Technical task | Scope, justification, impact, rollback plan, feasibility |
| `type:bug` | Bug report | Reproduction steps, expected vs. actual behavior, environment, severity |
| `demo-seed` | Seed marker | Marks issues created by the seed script; not used for the assessment |

If the type label is missing, the model assigns a type (example: #19). This type detection has not been evaluated, so set the labels yourself in your own backlog.

The complete rule set: [Phrom Rules (v0.3.1)](https://github.com/MKalder/phrom/blob/main/docs/RULES.en.md).

---

## Using this demo backlog

This repository is public. You do not need write access.

**Requirements:** Node.js, a running [Ollama](https://ollama.com) with the model `qwen3:30b-instruct` (about 19 GB of RAM), and the Phrom main repository.

1. **Clone and install the main repository:**

   ```bash
   git clone https://github.com/MKalder/phrom.git
   cd phrom
   npm install
   cp .env.example .env
   ```

2. **Set these values in `.env`:**

   ```env
   GITHUB_OWNER=MKalder
   GITHUB_REPO=phrom-backlog-demo
   OLLAMA_HOST=http://localhost:11434
   MODEL_NAME=qwen3:30b-instruct

   # optional, only raises the GitHub API limit:
   #GITHUB_TOKEN=github_pat_your_token_here
   ```

   **About the token:** No token is needed for this public repository. Without a token, GitHub allows 60 API requests per hour per IP address. One demo run needs about 25, so the limit is reached on the third run within an hour. Any token raises the limit to 5,000 per hour. A fine-grained token with read access to public repositories is enough. Phrom never writes to GitHub.

3. **Start the guided demo:**

   ```bash
   npm run demo
   ```

   Or run individual commands:

   ```bash
   npm run phrom status        # formal pre-check, rules only, a few seconds
   npm run phrom select 3 7    # assess selected issues including AI
   npm run phrom improve 3     # assess and create an improvement draft
   npm run phrom run           # assess all issues (about 12 minutes on a CPU server)
   ```

**Note on drafts:** Improvement drafts are suggestions. In the tests, they contained details the model had invented or copied from reference examples. Check every draft before you use any part of it.

---

## Important notes

- **No issues or pull requests here:** Report bugs and feature requests for Phrom in the main repository: [github.com/MKalder/phrom/issues](https://github.com/MKalder/phrom/issues).
- **Seed and reset:** Only the separate helper scripts in the main repository write to this backlog, using their own login and never Phrom's analysis token (see [ADR-004](https://github.com/MKalder/phrom/blob/main/adr/en/ADR-004-seed-and-reset-scripts.en.md)):
  - `npm run seed` creates missing seed issues and skips issues whose title already exists.
  - A separate reset script deletes the issues; it needs admin permissions and an explicit confirmation.
- **Issue numbers can change:** GitHub does not reset the issue counter. After a reset, new issues continue from the next free number, so references like "#3" in documentation can point to a different issue afterwards.

---

## License & Copyright

© 2026 Marius Kalder. All rights reserved.
This test set is provided exclusively for demonstration and testing purposes in conjunction with [Phrom](https://github.com/MKalder/phrom).
