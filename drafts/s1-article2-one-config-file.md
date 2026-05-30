# S1/Article 2 — "One Config File to Provision an Entire Engineering Program"
**Series:** What I Built
**Source repo:** `epm-engineering-tracker`
**Target length:** 1,000–1,300 words
**LinkedIn/Medium friendly:** Yes

---

## Hook

> Every new program I picked up started the same way: two weeks of setup. Create the issue tracker hierarchy. Build the dashboard. Set up the Quip doc. Configure the status email. Wire up the skills. By the time the scaffolding was done, the program was already behind.
> So I built a provisioner. One YAML file. One command. Everything spins up in under 5 minutes.

---

## 1. The Problem

**Fill in your voice here. Prompts:**
- Walk through what "setting up a new program" actually looked like before. Be specific about each step and how long each took.
- How many times did you do this? (You can reference: Guardrails tracker, Secure Endor tracker, others)
- What broke when you did it manually? (Wrong component name, wrong assignee, missing milestone fields)

**Real structure to anchor:**
Every program needs:
1. Issue tracker umbrella hierarchy (1 parent → N area umbrellas → M track radars)
2. Live dashboard (ARC report) with state transformations
3. Status doc (Quip) with program context
4. Skills configured and scheduled (`/generate-program-status`, `/program-track-monitor`, `/program-hygiene-nudge`, `/generate-program-commitments`)
5. Email recipients list
6. DRI assignment mapping

Manual: ~2 hours minimum. Repeated for every program.

---

## 2. What I Built

**The config file (generalized):**

```yaml
program_name: "My Program"
component:
  name: "My Team"
  version: "My Program"
areas:
  - name: "Authentication"
    tracks: ["Track A", "Track B"]
  - name: "Data Pipeline"
    tracks: ["Track C"]
skills:
  - generate-program-status
  - program-track-monitor
  - program-hygiene-nudge
email_recipients:
  - dri_lead@company.com
  - pm@company.com
```

One command runs `/setup-engineering-tracker config=my-program`:
1. Creates the full Radar hierarchy (umbrella + area umbrellas + track radars)
2. Provisions the ARC dashboard with the right template and transformations
3. Creates the Quip status doc pre-populated with program context
4. Configures and schedules all skills
5. Sends a confirmation with links to everything

**The Config Builder UI:**
Rather than hand-editing YAML, built a web-based config builder (static HTML, no backend) that generates the correct YAML from a form. Anyone on the team can set up a new program without touching a config file directly.

---

## 3. What Broke

**Broken #1 — Component API format**
The issue tracker's API accepts component in two formats: `{"name": "Foo | Bar"}` (display format) and `{"name": "Foo", "version": "Bar"}` (API format). These look equivalent but behave differently. Using the display format returns `CV.componentDoesNotExist`. The correct format is always `name` + `version` as separate fields. Lost an hour to this.

**Broken #2 — Umbrella radar distinction**
The status query is `isUmbrella: false` — it only returns feature track radars. But when the provisioner created umbrella radars, they needed to be flagged as umbrellas explicitly or they'd appear in the status count. The API doesn't auto-flag them. Fix: pass `isUmbrella: true` on every umbrella radar creation call.

**Broken #3 — Keyword IDs vs. names**
Adding a program keyword tag to findings via the API requires the numeric keyword ID, not the keyword name. `{"keywords": {"insert": [{"name": "My Program"}]}}` returns `VK.invalidObjectFormat`. Only `{"keywords": {"insert": [{"id": 1234567}]}}` works. The ID has to be looked up from an existing tagged finding before the first run.

**Broken #4 — Resolution field on state transition**
Moving a radar from Analyze state via the API requires passing `"resolution": "Software Changed"`. Tried: "Fixed", "Code Fix", "Software Fixed", "Fix Submitted" — all return `CV.enumFieldNotValid`. The valid value isn't documented anywhere obvious.

**Broken #5 — FabCloud PR requirements**
Any radar created as a code review reference requires both a milestone and a priority field set, or the CI hook rejects the PR with no explanation. First 3 PRs failed. Fix: always set milestone + priority on any programmatically created tracker radar.

---

## 4. What It Still Can't Do

- Can't validate the business logic of the config — if you assign the wrong person or the wrong milestone, it provisions correctly but incorrectly.
- Can't detect when a track's scope has drifted from its original definition. The hierarchy is correct; whether it's tracking the right things is a judgment call.
- Can't replace the kick-off conversation with engineering leads. The scaffolding is right; the alignment is still human.

---

## 5. The Takeaway

> The 2-hour setup wasn't the hard part — the inconsistency was. Every manual setup had slightly different component names, different field conventions, different email lists. The config file enforced the standard. Now every program looks the same to the agents.

---

## Meta notes

- Include the before/after: "2 hours manual setup, repeated per program → 5 minutes, consistent every time"
- The config builder UI screenshot would be great here — shows craft, not just code
- The "What Broke" API details are highly specific and credible — don't cut them
- Link to the live Config Builder at the end
