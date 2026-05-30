---
layout: null
title: "Your Job Is Disappearing. I Watched Mine Disappear Six Times."
permalink: /blog/your-job-is-disappearing-i-watched-mine-disappear-six-times/
date: 2026-05-28
---
<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>Your Job Is Disappearing. I Watched Mine Disappear Six Times.</title>
<style>
  :root {
    --bg: #0a0a0f;
    --surface: #12121a;
    --card: #1a1a26;
    --border: #2a2a3d;
    --text: #e8e8f0;
    --text-sec: #8888aa;
    --accent: #6c63ff;
    --accent2: #00d4aa;
    --accent3: #ff6b6b;
    --gold: #ffd700;
  }
  * { margin: 0; padding: 0; box-sizing: border-box; }
  body {
    font-family: -apple-system, BlinkMacSystemFont, "SF Pro Display", "Segoe UI", system-ui, sans-serif;
    background: var(--bg);
    color: var(--text);
    line-height: 1.6;
    min-height: 100vh;
  }

  /* ── HERO ── */
  .hero {
    background: linear-gradient(135deg, #0d0d1a 0%, #1a0d2e 50%, #0d1a1a 100%);
    padding: 80px 24px 64px;
    text-align: center;
    position: relative;
    overflow: hidden;
    border-bottom: 1px solid var(--border);
  }
  .hero::before {
    content: '';
    position: absolute;
    inset: 0;
    background: radial-gradient(ellipse at 30% 50%, rgba(108,99,255,.15) 0%, transparent 60%),
                radial-gradient(ellipse at 70% 30%, rgba(0,212,170,.1) 0%, transparent 50%);
  }
  .hero-tag {
    display: inline-block;
    background: rgba(108,99,255,.15);
    border: 1px solid rgba(108,99,255,.4);
    color: #a89fff;
    font-size: 11px;
    font-weight: 600;
    letter-spacing: 2px;
    text-transform: uppercase;
    padding: 6px 14px;
    border-radius: 20px;
    margin-bottom: 28px;
    position: relative;
  }
  .hero h1 {
    font-size: clamp(28px, 5vw, 52px);
    font-weight: 800;
    line-height: 1.15;
    max-width: 820px;
    margin: 0 auto 16px;
    position: relative;
    letter-spacing: -0.5px;
  }
  .hero h1 em {
    font-style: normal;
    background: linear-gradient(90deg, var(--accent), var(--accent2));
    -webkit-background-clip: text;
    -webkit-text-fill-color: transparent;
    background-clip: text;
  }
  .hero-sub {
    font-size: 18px;
    color: var(--text-sec);
    max-width: 600px;
    margin: 0 auto 32px;
    position: relative;
  }
  .hero-meta {
    font-size: 13px;
    color: var(--text-sec);
    position: relative;
  }
  .hero-meta span { color: var(--accent); }

  /* ── LAYOUT ── */
  .wrapper { max-width: 1100px; margin: 0 auto; padding: 0 24px; }

  /* ── OPENING QUOTE ── */
  .opening {
    padding: 56px 24px;
    text-align: center;
  }
  .opening blockquote {
    font-size: clamp(18px, 2.5vw, 24px);
    font-weight: 500;
    color: var(--text);
    max-width: 780px;
    margin: 0 auto;
    line-height: 1.55;
    border-left: 3px solid var(--accent);
    padding-left: 24px;
    text-align: left;
  }
  .opening cite {
    display: block;
    margin-top: 16px;
    font-size: 14px;
    color: var(--text-sec);
    font-style: normal;
  }

  /* ── SECTION LABEL ── */
  .section-label {
    text-align: center;
    font-size: 11px;
    font-weight: 700;
    letter-spacing: 3px;
    text-transform: uppercase;
    color: var(--accent);
    padding: 48px 0 24px;
  }

  /* ── TIMELINE ── */
  .timeline {
    padding: 0 24px 64px;
    position: relative;
  }
  .timeline-track {
    position: absolute;
    left: 50%;
    top: 0;
    bottom: 0;
    width: 2px;
    background: linear-gradient(to bottom, transparent, var(--border) 5%, var(--border) 95%, transparent);
    transform: translateX(-50%);
  }
  .stage {
    display: flex;
    align-items: flex-start;
    gap: 40px;
    margin-bottom: 48px;
    position: relative;
  }
  .stage:nth-child(even) { flex-direction: row-reverse; }
  .stage-content {
    flex: 1;
    background: var(--card);
    border: 1px solid var(--border);
    border-radius: 16px;
    padding: 28px 32px;
    position: relative;
    transition: border-color .2s, transform .2s;
  }
  .stage-content:hover {
    border-color: var(--accent);
    transform: translateY(-2px);
  }
  .stage-num {
    width: 56px;
    flex-shrink: 0;
    display: flex;
    flex-direction: column;
    align-items: center;
    padding-top: 28px;
    gap: 12px;
  }
  .num-circle {
    width: 40px;
    height: 40px;
    border-radius: 50%;
    display: flex;
    align-items: center;
    justify-content: center;
    font-size: 14px;
    font-weight: 800;
    border: 2px solid;
    position: relative;
    z-index: 1;
    background: var(--bg);
  }
  .stage-label-pill {
    font-size: 10px;
    font-weight: 700;
    letter-spacing: 1.5px;
    text-transform: uppercase;
    padding: 3px 8px;
    border-radius: 10px;
    white-space: nowrap;
  }

  /* Stage colors */
  .s1 .num-circle { color: #6c63ff; border-color: #6c63ff; }
  .s2 .num-circle { color: #00d4aa; border-color: #00d4aa; }
  .s3 .num-circle { color: #ff9f43; border-color: #ff9f43; }
  .s4 .num-circle { color: #ff6b6b; border-color: #ff6b6b; }
  .s5 .num-circle { color: #a29bfe; border-color: #a29bfe; }
  .s6 .num-circle { color: #ffd700; border-color: #ffd700; }

  .s1 .stage-label-pill { background: rgba(108,99,255,.15); color: #a89fff; }
  .s2 .stage-label-pill { background: rgba(0,212,170,.15); color: #00d4aa; }
  .s3 .stage-label-pill { background: rgba(255,159,67,.15); color: #ff9f43; }
  .s4 .stage-label-pill { background: rgba(255,107,107,.15); color: #ff6b6b; }
  .s5 .stage-label-pill { background: rgba(162,155,254,.15); color: #a29bfe; }
  .s6 .stage-label-pill { background: rgba(255,215,0,.15); color: #ffd700; }

  .stage-era {
    font-size: 11px;
    font-weight: 600;
    letter-spacing: 1px;
    text-transform: uppercase;
    color: var(--text-sec);
    margin-bottom: 8px;
  }
  .stage-title {
    font-size: 20px;
    font-weight: 700;
    margin-bottom: 12px;
    line-height: 1.3;
  }
  .stage-body {
    font-size: 14px;
    color: var(--text-sec);
    line-height: 1.65;
    margin-bottom: 16px;
  }
  .stage-shift {
    display: flex;
    gap: 8px;
    align-items: flex-start;
    background: rgba(255,255,255,.04);
    border-radius: 10px;
    padding: 12px 14px;
    border-left: 3px solid;
  }
  .s1 .stage-shift { border-color: #6c63ff; }
  .s2 .stage-shift { border-color: #00d4aa; }
  .s3 .stage-shift { border-color: #ff9f43; }
  .s4 .stage-shift { border-color: #ff6b6b; }
  .s5 .stage-shift { border-color: #a29bfe; }
  .s6 .stage-shift { border-color: #ffd700; }

  .shift-label {
    font-size: 10px;
    font-weight: 700;
    letter-spacing: 1.2px;
    text-transform: uppercase;
    white-space: nowrap;
    padding-top: 2px;
  }
  .s1 .shift-label { color: #6c63ff; }
  .s2 .shift-label { color: #00d4aa; }
  .s3 .shift-label { color: #ff9f43; }
  .s4 .shift-label { color: #ff6b6b; }
  .s5 .shift-label { color: #a29bfe; }
  .s6 .shift-label { color: #ffd700; }

  .shift-text {
    font-size: 13px;
    color: var(--text);
    line-height: 1.5;
  }

  /* ── BEFORE/AFTER ── */
  .before-after {
    display: flex;
    gap: 8px;
    margin-bottom: 14px;
    flex-wrap: wrap;
  }
  .ba-pill {
    font-size: 11px;
    font-weight: 600;
    padding: 4px 10px;
    border-radius: 6px;
    display: flex;
    align-items: center;
    gap: 6px;
  }
  .ba-was { background: rgba(255,107,107,.12); color: #ff8080; }
  .ba-now { background: rgba(0,212,170,.12); color: #00d4aa; }
  .ba-arrow { color: var(--text-sec); font-size: 12px; }

  /* ── CONCLUSION ── */
  .conclusion {
    padding: 0 24px 80px;
  }
  .conclusion-card {
    background: linear-gradient(135deg, rgba(108,99,255,.1), rgba(0,212,170,.08));
    border: 1px solid rgba(108,99,255,.3);
    border-radius: 20px;
    padding: 48px;
    text-align: center;
    max-width: 900px;
    margin: 0 auto;
    position: relative;
    overflow: hidden;
  }
  .conclusion-card::before {
    content: '';
    position: absolute;
    top: -60px; right: -60px;
    width: 200px; height: 200px;
    border-radius: 50%;
    background: radial-gradient(circle, rgba(108,99,255,.15), transparent 70%);
  }
  .conclusion-icon { font-size: 40px; margin-bottom: 20px; }
  .conclusion-card h2 {
    font-size: clamp(22px, 3vw, 32px);
    font-weight: 800;
    margin-bottom: 20px;
    line-height: 1.25;
  }
  .conclusion-card p {
    font-size: 16px;
    color: var(--text-sec);
    max-width: 640px;
    margin: 0 auto 28px;
    line-height: 1.7;
  }
  .three-truths {
    display: flex;
    gap: 16px;
    justify-content: center;
    flex-wrap: wrap;
    margin-top: 8px;
  }
  .truth {
    background: rgba(255,255,255,.05);
    border: 1px solid var(--border);
    border-radius: 12px;
    padding: 16px 20px;
    flex: 1;
    min-width: 180px;
    max-width: 220px;
    font-size: 13px;
    color: var(--text);
    line-height: 1.5;
  }
  .truth strong { display: block; font-size: 11px; text-transform: uppercase; letter-spacing: 1px; color: var(--accent); margin-bottom: 6px; }

  /* ── FOOTER ── */
  .footer {
    text-align: center;
    padding: 24px;
    border-top: 1px solid var(--border);
    font-size: 12px;
    color: var(--text-sec);
  }
  .footer a { color: var(--accent); text-decoration: none; }

  /* ── RESPONSIVE ── */
  @media (max-width: 700px) {
    .timeline-track { display: none; }
    .stage, .stage:nth-child(even) { flex-direction: column; }
    .stage-num { flex-direction: row; padding-top: 0; margin-bottom: 12px; }
    .conclusion-card { padding: 32px 20px; }
    .three-truths { flex-direction: column; align-items: center; }
  }
</style>
</head>
<body>

<!-- HERO -->
<div class="hero">
  <div class="hero-tag">EPM / TPM Perspective</div>
  <h1>Your Job Is Disappearing.<br><em>I Watched Mine Disappear Six Times.</em></h1>
  <p class="hero-sub">Why automation always reveals the project management work you previously couldn't see.</p>
  <div class="hero-meta">By <span>Pankil Shah</span> &nbsp;·&nbsp; May 2026 &nbsp;·&nbsp; 7 min read</div>
</div>

<!-- OPENING QUOTE -->
<div class="opening">
  <div class="wrapper">
    <blockquote>
      "The job of an Engineering Project Manager was never a single task. We get paid to align human intent, manage risk, and ship complex systems. The specific mechanics keep changing underneath us."
      <cite>— Pankil Shah</cite>
    </blockquote>
  </div>
</div>

<!-- TIMELINE -->
<div class="wrapper">
  <div class="section-label">The Six Disappearances</div>
</div>

<div class="wrapper">
  <div class="timeline">

    <!-- Stage 1 -->
    <div class="stage s1">
      <div class="stage-num">
        <div class="num-circle">1</div>
        <div class="stage-label-pill">Manual</div>
      </div>
      <div class="stage-content">
        <div class="stage-era">Early Software Era</div>
        <div class="stage-title">Tracking Status by Hand</div>
        <p class="stage-body">You chased engineers for updates. You manually copied data from spreadsheets into PowerPoint decks. Fifty moving parts meant fifty pings. Modern software suites wiped this out — a developer closing a PR now moves the ticket automatically.</p>
        <div class="before-after">
          <span class="ba-pill ba-was">Was: Human router for data</span>
          <span class="ba-arrow">→</span>
          <span class="ba-pill ba-now">Now: Automated workflows</span>
        </div>
        <div class="stage-shift">
          <span class="shift-label">Shift →</span>
          <span class="shift-text">From collecting status to acting on it. The scale of modern cross-functional engineering outran the manual technique.</span>
        </div>
      </div>
    </div>

    <!-- Stage 2 -->
    <div class="stage s2">
      <div class="stage-num">
        <div class="num-circle">2</div>
        <div class="stage-label-pill">Scoping</div>
      </div>
      <div class="stage-content">
        <div class="stage-era">Requirements Era</div>
        <div class="stage-title">Defining Scope: WBS & Requirements</div>
        <p class="stage-body">Breaking a product vision into a Work Breakdown Structure ate up weeks. Today, an AI-native workflow can take a raw PRD and output a comprehensive WBS with initial sizing in minutes.</p>
        <div class="before-after">
          <span class="ba-pill ba-was">Was: Weeks of grueling meetings</span>
          <span class="ba-arrow">→</span>
          <span class="ba-pill ba-now">Now: AI-drafted in minutes</span>
        </div>
        <div class="stage-shift">
          <span class="shift-label">Shift →</span>
          <span class="shift-text">From formatting tickets to asking bigger questions: Is this the right architecture? Are we solving for the right constraints?</span>
        </div>
      </div>
    </div>

    <!-- Stage 3 -->
    <div class="stage s3">
      <div class="stage-num">
        <div class="num-circle">3</div>
        <div class="stage-label-pill">Execution</div>
      </div>
      <div class="stage-content">
        <div class="stage-era">SDLC Era</div>
        <div class="stage-title">Managing Execution: Driving the SDLC</div>
        <p class="stage-body">PMs were the gatekeepers of the release train — coordinating code freezes, running triage meetings, signing off on deployment checklists. CI/CD pipelines and intelligent triaging now handle the execution mechanics.</p>
        <div class="before-after">
          <span class="ba-pill ba-was">Was: Hand-managing release checklists</span>
          <span class="ba-arrow">→</span>
          <span class="ba-pill ba-now">Now: Architecting the delivery process</span>
        </div>
        <div class="stage-shift">
          <span class="shift-label">Shift →</span>
          <span class="shift-text">Compliance, security, and velocity are now built into the platform engineering lifecycle — not checked manually at the end.</span>
        </div>
      </div>
    </div>

    <!-- Stage 4 -->
    <div class="stage s4">
      <div class="stage-num">
        <div class="num-circle">4</div>
        <div class="stage-label-pill">Risk</div>
      </div>
      <div class="stage-content">
        <div class="stage-era">Microservices Era</div>
        <div class="stage-title">Mitigating Risk: The Dependency Problem</div>
        <p class="stage-body">In the move to distributed microservices, the hardest EPM skill was managing cross-team dependencies. You hand-rolled alignment, set up recurring syncs, built risk registers, and negotiated timelines between siloed orgs.</p>
        <div class="before-after">
          <span class="ba-pill ba-was">Was: "When will the API be ready?"</span>
          <span class="ba-arrow">→</span>
          <span class="ba-pill ba-now">Now: How do we govern the ecosystem?</span>
        </div>
        <div class="stage-shift">
          <span class="shift-label">Shift →</span>
          <span class="shift-text">When the cost of spinning up a new service drops to zero, the volume of services explodes. The frontier moves from tracking dates to managing systemic architectural health.</span>
        </div>
      </div>
    </div>

    <!-- Stage 5 -->
    <div class="stage s5">
      <div class="stage-num">
        <div class="num-circle">5</div>
        <div class="stage-label-pill">Alignment</div>
      </div>
      <div class="stage-content">
        <div class="stage-era">Analytics Era</div>
        <div class="stage-title">Stakeholder Alignment: The Data Layer</div>
        <p class="stage-body">The most bureaucratic work: defending project health to leadership committees, reading metrics by hand, sitting through alignment sessions begging for headcount from executive priesthoods who guarded resources.</p>
        <div class="before-after">
          <span class="ba-pill ba-was">Was: Building the chart manually</span>
          <span class="ba-arrow">→</span>
          <span class="ba-pill ba-now">Now: Interpreting it strategically</span>
        </div>
        <div class="stage-shift">
          <span class="shift-label">Shift →</span>
          <span class="shift-text">Data is now democratized. Leadership sees real-time health metrics without a PM building the deck. You own the narrative, the edge cases, and the strategy when things go wrong at scale.</span>
        </div>
      </div>
    </div>

    <!-- Stage 6 -->
    <div class="stage s6">
      <div class="stage-num">
        <div class="num-circle">6</div>
        <div class="stage-label-pill">AI Era</div>
      </div>
      <div class="stage-content">
        <div class="stage-era">Now &amp; Next</div>
        <div class="stage-title">Agentic Delivery: The Current Disappearance</div>
        <p class="stage-body">AI can now generate project plans, track dependencies, write status reports from Slack history, and synthesize risk from code commits. If your job is defined as a collection of administrative tasks — you are right to be nervous.</p>
        <div class="before-after">
          <span class="ba-pill ba-was">Was: Mechanical PM tasks</span>
          <span class="ba-arrow">→</span>
          <span class="ba-pill ba-now">Now: Execution judgment</span>
        </div>
        <div class="stage-shift">
          <span class="shift-label">Shift →</span>
          <span class="shift-text">If your job is <em>solving operational and delivery complexity</em>, the reality is entirely different. Tasks run out. Complexity never does.</span>
        </div>
      </div>
    </div>

  </div>
</div>

<!-- FULL ESSAY SECTION -->
<style>
  .essay-container {
    max-width: 720px;
    margin: 0 auto;
    padding: 0 24px 64px;
    color: var(--text);
  }
  .essay-divider {
    height: 1px;
    background: var(--border);
    margin: 48px 0;
    position: relative;
    text-align: center;
  }
  .essay-divider::after {
    content: '◆';
    color: var(--accent);
    background: var(--bg);
    padding: 0 16px;
    position: absolute;
    top: 50%; left: 50%;
    transform: translate(-50%, -50%);
    font-size: 14px;
  }
  .essay-title {
    font-size: 24px;
    font-weight: 700;
    margin-bottom: 24px;
    color: var(--text);
    text-align: center;
  }
  .essay-content p {
    font-size: 16px;
    line-height: 1.8;
    color: var(--text-sec);
    margin-bottom: 24px;
  }
  .essay-content p strong {
    color: var(--text);
  }
  .essay-content h3 {
    font-size: 20px;
    font-weight: 700;
    margin: 40px 0 16px;
    color: var(--text);
  }
</style>

<div class="essay-container" id="full-essay">
  <div class="essay-divider"></div>
  <h2 class="essay-title">The Complete Essay</h2>
  <div class="essay-content">
    <p>From the mid-20th century until well past the dawn of the internet, a massive portion of project management was administrative. Armies of project coordinators sat in windowless rooms, manually updating physical Gantt charts with colored tape, cold-calling engineers for status updates, and compiling massive paper binders for monthly steering committee reviews.</p>
    
    <p>Then, desktop scheduling software arrived, followed by cloud-based issue trackers. The physical boards were wiped out. It’s the classic story of technology erasing a job, and it’s the story everyone’s afraid of with AI. The fear is everywhere. When you see AI engines instantly generating project plans, tracking dependencies, and writing status reports from Slack history, you can’t help but think that the thing you’re doing is going away.</p>
    
    <p>If your job is defined as a collection of administrative tasks, you are right to be nervous. But if your job is solving operational and delivery complexity, the reality is entirely different. The job of an Engineering Project Manager (EPM) or Technical Program Manager (TPM) was never a single task. We get paid to align human intent, manage risk, and ship complex systems. The specific mechanics keep changing underneath us. Here is how the project management layer has evolved, and where it is going next.</p>

    <h3>1. Tracking Status by Hand</h3>
    <p>Early in the software era, project management was fiercely manual. You chased engineers down to get status updates. You manually copied data from a spreadsheet into a PowerPoint deck for leadership. If a project had fifty moving parts, you had to manually ping fifty people. When a deadline was missed, you were the one updating individual date fields across a massive, brittle schedule.</p>
    
    <p>Within a decade, those tasks began to evaporate. Modern software suites introduced automated workflows. A developer closing a pull request automatically moved the ticket and updated the burn-down chart. Status tracking became a byproduct of actual engineering work rather than a separate bureaucratic tax. There are still project managers, but they don’t spend their days playing human router for data. The scale of modern cross-functional engineering outran the manual technique.</p>

    <h3>2. Defining Scope: The WBS and Requirements Gathering</h3>
    <p>There was a time when breaking a massive product vision down into a Work Breakdown Structure (WBS) or an Epic-and-Story hierarchy ate up weeks of administrative time. You sat in grueling meetings, pulling requirements out of stakeholders, trying to translate ambiguous product statements into clean, actionable technical milestones.</p>
    
    <p>Today, that boilerplate scoping is collapsing into the tooling. With modern AI-native delivery, you can feed a raw product requirements document into an agent framework and watch it output a comprehensive WBS, assign initial sizing, and draft technical tasks in minutes.</p>
    
    <p>The part that surprises people is how much of what we thought of as "project management skill" was just working around primitive tools. The meticulous formatting of tickets and structural mapping was the toll you paid. Better tools eliminate the toll, freeing up the manager to ask the bigger questions: <strong>Is this the right architecture to solve the business problem? Are we solving for the right constraints? Do the teams actually understand the "why" behind the code?</strong></p>

    <h3>3. Managing Execution: Driving the SDLC</h3>
    <p>Guiding a project through the Software Development Life Cycle (SDLC) used to require heavy operational babysitting. Project managers were the gatekeepers of the release train. You coordinated code freezes, ran triage meetings to manually deduplicate bugs, and signed off on deployment checklists.</p>
    
    <p>Now, the SDLC is shifting from human-authored tracking to agent-observed governance. Continuous Integration and Continuous Deployment (CI/CD) pipelines, automated testing, and intelligent error triaging handle the execution mechanics. The project manager who used to hand-manage release checklists now spends their time architecting the delivery process itself—ensuring that compliance, security, and velocity are built into the platform engineering lifecycle rather than checked manually at the end.</p>

    <h3>4. Mitigating Risk: The Distributed Dependency Problem</h3>
    <p>In the era of massive monolithic systems moving to distributed microservices, the hardest-won knowledge for an EPM was managing cross-team dependencies. If Team A needed an API from Team B, you hand-rolled the alignment logic. You set up recurring synchronization meetings, built risk registers, and negotiated timelines between siloed engineering orgs.</p>
    
    <p>One by one, platforms solved the plumbing. Shared data platforms, atomic APIs, and service meshes made it easier for systems to decouple. The dependency-wrangler didn't disappear; they moved up to subtler problems. When the cost of spinning up a new service drops to near zero, the volume of services explodes. The question is no longer "When will the API be ready?" Instead, it becomes: <strong>How do we govern this massive, sprawling ecosystem? How do we maintain data lineage and translation workflows across global boundaries?</strong> The frontier moves from tracking dates to managing systemic architectural health.</p>

    <h3>5. Stakeholder Alignment: The Data Layer of Leadership</h3>
    <p>The most bureaucratic work a project manager used to do was defending the project's health to leadership committees. You read through metrics by hand to explain a slip in velocity. You sat through alignment sessions begging for resource allocations from executive priesthoods who guarded headcount.</p>
    
    <p>That ground has shifted. Automated analytics dashboards and portfolio management tools mean data is democratized. Leadership can see real-time health metrics, financial burns, and predictive completion lines without needing a project manager to manually build the chart. The bureaucratic formality has dissolved. Because the data is transparent, the EPM’s role shifts entirely from reporting the data to interpreting it. You own the narrative, the edge cases, and the strategy for when a complex design decision goes wrong at scale.</p>
  </div>
</div>

<!-- CONCLUSION -->
<div class="conclusion">
  <div class="wrapper">
    <div class="conclusion-card">
      <div class="conclusion-icon">⚡</div>
      <h2>The Premium on Judgment Only Goes Up</h2>
      <p>Every time a manual task gets automated away, the nervous question is the same: <em>What will I do with my time now?</em> The answer is always the same — more impactful work, harder than before.</p>
      <div class="three-truths">
        <div class="truth">
          <strong>What gets automated</strong>
          Scheduling, ticket routing, basic status synthesis — anything with a known answer.
        </div>
        <div class="truth">
          <strong>What stays human</strong>
          Execution judgment, cross-functional empathy, strategic negotiation under real constraints.
        </div>
        <div class="truth">
          <strong>What this means</strong>
          Dive deep into technical architecture. Understanding is the raw material of judgment.
        </div>
      </div>
    </div>
  </div>
</div>

<!-- FOOTER -->
<div class="footer">
  By Pankil Shah &nbsp;·&nbsp; <a href="#full-essay">Read full essay</a> &nbsp;·&nbsp; Adapted from Steve Huynh's essay on software engineering role evolution
</div>

</body>
</html>
