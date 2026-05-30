# S1/Article 2 — DRAFT v1
# "One Config File to Provision an Entire Engineering Program"
**Status:** First draft — no [YOUR VOICE] markers, fully derived from the repo
**Word count:** ~1,400

---

After building the status automation for Secure Endor, I picked up a second program. Then a third.

Every time, I spent the first two weeks doing the same setup: create the issue tracker hierarchy, build the dashboard, configure the status email, set up the weekly nudge, wire up the escalation rules. None of that work was the program. It was infrastructure. And I was rebuilding it from scratch each time, with slightly different field conventions, slightly different component names, slightly different email lists.

By the third program I had a clear picture of the real problem: I was solving the same problem repeatedly without a system. So I built the system.

---

## What Every Program Needs

Strip away the domain-specific details and every engineering program an EPM manages needs the same six things:

**1. An issue tracker hierarchy**
One top-level umbrella → area umbrellas → feature tracks. Without the hierarchy, all tickets are flat. The dashboard can't break down health by area. The monitor can't identify which component is at risk.

**2. A live engineering dashboard**
A report that shows every track's state — SLIP, DUE SOON, ON TRACK, NO DATE — updated daily. The kind of view the native tracker doesn't give you: all your items, classified, in one place.

**3. A daily status sync**
A skill that queries the tracker, classifies every open track against the active milestone, and pushes results to the dashboard. Runs at 8:50 AM on a schedule. Doesn't ask for permission.

**4. A daily escalation monitor**
A skill that reads the dashboard (already populated by the sync), identifies SLIP tracks and P1 items with no committed date, sends one batched email per DRI, and creates calendar blocks for the EPM on at-risk areas. Runs at 9:00 AM.

**5. A weekly hygiene nudge**
The tracker sync only works if ticket fields are populated. Every Monday morning, a skill queries the tracker directly — not the dashboard — for P1 and P2 items with no sprint/event date set. Sends one batched email per engineer: "you have 3 tickets with no committed date, here are the links." The upstream gate that keeps the tracker accurate.

**6. A DRI reply processor**
When an engineer replies to an escalation email — DONE, BLOCKED, or ACK — the EPM pastes the reply into the skill. It parses the intent, updates the tracker ticket, logs to the dashboard, and (on BLOCKED only) emails the EPM and creates a calendar block.

Six things. Every program. Every time.

---

## The Config File

The EPM Engineering Tracker represents all six as a YAML config file:

```yaml
program:
  name: "Guardrails"
  component:
    name: "Endor"
    version: "Guardrails"

people:
  epm_email: "pm@company.com"
  eng_manager_email: "manager@company.com"
  eng_lead_email: "lead@company.com"

areas:
  - name: "Policy / RAI"
    umbrella_radar_ids: [174624818]
  - name: "Guard Models"
    umbrella_radar_ids: [177402106]
  - name: "Platform"
    umbrella_radar_ids: [177402150]

arc:
  engineering_tracker_report_id: "report-1779149198304"

escalation:
  slip:
    to: [dri_lead, direct_manager]
    cc: [epm]
  blocked:
    to: [epm]
    cc: []

cadence:
  generate_program_status:
    schedule: "daily"
    time: "08:50"
  program_track_monitor:
    schedule: "daily"
    time: "09:00"
  program_hygiene_nudge:
    schedule: "weekly"
    day: "Monday"
    time: "08:00"
```

One file. All six behaviors fall out of it.

Most fields are optional — EPM name, engineering lead email, and component ID are all auto-resolved from the tracker's component API at runtime. The config only needs what the tracker can't figure out on its own.

---

## The Bootstrap Command

Once the config exists, one command provisions everything:

```
/setup-engineering-tracker config=guardrails
```

The skill runs three environment checks first (tracker MCP connected, dashboard MCP connected, ARC skills installed). If any fail, it surfaces the exact fix and stops. No partial provisioning.

Then in sequence:
1. Creates the top-level program umbrella in the issue tracker
2. Creates one area umbrella per area in the config
3. Provisions the ARC Engineering Tracker report with the right permissions
4. Creates the Quip guidelines doc with the program context pre-filled
5. Writes all generated IDs (umbrella radar IDs, report ID) back to the config file

