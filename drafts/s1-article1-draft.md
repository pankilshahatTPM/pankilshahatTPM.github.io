# S1/Article 1 — DRAFT v2
# "I Stopped Chasing Engineers for Status Updates. Here's What I Built Instead."
**Status:** First draft — [YOUR VOICE] markers need your words, everything else is from the actual build
**Word count target:** 1,200–1,500

---

Every week I spent 90 minutes preparing a security program status report. I opened the issue tracker, filtered by component — six of them, no hierarchy — checked each engineer's state, copy-pasted numbers into a spreadsheet, and sent an email nobody was confident was accurate.

The problem wasn't that I was slow. The problem was that the data was wrong before I even started.

---

## The Real Problem Was Two Problems

When I started managing this program, 126 security findings were tracked across 6 engineering components with no parent/child hierarchy. Radar's native interface wasn't designed to answer the questions a program manager needs answered every day: *who owns what, what's the state of each component, what's overdue?* I tried building a tracker in Quip first. It worked for human viewing but couldn't be automated reliably — every update was still manual.

The harder problem was the data source itself.

Engineers were supposed to update their radar tickets as they made progress. In practice, they didn't — not because they didn't care, but because updating a ticket manually is friction that competes with actually fixing the issue. A finding would be code-complete, tested, and deployed to staging. The radar still said "Analyze." I had no way to know unless I asked.

So I was spending 90 minutes each week building a report from data I knew was stale.

[YOUR VOICE — 1–2 sentences on what the consequence was. Did you have inaccurate escalations? Did engineering leads push back on your counts? Did leadership make decisions on wrong numbers?]

---

## Solution 1: The Automated Dashboard

The first problem — manual status aggregation — I solved with a Claude skill and an HTML dashboard.

**The query layer**

