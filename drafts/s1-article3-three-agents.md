# S1/Article 3 — "I Built Three Agents to Run My Security Program. Here's What Actually Happened."
**Series:** What I Built
**Source repo:** `agentic-avengers`
**Target length:** 1,500–1,800 words
**LinkedIn/Medium friendly:** Yes

---

## Hook

> I managed 126 security findings across 6 engineering components with a single deadline. The manual version of my job was: open tracker every morning, check each DRI's status, send follow-up emails, update a spreadsheet, repeat.
> At the hackathon I built three agents to replace that loop. They worked. Not perfectly. Here's exactly what each one does, what broke, and the one thing none of them can do.

---

## 1. The Problem

**Fill in your voice here. Prompts:**
- What was the daily manual loop before the agents?
- How long did it take? What was the failure mode — findings slipping, wrong person notified, stale data?
- What was the specific pain point that made you decide to build this at a hackathon vs. just improving the manual process?

**Real context:**
- 126 AIS security findings (grew to 129 during the build period)
- 6 components: Agents, Atomic APIs, Evaluations, Ingestions, Keystone Events, Infra
- 40+ individual DRIs
- Fixed certification deadline: May 31, 2026
- One EPM (you)

---

## 2. What I Built — Three Agents

### Agent 1 — The Findings Processor (`/radar-findings-monitor`)
**Trigger:** EPM-triggered (on demand)
**What it does:**
- Reads all 126+ findings from the live dashboard
- Classifies each into 4 buckets: OVERDUE / NOT STARTED / ETA NEEDED / DUE SOON
- Generates a terminal report (formatted, color-coded)
- Sends one batched email per DRI (not one per finding — a critical design decision)
- Creates calendar blocks for OVERDUE DRIs in their timezone, next available slot

**Real numbers from the May 17 dry run:**
- 126 findings loaded
- 3 OVERDUE · 9 NOT STARTED · 20 ETA NEEDED · 18 DUE SOON
- 2 calendar blocks created: one DRI in Cupertino, one in Cupertino (different times)

**Key design decision to explain:** One email per DRI. Early version sent one email per finding. An engineer with 6 overdue findings got 6 emails. They started ignoring all of them. Batching fixed this — one email with all their items, one ask.

---

### Agent 2 — The Daily Monitor (`/arc-daily-monitor`)
**Trigger:** Automated, daily at 9 AM
**What it does:**
- Reads all findings and classifies them: RED / AMBER / GREEN / MISSING FIELDS / DEADLINE RISK
- Sends batched escalation emails (one per DRI for Tier 1; one combined email to EPM + lead for Tier 2)
- Creates calendar blocks for RED components on the EPM's calendar
- Sends EPM a summary with macOS notification

**Real numbers from the May 20 run:**
- 126 findings: 🔴 12 · 🟡 23 · 🟢 11 · ⚪ 17 (missing fields) · ⚠️ 11 (deadline risk)
- Ashok: 15 Infra findings, all missing fields (new AWS audit, 4 days old) → 1 Tier 1 email to Ashok + manager
- Neeti: 9 Dhari findings, ETA 06/09 — 9 days past the May 31 deadline → Tier 2 risk email to EPM + lead
- Hani: 6 overdue iConvert findings (ETA 05/19, all RED)
- Calendar blocks: Agents review 9:30 AM · Ingestions review 10:00 AM

---

### Agent 3 — The Reply Processor (`/re-ai-intelligence`)
**Trigger:** EPM pastes a DRI's email reply
**What it does:**
- Parses the reply: DONE / BLOCKED / ACK
- DONE → adds a note to the tracker ticket only (no email to EPM)
- ACK → adds a note only
- BLOCKED → adds a note, logs to dashboard, emails EPM, creates calendar block with that DRI to work through the blocker

**Why this agent is the most interesting:**
The intelligence isn't in reading the tracker — it's in reading the human. Same words, different meaning. "Looking into it" is ACK if it's day 1, it might be BLOCKED if it's day 10. The agent classifies based on explicit signals; the EPM escalates based on context.

**Real test results:**
- DONE (Charlie Chang, Topicality encryption): note logged, no email ✅
- BLOCKED (Garrett Souza, CVV Guardrail): note + dashboard log + EPM email + 30-min calendar block with Garrett ✅
- ACK (Rakshit, verbose trace finding): note logged, no email ✅

---

## 3. What Broke

**Fill in your voice here. Use these as anchors:**

**Broken #1 — 20+ emails became 1**
First version: one email per finding. Engineer with 6 overdue items got 6 separate emails. Inbox chaos → they ignored all of them. Fix: batch by DRI. One email, all their items, one ask. Lesson: agent outputs are stakeholder communications. Design them like a human would write them.

**Broken #2 — Wrong MCP endpoint**
Agent 1 was calling the staging dashboard endpoint (`mcp__report-builder__`) instead of production (`mcp__report-builder-prod__`). Caught it in dry run. Production had the real data; staging had stale test data. The fix was one word change in the skill config — but without the dry run, it would have sent emails referencing wrong finding counts.

**Broken #3 — Calendar dropped invitees**
The native `create_calendar_event` tool drops the invitees list silently — creates the event on your calendar only. Found this in testing when checking the actual calendar entry. Fix: route calendar creation through an A2A call to a separate calendar agent that has the right permissions. Added 2 hops to what should be a simple API call.

**Broken #4 — 21 Tier 2 emails → 1**
First version of Agent 2 sent a separate risk email for each of the 21 Tier 2 (deadline risk) findings. The EPM (me) would have gotten 21 emails from my own agent. Fix: one combined Tier 2 email to EPM + lead covering all at-risk items. Same lesson as #1.

**Broken #5 — Demo radar mismatch**
Pre-staged the Agent 3 demo with two radar IDs that were in the V1/V2 migration tracking list, not in the AIS findings list. Agent 3 fetched them fine, but the ARC dashboard — which Agent 2 reads — didn't have those findings. The demo flow broke: Agent 2's report didn't reference the same items Agent 3 was processing. Fix: swap to radar IDs that exist in both the AIS findings list and the tracker.

---

## 4. What It Still Can't Do

- **Can't distinguish a real blocker from a forgotten update.** An engineer with 6 overdue findings might be blocked on Privacy approval (legitimate) or might just not have updated their ticket in three weeks (a nudge problem, not a blocker). The classification looks identical. The EPM decides.
- **Can't interpret organizational context.** Neeti's 9 Dhari findings targeting June 9 might be acceptable to AIS as post-certification remediation, or they might be a dealbreaker. The agent flags the risk; the EPM negotiates.
- **Can't send Slack.** No Slack connector existed at build time. The escalation path is email → calendar. In practice, most engineers respond faster to Slack. This is a platform gap, not an agent gap.

---

## 5. The Takeaway

> The agents didn't replace the decisions. They replaced the data collection and the routing. I spend less time finding out what's broken and more time deciding what to do about it.

---

## Meta notes

- Use the demo narration script from `docs/demo-readiness.md` as your personal voice reference — you wrote it, it's already in the right register
- The hackathon framing ("advanced to finals") is social proof — mention it
- Before/after stat: "manual daily loop ~45 min → agents run at 9 AM, EPM reviews exceptions only"
- The "3 agents, 6 prompts, 10 min demo" structure is a great article structure too
- Don't name the platform. "Issue tracker", "live dashboard", "calendar agent" — all generic