After setup, the config is self-contained. Every subsequent skill run reads only the config — no manual ID management needed.

---

## The Config Builder

Not everyone wants to hand-edit YAML. The toolkit includes a web-based config builder: a static HTML form that asks for the program name, component, areas, team emails, and ARC report IDs, then generates the filled-out YAML. One copy-paste into `~/.claude/skills/configs/<program>.yaml` and the setup command is ready to run.

The config builder is the onboarding surface — the thing you hand to another EPM who wants to set up their own tracker in an afternoon instead of two weeks.

---

## What Broke During the Build

**Component API format**
The tracker API accepts component as `{"name": "Foo", "version": "Bar"}`. The UI displays it as "Foo | Bar". These look equivalent. The API rejects the display format with a validation error. Every config that uses the pipe-separated display name fails silently at provisioning time. Fixed by enforcing the name/version split in the config schema and the skill's API calls.

**Umbrella radar distinction**
The status sync queries `isUmbrella: false` — it only returns feature tracks, not the hierarchy nodes. But when the bootstrap creates the umbrella radars, they need to be explicitly flagged as umbrellas or they appear in the track count. The API doesn't infer this. Fixed by passing `isUmbrella: true` on every umbrella creation call in the setup skill.

**Resolution field on state transition**
Moving a radar from Analyze state via the API requires `"resolution": "Software Changed"`. Tried: "Fixed", "Code Fix", "Software Fixed", "Fix Submitted" — all return `CV.enumFieldNotValid`. The valid value isn't in the documentation. Found by inspecting an existing radar already in Verify state.

**FabCloud PR requirement**
Any tracker radar created as a code review reference fails CI if milestone or priority is missing. The error message from the hook is generic — it doesn't say which field. Fixed by always setting milestone and priority on any programmatically created radar, regardless of whether the program currently needs them.

**EPM and engineering lead email auto-resolution**
Both are auto-resolved from the tracker's component API at runtime — no manual input required. But the component API only exposes this if the component has an EPM and owner assigned in the tracker. First-time setup on a new component with no roles set: both fields return null, the skill errors on the email step. Fixed by adding a null check and surfacing a clear error: "component has no EPM assigned in Radar — add one or override in the config."

---

## What It Still Can't Do

**It can't validate business logic.**
If the config assigns the wrong person as engineering lead, or uses a milestone that doesn't match the team's actual sprint calendar, the provisioner succeeds. The tracker hierarchy is correct. Whether it's tracking the right things is a separate judgment.

**It can't replace the kick-off conversation.**
The config says what areas exist. It doesn't say what the areas mean, what "done" looks like for each track, or what the escalation threshold is in practice. The scaffolding takes two minutes. The alignment takes a week of conversations with engineering leads.

**It can't adapt to mid-program scope changes.**
If an area gets added or renamed mid-program, the config needs to be updated and the bootstrap re-run for the new area. The existing areas are unaffected but the new hierarchy doesn't create itself.

---

## The Numbers

- Programs supported by the same toolkit: currently Guardrails + Secure Endor, extensible to any program with a tracker component
- Setup time: 5 minutes with the config builder, vs. ~2 hours manual
- Skills provisioned per program: 6 (sync, monitor, hygiene nudge, commitments, reply processor, setup)
- Config fields that auto-resolve from the tracker at runtime: component ID, EPM email, engineering lead email, active milestones, sprint/event IDs

---

## Why This Exists

The first version of every tool I built for Secure Endor was hardcoded to that program. Radar IDs, email addresses, component names — all literal values in the skill files. It worked. It also meant every new program needed a new skill rewrite.

The engineering tracker reifies what I learned the hard way: the logic is the same across programs. The config is what changes. Separating them means the next EPM who picks up a program doesn't spend two weeks on infrastructure. They spend twenty minutes on a form and twenty minutes on the bootstrap command.

The lessons from Article 1 are already in the code.

---

*Next in this series: [Article 3 — I Built Three Agents to Run My Security Program. Here's What Actually Happened.]*
