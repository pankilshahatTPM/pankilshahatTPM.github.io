# S1/Article 1 — "I Automated My Weekly Security Program Status Report"
**Series:** What I Built
**Source repos:** `aidp-endor-status`, `generate-endor-status` skill
**Target length:** 1,200–1,500 words
**LinkedIn/Medium friendly:** Yes

---

## Hook

> Every Monday morning I opened my issue tracker, filtered by component — six of them — checked each engineer's status, copied the numbers into a spreadsheet, colored some cells red, pasted it into a slide, and sent an email.
> 126 findings. 40+ engineers. One EPM. About 90 minutes, every week, just to answer: "where do we stand?"
> Then I automated it. Here's exactly what I built, what broke, and what it still can't do.

---

## 1. The Problem — what the manual version looked like

**Fill in your voice here. Prompts:**
- How long did it actually take each week?
- What was the most painful part — pulling the data, formatting it, writing the email, or chasing people who hadn't updated their tickets?
- What happened when you missed a week?
- What did the email look like before — bullet list? slide? Quip doc?

**Real numbers to anchor this section:**
- 126 AIS security findings across 6 components (Agents, Atomic APIs, Evaluations, Ingestions, Keystone Events, Infra)
- 40+ individual DRIs
- Weekly cadence, certification deadline fixed (May 31, 2026)
- State classification was manual: you had to interpret "Analyze" vs "In Progress" vs "Verify" yourself

---

## 2. What I Built

**The architecture (generalized):**

Three moving parts:

**Part 1 — Live data fetch**
Query the issue tracker for all tagged findings. Batch calls because the API caps at 75 per request. For each finding: state, assignee, ETA, component, severity from the title prefix.

**Part 2 — Classification engine**
State mapping (first match wins):
- Closed/Resolved → "Closed"
- Verify → "Verify"
- Integrate/In Development/Fix In Progress → "In Progress"
- Substate = Review → "In Progress"
- Sprint/event field set → "In Progress"
- Anything else → "Not Started"

Severity from title prefix (`[Critical]`, `[High]`, `[Mod]`) — not from the priority field, which was unreliable.

**Part 3 — Push to dashboard + generate email**
129 findings pushed to a live report. Dashboard auto-computes: % closed, counts by state, counts by component.
Email generated from live data — subject, body, key callouts — and written as an `.eml` file ready to open in the mail client.

**The cache layer** (the part nobody talks about):
Run this daily and you're making 2–3 API calls to the issue tracker on every run. Add a daily cache: on cache hit, only re-fetch the findings whose `lastModifiedAt` changed. Cuts API calls by ~85% on days when nothing has moved.

**Real stat:** 1 finding changed `lastModifiedAt` between the May 28 and May 29 runs out of 129. The cache fetched only that 1.

---

## 3. What Broke

**Fill in your voice here. These are the real failure modes from the build:**

**Broken #1 — Assignee staleness**
The issue tracker's `lastModifiedAt` field does not update when only the assignee changes. This means an engineer could be reassigned and the cache would never know. Fix: always re-fetch assignee live for all non-Closed findings on every run, regardless of cache hit.

*Why this matters:* You send a follow-up email to the wrong person. They reply "that's not my ticket." You've burned credibility.

**Broken #2 — Component mapping**
The issue tracker stores findings under the actual service component name (e.g. "Endor | Secure Endor"). The dashboard uses umbrella component names (Agents, Ingestions, Infra). These are different. The first version of the script used the raw tracker name — the dashboard showed every finding as "Ingestions."

Fix: maintain a separate mapping from umbrella parent radar ID to display name. Any finding whose ID appears in a parent's `relatedProblemIds` gets that parent's comp name.

**Broken #3 — The 48-hour visibility rule**
A closed finding shouldn't disappear from the dashboard immediately — it should stay visible for 48 hours so stakeholders can see what just closed. First version hid all closed findings. Second version showed all closed findings (too noisy — 65 closed items in a 129-row table). Fix: `hidden = closedAt < (now - 48h)`.

**Broken #4 — API keyword format**
Adding a tag/keyword to a finding via the API required a numeric ID, not the keyword name. `{"keywords": [{"name": "AIS Tier1"}]}` returns a validation error. `{"keywords": {"insert": [{"id": 2288296}]}}` works. Spent 45 minutes on this. The ID had to be looked up from an existing tagged finding.

**Broken #5 — Email rendering in Apple Mail**
Generated HTML emails with `<!DOCTYPE html>` at the top. Apple Mail's WebKit renderer displays the DOCTYPE declaration as visible text. Same problem with HTML comments (`<!-- -->`). Fix: assert both are absent before writing the `.eml` file.

---

## 4. What It Still Can't Do

**Fill in your voice here. Prompts:**
- What do you still decide manually every week when you look at the output?
- When does the "In Progress" classification lie?
- What's the call you make that no classification rule could make?

**Anchors:**
- It can't decide which overdue finding to escalate vs. let slide. An engineer with 6 overdue findings and a valid blocker is different from one with 6 overdue findings who just hasn't updated their ticket. The classification looks identical.
- It can't read the difference between a sprint ETA that's a real commitment and one that was set to stop the nudge emails. Both show as "In Progress, ETA 06/02."
- It can't decide what goes in the "Help Needed" section of the email. That's judgment about organizational dynamics, not data.

---

## 5. The Takeaway

> The 90 minutes wasn't the work. It was the tax on the work. Automating it didn't change what I do — it changed what I have time to do.

---

## Meta notes for writing

- Keep the code/technical details minimal — this isn't a tutorial. The point is the decisions and the failures, not the implementation.
- The "What Broke" section is what makes this credible. Don't soften it.
- One concrete before/after stat: "90 minutes manually → 2 minutes automated, daily instead of weekly."
- Link to the live portal at the end if it's still up.
