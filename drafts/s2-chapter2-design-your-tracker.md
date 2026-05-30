# S2/Chapter 2 — "Design Your Issue Tracker for Agents, Not Humans"
**Series:** AI-Native Program Management Primer
**Source:** `epm-engineering-tracker`, `epm-agent-skills-matrix.md`
**Target length:** 1,000–1,200 words
**LinkedIn/Medium friendly:** Yes — practical, checklist format, high shareability

---

## Hook

> Most issue trackers are designed for humans to read in a meeting. Ticket titles that make sense in context. Status fields that mean "ask me what this means." ETAs that are optimistic suggestions.
> An agent can't ask. It reads what's there and classifies it. Design your tracker for the agent, and humans benefit too.

---

## 1. The 5 Fields Every Agent Needs

**Fill in your voice — these are the non-negotiable fields from your build:**

**Field 1: State (structured, not freeform)**
Every ticket needs a state the agent can map to one of 4 buckets: Closed / Verify / In Progress / Not Started. This means your team can't use the state field creatively. "On hold" is Not Started. "Waiting on legal" is Blocked (a substate of In Progress). The mapping has to be explicit and documented.

*Real failure mode:* You have 126 findings. 17 have no state set — they're in the tracker's default "Open" state. The agent counts them as Not Started. Leadership asks why 17% of findings are not started when engineers swear they're working on them.

**Field 2: Assignee (clean name, valid person)**
The agent sends emails and creates calendar events based on the assignee field. If the assignee is a team alias, a former employee, or empty — the agent either errors out or sends to the wrong person.

*Real failure mode:* An engineer was reassigned three weeks ago. The ticket still shows the old assignee. The agent sends the follow-up email to someone who no longer owns the work. They reply "not mine." You've burned a relationship.

Fix: run a weekly check that every open ticket's assignee is a valid, active person. Make this a hygiene nudge, not a manual audit.

**Field 3: ETA / Target Date (future, ISO format)**
The agent compares ETA to today. If the field is empty, the finding is invisible to deadline tracking. If the date is in the past and the finding is still open, it's overdue. The format has to be machine-parseable — "Q2" is not a date, "June" is not a date, "06/02" is ambiguous. Use ISO 8601 (YYYY-MM-DD) or your tracker's native date field.

*Real stat:* In one sweep, 20 out of 126 open findings had no ETA set. Each one is a deadline risk the daily monitor can't see.

**Field 4: Component / Area (controlled vocabulary, not freeform)**
The agent groups findings by component to generate per-area health summaries. If your component names are inconsistent — "Infra" vs "Infrastructure" vs "infra-team" — the agent creates three buckets where you intended one.

*Real failure mode:* The API stores component as `name` + `version` (separate fields). The UI displays it as "Name | Version". Your config says "Infra | AWS" but the API needs `{"name": "Infra", "version": "AWS"}`. Using the display format returns a validation error.

**Field 5: Severity (from title prefix, not from priority field)**
The agent reads severity from the title prefix (`[Critical]`, `[High]`, `[Mod]`). The priority field is unreliable — engineers set it inconsistently, it gets overwritten by automated workflows, and different trackers use different scales. Embedding severity in the title makes it human-readable in any view and machine-readable in any query.

---

## 2. The Taxonomy Convention

**Fill in your voice — this is the area/track/finding hierarchy from your build:**

One umbrella per program → N area umbrellas → M track radars per area → findings tagged to the program.

The hierarchy matters because the agent traverses it to generate the "per-component" breakdown. Without it, all findings are flat — you can filter by assignee but not by architectural area.

---

## 3. The Enforcement Layer

Designing the fields isn't enough. You need enforcement.

**Option A — Hygiene nudge (built):**
A weekly skill that queries all open tickets, finds any with missing ETA, missing assignee, or missing milestone, and emails the DRI with a specific ask. Not a Slack blast to the channel — one targeted email per person. "You have 3 tickets with no ETA. Here are the links."

**Option B — PR validation (not built, worth considering):**
Any ticket created as a code review reference fails the CI hook if milestone or priority is missing. This enforces the convention at commit time, not in a follow-up email.

---

## 4. What Good Looks Like

A well-designed tracker produces this output from the agent daily, with no manual input:

```
DAILY MONITOR — 2026-05-29
────────────────────────────
🔴 RED (12)     — overdue, still open
🟡 AMBER (23)   — in progress, ETA within 7 days
🟢 GREEN (11)   — in progress, ETA > 7 days
⚪ MISSING (17) — no ETA, no priority, or no assignee
⚠️ RISK (11)    — ETA past the program deadline
```

If you're getting MISSING > 10%, your taxonomy discipline is breaking down. That's the leading indicator, not the RED count.

---

## 5. The Takeaway

> A tracker designed for agents is also a better tracker for humans. The discipline that makes findings machine-classifiable is the same discipline that makes a program actually trackable.

---

## Meta notes

- The "5 fields" format is highly shareable on LinkedIn — consider making this a carousel post too
- The real failure modes (assignee staleness, component format, ETA = "Q2") are what make this credible — don't cut them
- The MISSING > 10% heuristic is a concrete, memorable takeaway
- Links forward to Chapter 4 (first skill to build on top of this clean tracker)
