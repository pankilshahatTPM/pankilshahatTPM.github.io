# S1/Article 4 — "Which EPM Tasks Are Worth Automating? Here's the Framework I Built."
**Series:** What I Built
**Source:** `epm-agent-skills-matrix.md`
**Target length:** 1,000–1,200 words
**LinkedIn/Medium friendly:** Yes — high shareability, practical framework

---

## Hook

> Before I built anything, I made a list of everything I do as a TPM. 15 recurring activities. Then I went through each one and asked: can an agent do this? Should it?
> Some of the answers surprised me.

---

## 1. The Matrix

**The 15 activities (generalized from `epm-agent-skills-matrix.md`):**

| # | Activity | Frequency |
|---|----------|-----------|
| 1 | Weekly status report | Weekly |
| 2 | Meeting action item extraction | After every sync |
| 3 | Issue tracker state updates | Ongoing |
| 4 | Blocker / risk identification | Weekly |
| 5 | Executive program health digest | Bi-weekly |
| 6 | OKR / KR progress tracking | Weekly |
| 7 | Compliance / infocheck tracking | Weekly |
| 8 | Meeting prep | Before every sync |
| 9 | Dependency mapping | Per milestone |
| 10 | Stakeholder communication draft | As needed |
| 11 | New ticket creation | As needed |
| 12 | Sprint / milestone planning | Per sprint |
| 13 | Post-meeting MOM generation | After every sync |
| 14 | Program onboarding doc | Per new program |
| 15 | Release / go-live checklist | Per release |

---

## 2. The Decision Framework

**Three questions for every activity:**

**Q1: Is the input structured?**
Does the agent have a reliable, queryable source for the inputs it needs? Issue tracker API = yes. Slack thread = maybe. Verbal conversation = no.

**Q2: Is the output evaluable?**
Can you tell in 30 seconds whether the agent got it right? A status report with wrong counts is obviously wrong. A stakeholder email with wrong tone is subtle.

**Q3: Is the judgment embedded or external?**
If the decision rule is "if state = Closed AND closedAt > 48h, hide it" — that's embeddable. If the decision rule is "escalate this one but not that one because of the org dynamics" — that's external. Agents handle embedded judgment well and external judgment poorly.

---

## 3. What I Built vs. What I Skipped

**Built (✅):**
- #1 Weekly status report → `/generate-program-status` — structured inputs, evaluable output, embedded classification rules
- #2 Meeting action item extraction → `/epm-meeting-process` — transcript in, structured MOM out
- #5 Executive health digest → same as #1
- #13 Post-meeting MOM → `/epm-meeting-process`

**Worth building (🔜):**
- #4 Blocker/risk digest — recurring, self-contained, doesn't need human input to generate
- #7 Compliance tracking — binary checks with clear inputs (approved or not, open or closed)
- #15 Release checklist — binary pass/fail at every milestone

**Skipped (⏭️):**
- #6 OKR tracking — redundant with #1. Same data, same output. One skill covers it.
- #8 Meeting prep — too input-dependent. Agenda varies too much per meeting to template well.
- #9 Dependency mapping — needs capacity + priority inputs from engineering leads. Agent can pull the data but can't make the call.
- #10 Stakeholder communication draft — too ad-hoc. Every message has different context.
- #12 Sprint planning — needs capacity inputs the agent doesn't have.

**Low value / guardrail only (⏭️):**
- #3 Tracker state updates — done ad-hoc via the tracker API. Could be a guardrail to enforce required fields, but not worth building as a standalone skill.
- #11 New ticket creation — same. The interesting use case isn't creation, it's enforcement (milestone set? priority set? assignee valid?).

---

## 4. The Maturity Levels

**Fill in your voice here — this is your L1/L2/L3 framework from the matrix:**

- **L1 — Assisted:** Agent drafts, human approves and sends. Status report, MOM draft.
- **L2 — Semi-automated:** Agent acts, human reviews exceptions only. Blocker digest, hygiene nudge.
- **L3 — Automated:** Agent runs end-to-end on schedule, human notified. Daily monitor, stale radar alerts.

Most skills start at L1. The trust builds over time. I promoted `/generate-program-status` to L3 (daily scheduled, no review) after 6 weeks of L1 runs with no errors.

---

## 5. What Agents Cannot Replace

**From the bottom of the matrix — fill in with your voice:**

- **Judgment calls** — escalation decisions, priority tradeoffs, stakeholder politics. The agent surfaces the data. You decide.
- **Relationship building** — trust with engineering leads, cross-org influence. No agent sends a message that lands the way you do after 6 months of working relationship.
- **Ambiguity resolution** — defining scope, making decisions when requirements conflict. The agent needs a clear input. When the input is "what should we even be tracking?", that's you.
- **Executive presence** — reading the room, adjusting communication in real time. The agent writes the draft. You know when to put it away.

---

## 6. The Takeaway

> The most valuable thing the matrix told me wasn't what to build — it was what to stop feeling guilty about not automating. Dependency mapping, sprint planning, stakeholder communication: those are the job. The rest is overhead.

---

## Meta notes

- This article stands alone well — it's the framework article that applies to any EPM, not just your specific build
- The before/after isn't a time stat here — it's a clarity stat: "I used to try to automate everything. Now I know which 4 out of 15 are worth it."
- High LinkedIn shareability: the 3-question framework + L1/L2/L3 model are tweetable/shareable on their own
- Link to the other three articles as "here's what I built from this framework"
