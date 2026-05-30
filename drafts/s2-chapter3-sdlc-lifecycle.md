# S2/Chapter 3 — "The AI-Native SDLC: Where Agents Plug In"
**Series:** AI-Native Program Management Primer
**Source:** `epm-agent-skills-matrix.md`, `aidp-platform-pmo-ai-agent-journey`
**Target length:** 1,000–1,200 words
**LinkedIn/Medium friendly:** Yes

---

## Hook

> The Software Development Life Cycle hasn't changed. You still go from requirements to shipped. What's changed is where the human has to be present — and where they don't.

---

## The Lifecycle Map

Walk through each stage and show: what the agent handles, what the human handles, and what the hand-off looks like.

**Stage 1: Requirements → Tracker Hierarchy**
- Agent can: take a PRD or charter doc, generate the ticket hierarchy (epics → areas → tracks → findings), draft descriptions, assign initial severity
- Human must: validate that the hierarchy reflects the real scope, confirm component assignments, approve before creation
- Hand-off: agent drafts, human approves, agent creates
- Maturity level: L1 (assisted)

**Stage 2: Sprint Planning**
- Agent can: pull all open tickets, group by milestone, flag overloaded assignees, surface tickets with missing ETA
- Human must: make the priority calls, negotiate with engineering leads, decide what's in vs. out of the sprint
- Hand-off: agent generates the data view, human runs the meeting
- Maturity level: L1 — don't try to automate the planning call itself

**Stage 3: Execution Tracking (the core loop)**
- Agent can: daily state check, classify RED/AMBER/GREEN/MISSING, send per-DRI nudges, escalate to leads, create EPM calendar blocks, log all actions to dashboard
- Human must: decide on escalations where the agent flags BLOCKED, negotiate timeline slips, make the go/no-go call
- Hand-off: agent runs at 9 AM, EPM reviews exceptions (typically 3–5 per day, not 126)
- Maturity level: L3 (fully automated, human reviews output only)

**Stage 4: Meeting Follow-Up**
- Agent can: take transcript → extract decisions/action items/risks → create tracker tickets → generate MOM in doc → share doc with attendees → send personalized emails → schedule follow-up
- Human must: review and approve before anything is sent or created
- Hand-off: agent produces the full draft in one pass, human reviews in 5 minutes instead of building it in 45
- Maturity level: L1 (always human-reviewed before send)

**Stage 5: Release / Go-Live**
- Agent can: query all blocking tickets, confirm they're closed, check compliance requirements, generate the go/no-go checklist with current state
- Human must: make the final go/no-go call
- Hand-off: agent generates the checklist with green/red status on every item, human signs off
- Maturity level: L2 (agent generates, human approves the single decision)

**Stage 6: Stakeholder Reporting**
- Agent can: generate the weekly status report, push to dashboard, draft the stakeholder email
- Human must: review the "Help Needed" section (which items to escalate, how to frame them), adjust tone for the specific audience
- Hand-off: agent drafts, human edits the 20% that requires judgment
- Maturity level: L1 → L2 as trust builds

---

## The Pattern

**Fill in your voice — this is the key insight:**

Every stage has the same structure:
1. Agent collects and structures the information (fast, consistent, tireless)
2. Human makes the decisions that require context (org dynamics, priorities, relationships)
3. Agent executes the decision (emails, tickets, calendar, dashboard)

The EPM's job doesn't shrink — it concentrates. Less time gathering, more time deciding.

---

## What to Build First

If you're starting from zero, the order matters:

1. **Daily execution tracker** (L3) — highest ROI, most of your time goes here
2. **Meeting follow-up** (L1) — second highest, runs after every sync
3. **Release checklist** (L2) — high stakes, binary, easy to get right
4. **Status report** (L1→L3) — start reviewed, promote to scheduled over time

Don't start with sprint planning or dependency mapping. Too much external input needed. Start where the inputs are clean and the output is evaluable.

---

## The Takeaway

> The SDLC hasn't changed. What's changed is the ratio of time you spend on each stage. AI-native shifts the ratio toward the stages that actually need you.

---

## Meta notes

- The 6-stage lifecycle map could be a visual in the blog post (same format as the "disappearances" article)
- The L1/L2/L3 system introduced in Article 4 should be referenced here — this is where it gets applied
- "The EPM's job concentrates, not shrinks" is the key line
- Links back to Chapter 2 (tracker design) and forward to Chapter 4 (first skill)
