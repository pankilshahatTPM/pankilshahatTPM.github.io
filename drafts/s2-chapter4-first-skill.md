# S2/Chapter 4 — "Build Your First Agent Skill in a Day"
**Series:** AI-Native Program Management Primer
**Source:** `epm-engineering-tracker`, `generate-program-status` skill
**Target length:** 1,200–1,500 words
**LinkedIn/Medium friendly:** Yes — practical tutorial format

---

## Hook

> The hardest part of building your first agent skill isn't the code. It's deciding what's worth building. Here's the one to start with — and exactly how to do it.

---

## Why Status Reporting First

It's the best first skill because:
- Input is clean: your issue tracker, always available via API
- Output is evaluable: you can look at the numbers and know in 10 seconds if they're wrong
- Failure is low-stakes: a wrong status report gets corrected on the next run; it doesn't send the wrong email to the wrong executive
- Payoff is immediate: 45–90 min/week → 2 min/day

---

## The Three-Part Architecture

Every status reporting skill has the same three parts. Build them in order.

**Part 1: The Query**
Query your issue tracker for all findings tagged to your program. Parameters:
- Tag/keyword: your program's identifier
- Exclude umbrella/parent radars: you want findings only, not the hierarchy nodes
- Fields to request: id, title, state, substate, assignee, priority, closedAt, lastModifiedAt, component, event/sprint field, target date

Batch your requests — most tracker APIs cap at 50–75 results per call.

**Part 2: The Classification**
Map each finding's raw state to one of four display states:

```
Closed/Resolved          → "Closed"
Verify                   → "Verify"
In Development/Integrate → "In Progress"
Substate = Review        → "In Progress"
Sprint field set         → "In Progress"
Everything else          → "Not Started"
```

Read severity from the title prefix (`[Critical]`, `[High]`, `[Mod]`) — not from the priority field. Priority is unreliable; title prefix is always there because you set it.

Read DRI from the assignee field — always live, never cached. (Assignees change without the `lastModifiedAt` updating.)

**Part 3: The Output**
Push the classified findings to a live dashboard. The dashboard auto-computes: % closed, counts by state, counts by component.

Then generate a status email:
- Subject: `[REVIEW] Program Name — AIS Tier 1 Weekly Status | May 29, 2026`
- Body: counts by state, top 3 items needing attention, help needed section
- Write as `.eml` file, open in your mail client, review, send

---

## The Cache Layer (add this on day 2)

Without caching, every run makes 2–3 API calls to your tracker. On a daily schedule, that's ~600 API calls per month.

Add a daily cache:
1. On first run of the day: fetch everything, write to `/tmp/status-cache-YYYY-MM-DD.json`
2. On subsequent runs: load the cache, then check `lastModifiedAt` for all findings in one batch call
3. Re-fetch only the findings where `lastModifiedAt` changed
4. Always re-fetch `assignee` live for all non-Closed findings (assignees change silently)

Real stat: on most days, 1–3 findings changed out of 129. The cache reduces API calls by ~85%.

---

## The Config File (add this on day 3)

Hard-coding your program's details (name, component, email recipients, ARC report ID) means every new program needs a new skill file. Instead, make the skill config-driven:

```yaml
program_name: "Security Certification Q2"
component:
  name: "Platform"
  version: "Security"
email_recipients:
  - lead@company.com
  - pm@company.com
deadline: "2026-05-31"
```

The skill reads the config at runtime. New program = new YAML, same skill.

This is the foundation of the engineering tracker: one provisioner, N programs, all sharing the same skill logic.

---

## The Failure Modes to Expect

**Failure 1 — Component name mismatch**
Your tracker stores component as `name` + `version` (API format). Your config might use the display format (`Name | Version`). These look the same to a human. The API rejects the display format.

Fix: always use separate `name` and `version` fields in your API calls. Never use the pipe-separated display format.

**Failure 2 — Assignee staleness**
The `lastModifiedAt` field doesn't update when only the assignee changes. If you rely on cache + `lastModifiedAt` to know when to re-fetch, you'll miss assignee changes.

Fix: always re-fetch `assignee` live for non-Closed findings, even on cache hits.

**Failure 3 — State classification gaps**
Your tracker has states you didn't account for. "On Hold", "Blocked", "Waiting on Vendor" don't map cleanly to your four display states. The first time you run the skill, you'll find 3–5 states you didn't handle.

Fix: log unmapped states during development. Every unmapped state becomes a new classification rule.

**Failure 4 — Email rendering**
If you're generating HTML emails: no `<!DOCTYPE html>` declaration (renders as visible text in Apple Mail). No HTML comments (same). Assert both are absent before writing the `.eml` file.

---

## What "Done" Looks Like

After one day of building:
- [ ] Skill queries your tracker and returns all findings
- [ ] Classification maps raw states to 4 display states
- [ ] Dashboard shows correct counts (verify manually against tracker)
- [ ] Email draft generates with correct numbers

After one week of daily runs:
- [ ] Cache layer reduces API calls
- [ ] You've corrected 2–3 classification edge cases
- [ ] You've promoted from L1 (you review before send) to L2 (you review exceptions only)

After one month:
- [ ] Scheduled daily, you review output once a week
- [ ] L3: fully automated, you're notified only on exceptions

---

## The Takeaway

> Build the status report skill first. It's the lowest-risk, highest-payoff starting point — and every other skill you build will read from the same data it produces.

---

## Meta notes

- This is the most tutorial-like article in the series — include the actual classification pseudocode
- The "done looks like" checklist is highly practical — keep it
- The failure modes are real from your build — they're what make this credible vs. hypothetical
- Link to the Config Builder UI from the tracker repo
- "Build on day 1, cache on day 2, config on day 3" is a clean narrative arc