Using the Radar MCP (Radar's API exposed as a tool the LLM can call), I built a skill that runs on a schedule and pulls the current state of every finding tagged to the program. Two batched API calls, 75 findings per call. Fields per finding: state, assignee, sprint/event field, target date, component, severity from the title prefix, closedAt, lastModifiedAt.

**The classification engine**

Raw tracker states don't map cleanly to what stakeholders need. I defined four display states and a rule set (first match wins):

```
Closed / Resolved          →  Closed
Verify                     →  Verify  
In Development / Integrate →  In Progress
Substate: Review           →  In Progress
Sprint field set           →  In Progress
Everything else            →  Not Started
```

Severity from the title prefix (`[Critical]`, `[High]`, `[Mod]`), not from the priority field — priority gets set inconsistently and overwritten by automated workflows. DRI name always fetched live from the assignee field, never from cache. (More on why in the "What Broke" section.)

**The dashboard**

All findings pushed to a live HTML portal. The portal auto-computes: percentage closed, counts by state, counts by component. A new branch, PR, and merge every time it runs. Auto-deploys in ~3 minutes. Stakeholders bookmark it. It's always current.

Before: 90 minutes, weekly, manual.
After: under 2 minutes, daily, automated.

216 pull requests merged since February 2026.

---

## Solution 2: The Git-to-Radar Integration

The second problem — engineers not updating their tickets — required a different approach. Asking people to remember a manual step doesn't work. Making the step automatic does.

I worked with the QE and SRE teams to deploy a git-to-radar integration. The rule: every pull request an engineer opens automatically updates the corresponding radar ticket with the PR details and moves the state forward.

Engineer opens a PR → radar moves from Analyze to In Development, PR link attached.
PR merges to dev → radar moves to Integrate.
Deploy to stage → radar moves to Verify.
Deploy to prod → radar moves to Closed.

The engineer does the work. The ticket updates itself.

[YOUR VOICE — what was the hardest part of getting this integration in place? Was it the SRE team, the Radar API permissions, getting engineers to include the radar ID in their PR titles, or something else?]

This was the more impactful fix of the two. The automated dashboard was only useful if the data it read was accurate. The git integration made the data accurate.

---

## What Broke

**Assignee staleness**

The tracker's `lastModifiedAt` field doesn't update when only the assignee changes. An engineer gets reassigned — the ticket looks unchanged to my cache. The status report emails the previous owner.

This happened. The engineer replied: "that's not mine." 

Fix: always re-fetch the assignee field live for all non-Closed findings, even on cache hits. One extra API call per batch, worth it every time.

**The 48-hour visibility rule**

First version: hide all closed findings. Stakeholders wanted to see what just closed.
Second version: show all 65 closed findings. Too noisy in a 129-row table.
Correct version: show closed findings for 48 hours after `closedAt`, then hide them. The dashboard shows the recent story without carrying all of history.

**Component mapping**

The tracker stores findings under the raw service component name. The dashboard uses a simplified six-area taxonomy (Agents, APIs, Evaluations, Ingestions, Infrastructure, Events). These are different. First version mapped everything to "Ingestions."

Fix: maintain a lookup from parent umbrella radar ID to display component name. Any finding whose ID appears in a parent's child list gets that parent's taxonomy name.

**The keyword ID problem**

Adding the program tag to a finding via the API requires a numeric keyword ID, not the keyword name string. `{"keywords": {"insert": [{"name": "Program Name"}]}}` returns an error. `{"keywords": {"insert": [{"id": 2288296}]}}` works. The ID has to be looked up from an existing tagged finding. Not documented anywhere obvious. Cost 45 minutes.

**Email rendering**

Generated HTML emails with `<!DOCTYPE html>` at the top. Mail clients render the DOCTYPE as visible text at the top of the email. Same problem with HTML comments (`<!-- -->`). Fix: assert both are absent before writing the email file. The assertion runs on every generation — if it fails, the skill errors out before writing.

---

## What It Still Can't Do

**It can't track the full fix journey from PR merge to production — and that's still an open problem.**

This is the biggest gap. The git integration handles the Dev side: engineer opens a PR, radar moves to In Development. PR merges, radar logs the commit. That part works.

What we haven't solved reliably: the deployment trail. When a fix merges to main, does it actually get deployed to Stage? When it's in Stage, has it been validated? When was it promoted to Prod? These are separate pipeline events — build, deploy, validate, promote — and right now there's no auditable, automated link between "PR merged" and "fix confirmed in production."

In practice, engineers post a comment on the radar or the EPM follows up manually. That's the same problem we started with — just one step further down the pipeline.

The right solution probably involves deployment pipeline webhooks writing back to the radar at each stage gate. That work isn't done yet.

This matters for any program with a compliance or certification requirement: an auditor doesn't want to see "In Verify" on a radar. They want a timestamp-stamped trail: code changed → merged → deployed to Stage on date X → deployed to Prod on date Y → validated by team Z. We have the first two. The rest is still manual.

**It can't distinguish a real blocker from a stale ticket.**

The git integration handles the normal path — PRs flowing, radars moving. It doesn't handle the exception: engineer is blocked on an external dependency, nothing is moving, radar looks identical to a ticket nobody's touched. That still surfaces as a stale ETA, not a blocker, until someone says something.

**It can't read whether an ETA is a real commitment.**

A finding targeting June 2 could be a genuine engineering commitment or a date set to stop the automated nudge emails. Both look identical. Distinguishing them requires a conversation.

**It can't decide what to escalate.**

The dashboard surfaces what's overdue, what's missing ETAs, what has dates past the program deadline. It doesn't decide which of those to escalate to leadership versus manage quietly. That's organizational judgment the agent doesn't have context for.

---

## The Numbers

- Status report: 90 min/week manual → 2 min/day automated
- Dashboard PRs merged: 216 since February 2026
- Findings tracked: grew from 126 to 129 over the program lifetime
- Data quality: went from "last updated whenever someone remembered" to "updated at every PR merge"

---

## The One Thing That Made the Biggest Difference

Not the dashboard — the git integration.

The dashboard made *my* job easier. The git integration made the *data* accurate. A beautiful automated report built on stale data is still a stale report. Fixing the data source was worth more than any amount of automation on top of it.

[YOUR VOICE — anything else you want to add here? What would you tell another EPM starting this same project?]

---

*Next in this series: [Article 2 — One Config File to Provision an Entire Engineering Program]*
