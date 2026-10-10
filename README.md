# Study-plan-website
An agentic, multimodal AI-driven study companion built on the IBM Tech Stack that autonomously fuses scattered learning resources, maps concepts, and provides proactive exam readiness alerts.
!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0"/>
  <title>Study Planner Dashboard</title>
  <style>
    *, *::before, *::after { box-sizing: border-box; margin: 0; padding: 0; }

    :root {
      --sage:       #a8c5a0;
      --sage-light: #d4e8d0;
      --sage-pale:  #eef6ec;
      --blue:       #8bafd4;
      --blue-light: #c8ddf0;
      --blue-pale:  #eaf3fb;
      --cream:      #f5f0e8;
      --cream-dark: #e8dfc8;
      --lavender:   #c3b8e0;
      --lav-pale:   #f0edf9;
      --peach:      #e8b89a;
      --peach-pale: #fdf0e8;
      --bg:         #f8f7f4;
      --surface:    #ffffff;
      --border:     #e8e4dc;
      --text:       #2c2a26;
      --muted:      #7a7570;
      --accent:     #5e8c6e;
    }

    body {
      font-family: -apple-system, "Segoe UI", system-ui, sans-serif;
      background: var(--bg);
      color: var(--text);
      display: flex;
      min-height: 100vh;
      font-size: 14px;
      line-height: 1.6;
    }

    /* ── Sidebar ── */
    .sidebar {
      width: 220px;
      min-width: 220px;
      background: var(--surface);
      border-right: 1px solid var(--border);
      display: flex;
      flex-direction: column;
      padding: 28px 0 24px;
      position: sticky;
      top: 0;
      height: 100vh;
    }

    .sidebar-logo {
      padding: 0 22px 28px;
      border-bottom: 1px solid var(--border);
      margin-bottom: 20px;
    }
    .sidebar-logo .logo-icon {
      width: 36px; height: 36px;
      background: var(--sage);
      border-radius: 10px;
      display: flex; align-items: center; justify-content: center;
      font-size: 18px;
      margin-bottom: 8px;
    }
    .sidebar-logo h2 {
      font-size: 0.95rem;
      font-weight: 700;
      color: var(--text);
      letter-spacing: -0.3px;
    }
    .sidebar-logo p {
      font-size: 0.72rem;
      color: var(--muted);
      margin-top: 1px;
    }

    .nav-section { padding: 0 12px; margin-bottom: 8px; }
    .nav-label {
      font-size: 0.65rem;
      font-weight: 700;
      letter-spacing: 0.08em;
      text-transform: uppercase;
      color: var(--muted);
      padding: 0 10px;
      margin-bottom: 6px;
    }

    .nav-item {
      display: flex;
      align-items: center;
      gap: 10px;
      padding: 9px 12px;
      border-radius: 9px;
      cursor: pointer;
      font-size: 0.86rem;
      font-weight: 500;
      color: var(--muted);
      transition: background 0.15s, color 0.15s;
      margin-bottom: 2px;
      user-select: none;
    }
    .nav-item:hover { background: var(--bg); color: var(--text); }
    .nav-item.active {
      background: var(--sage-pale);
      color: var(--accent);
      font-weight: 600;
    }
    .nav-item .icon {
      width: 28px; height: 28px;
      border-radius: 7px;
      display: flex; align-items: center; justify-content: center;
      font-size: 14px;
      flex-shrink: 0;
    }
    .nav-item.active .icon { background: var(--sage-light); }
    .nav-item:not(.active) .icon { background: var(--bg); }

    .sidebar-footer {
      margin-top: auto;
      padding: 16px 22px 0;
      border-top: 1px solid var(--border);
    }
    .user-chip {
      display: flex; align-items: center; gap: 10px;
    }
    .avatar {
      width: 34px; height: 34px;
      border-radius: 50%;
      background: linear-gradient(135deg, var(--sage), var(--blue));
      display: flex; align-items: center; justify-content: center;
      font-size: 13px; font-weight: 700; color: #fff;
      flex-shrink: 0;
    }
    .user-info .user-name { font-size: 0.82rem; font-weight: 600; }
    .user-info .user-role { font-size: 0.7rem; color: var(--muted); }

    /* ── Main ── */
    .main {
      flex: 1;
      padding: 30px 32px;
      overflow-y: auto;
      max-width: 1100px;
    }

    .topbar {
      display: flex;
      align-items: flex-end;
      justify-content: space-between;
      margin-bottom: 28px;
      flex-wrap: wrap;
      gap: 12px;
    }
    .topbar-left h1 {
      font-size: 1.45rem;
      font-weight: 700;
      letter-spacing: -0.4px;
      color: var(--text);
    }
    .topbar-left p { font-size: 0.83rem; color: var(--muted); margin-top: 3px; }

    .topbar-right { display: flex; align-items: center; gap: 10px; }
    .pill {
      background: var(--surface);
      border: 1px solid var(--border);
      border-radius: 20px;
      padding: 6px 14px;
      font-size: 0.78rem;
      font-weight: 600;
      color: var(--muted);
      cursor: pointer;
    }
    .pill.primary {
      background: var(--accent);
      border-color: var(--accent);
      color: #fff;
    }

    /* ── Grid Layout ── */
    .grid-top {
      display: grid;
      grid-template-columns: 1fr 320px;
      gap: 20px;
      margin-bottom: 20px;
    }
    .grid-bottom {
      display: grid;
      grid-template-columns: 1fr 1fr;
      gap: 20px;
    }

    /* ── Card ── */
    .card {
      background: var(--surface);
      border: 1px solid var(--border);
      border-radius: 16px;
      padding: 22px 24px;
    }
    .card-header {
      display: flex;
      align-items: center;
      justify-content: space-between;
      margin-bottom: 18px;
    }
    .card-title {
      font-size: 0.88rem;
      font-weight: 700;
      color: var(--text);
      letter-spacing: -0.2px;
    }
    .card-action {
      font-size: 0.72rem;
      color: var(--accent);
      font-weight: 600;
      cursor: pointer;
    }

    /* ── Weekly Calendar ── */
    .week-grid {
      display: grid;
      grid-template-columns: repeat(7, 1fr);
      gap: 6px;
    }
    .day-col { display: flex; flex-direction: column; gap: 5px; }
    .day-header {
      text-align: center;
      font-size: 0.68rem;
      font-weight: 700;
      text-transform: uppercase;
      letter-spacing: 0.05em;
      color: var(--muted);
      padding-bottom: 6px;
    }
    .day-num {
      text-align: center;
      font-size: 0.88rem;
      font-weight: 700;
      color: var(--text);
      width: 30px; height: 30px;
      border-radius: 50%;
      display: flex; align-items: center; justify-content: center;
      margin: 0 auto 6px;
    }
    .day-num.today { background: var(--accent); color: #fff; }
    .day-num.past  { color: var(--muted); }

    .event-chip {
      border-radius: 6px;
      padding: 4px 6px;
      font-size: 0.67rem;
      font-weight: 600;
      line-height: 1.3;
      cursor: default;
    }
    .chip-sage   { background: var(--sage-light); color: #3a5c42; }
    .chip-blue   { background: var(--blue-light); color: #2b4f70; }
    .chip-cream  { background: var(--cream-dark); color: #5c4a2a; }
    .chip-lav    { background: var(--lavender); color: #3c2e6e; opacity:0.85; }
    .chip-peach  { background: var(--peach); color: #6b2f10; opacity:0.85; }

    /* ── Focus Goal ── */
    .focus-card {
      background: var(--sage-pale);
      border: 1.5px solid var(--sage-light);
      border-radius: 16px;
      padding: 22px 24px;
    }
    .focus-badge {
      display: inline-flex;
      align-items: center;
      gap: 6px;
      background: var(--sage-light);
      color: var(--accent);
      font-size: 0.68rem;
      font-weight: 700;
      letter-spacing: 0.06em;
      text-transform: uppercase;
      border-radius: 20px;
      padding: 4px 10px;
      margin-bottom: 14px;
    }
    .focus-goal-text {
      font-size: 1.05rem;
      font-weight: 700;
      color: var(--text);
      line-height: 1.45;
      margin-bottom: 14px;
    }
    .focus-meta {
      display: flex;
      gap: 14px;
      flex-wrap: wrap;
    }
    .meta-chip {
      background: var(--surface);
      border: 1px solid var(--sage-light);
      border-radius: 8px;
      padding: 5px 10px;
      font-size: 0.75rem;
      font-weight: 600;
      color: var(--muted);
      display: flex; align-items: center; gap: 5px;
    }
    .progress-mini {
      margin-top: 16px;
    }
    .progress-mini .prog-row {
      display: flex;
      justify-content: space-between;
      font-size: 0.74rem;
      color: var(--muted);
      margin-bottom: 5px;
      font-weight: 600;
    }
    .prog-track {
      background: var(--sage-light);
      border-radius: 999px;
      height: 7px;
      overflow: hidden;
    }
    .prog-fill {
      height: 100%;
      border-radius: 999px;
      background: var(--accent);
    }

    /* ── Time Block Tracker ── */
    .time-blocks {
      display: flex;
      flex-direction: column;
      gap: 8px;
    }
    .time-slot {
      display: flex;
      align-items: center;
      gap: 10px;
    }
    .time-label {
      font-size: 0.7rem;
      color: var(--muted);
      font-weight: 600;
      width: 44px;
      flex-shrink: 0;
      text-align: right;
    }
    .time-bar-wrap {
      flex: 1;
      display: flex;
      gap: 4px;
    }
    .block {
      border-radius: 7px;
      padding: 6px 10px;
      font-size: 0.7rem;
      font-weight: 600;
      line-height: 1.2;
      display: flex; align-items: center;
      cursor: default;
      transition: opacity 0.15s;
    }
    .block:hover { opacity: 0.82; }
    .block-sage   { background: var(--sage-light); color: #2e5436; }
    .block-blue   { background: var(--blue-light); color: #1e3f60; }
    .block-cream  { background: var(--cream-dark); color: #4a3820; }
    .block-lav    { background: var(--lav-pale); color: #3c2e6e; border: 1px solid var(--lavender); }
    .block-peach  { background: var(--peach-pale); color: #6b2f10; border: 1px solid var(--peach); }
    .block-empty  { background: var(--bg); color: var(--muted); border: 1px dashed var(--border); }

    /* ── Subject Progress ── */
    .subject-list { display: flex; flex-direction: column; gap: 14px; }
    .subject-item {}
    .subject-row {
      display: flex;
      justify-content: space-between;
      align-items: center;
      margin-bottom: 5px;
    }
    .subject-name {
      font-size: 0.82rem;
      font-weight: 600;
      color: var(--text);
      display: flex; align-items: center; gap: 7px;
    }
    .subj-dot {
      width: 8px; height: 8px;
      border-radius: 50%;
      flex-shrink: 0;
    }
    .subject-pct { font-size: 0.75rem; font-weight: 700; color: var(--muted); }
    .subj-track {
      background: var(--bg);
      border-radius: 999px;
      height: 7px;
      overflow: hidden;
    }
    .subj-fill { height: 100%; border-radius: 999px; }

    /* ── Active Recall Checklist ── */
    .recall-list { display: flex; flex-direction: column; gap: 10px; }
    .recall-item {
      display: flex;
      align-items: flex-start;
      gap: 12px;
      padding: 12px 14px;
      border-radius: 10px;
      background: var(--bg);
      border: 1px solid var(--border);
      cursor: pointer;
      transition: background 0.15s;
      user-select: none;
    }
    .recall-item:hover { background: var(--cream); }
    .recall-item.done { opacity: 0.55; }
    .recall-item.done .recall-text { text-decoration: line-through; color: var(--muted); }

    .checkbox {
      width: 18px; height: 18px;
      border-radius: 5px;
      border: 2px solid var(--border);
      flex-shrink: 0;
      display: flex; align-items: center; justify-content: center;
      margin-top: 1px;
      transition: background 0.15s, border-color 0.15s;
    }
    .recall-item.done .checkbox {
      background: var(--accent);
      border-color: var(--accent);
    }
    .checkbox-tick { display: none; font-size: 10px; color: #fff; }
    .recall-item.done .checkbox-tick { display: block; }

    .recall-text { font-size: 0.82rem; font-weight: 500; color: var(--text); }
    .recall-sub  { font-size: 0.7rem; color: var(--muted); margin-top: 2px; }
    .recall-tag {
      margin-left: auto;
      font-size: 0.65rem;
      font-weight: 700;
      padding: 2px 8px;
      border-radius: 20px;
      flex-shrink: 0;
    }

    /* ── Stats Strip ── */
    .stats-strip {
      display: grid;
      grid-template-columns: repeat(4, 1fr);
      gap: 12px;
      margin-bottom: 20px;
    }
    .stat-card {
      background: var(--surface);
      border: 1px solid var(--border);
      border-radius: 12px;
      padding: 16px 18px;
      display: flex;
      align-items: center;
      gap: 12px;
    }
    .stat-icon {
      width: 36px; height: 36px;
      border-radius: 10px;
      display: flex; align-items: center; justify-content: center;
      font-size: 16px;
      flex-shrink: 0;
    }
    .stat-val { font-size: 1.2rem; font-weight: 700; color: var(--text); }
    .stat-lbl { font-size: 0.7rem; color: var(--muted); font-weight: 600; margin-top: 1px; }

    @media (max-width: 900px) {
      .grid-top, .grid-bottom { grid-template-columns: 1fr; }
      .stats-strip { grid-template-columns: repeat(2, 1fr); }
      .main { padding: 20px 18px; }
      .sidebar { display: none; }
    }
  </style>
</head>
<body>

<!-- ── Sidebar ── -->
<aside class="sidebar">
  <div class="sidebar-logo">
    <div class="logo-icon">📖</div>
    <h2>StudyFlow</h2>
    <p>Your personal planner</p>
  </div>

  <div class="nav-section">
    <div class="nav-label">Planning</div>
    <div class="nav-item active">
      <div class="icon">🗓️</div> Weekly View
    </div>
    <div class="nav-item">
      <div class="icon">⏱️</div> Time Blocks
    </div>
    <div class="nav-item">
      <div class="icon">🎯</div> Focus Goals
    </div>
  </div>

  <div class="nav-section" style="margin-top:8px;">
    <div class="nav-label">Study</div>
    <div class="nav-item">
      <div class="icon">📊</div> Subject Progress
    </div>
    <div class="nav-item">
      <div class="icon">🃏</div> Flashcards
    </div>
    <div class="nav-item">
      <div class="icon">✅</div> Active Recall
    </div>
  </div>

  <div class="nav-section" style="margin-top:8px;">
    <div class="nav-label">Review</div>
    <div class="nav-item">
      <div class="icon">📈</div> Analytics
    </div>
    <div class="nav-item">
      <div class="icon">🗒️</div> Notes
    </div>
  </div>

  <div class="sidebar-footer">
    <div class="user-chip">
      <div class="avatar">P</div>
      <div class="user-info">
        <div class="user-name">Pranathi</div>
        <div class="user-role">Final Year · CS</div>
      </div>
    </div>
  </div>
</aside>

<!-- ── Main ── -->
<main class="main">

  <!-- Topbar -->
  <div class="topbar">
    <div class="topbar-left">
      <h1>Weekly Dashboard</h1>
      <p>Week of June 23 – 29, 2025 &nbsp;·&nbsp; Semester 2 Finals Block</p>
    </div>
    <div class="topbar-right">
      <div class="pill">← Prev Week</div>
      <div class="pill">Next Week →</div>
      <div class="pill primary">+ Add Session</div>
    </div>
  </div>

  <!-- Stats Strip -->
  <div class="stats-strip">
    <div class="stat-card">
      <div class="stat-icon" style="background:var(--sage-pale);">🔥</div>
      <div>
        <div class="stat-val">12</div>
        <div class="stat-lbl">Day Streak</div>
      </div>
    </div>
    <div class="stat-card">
      <div class="stat-icon" style="background:var(--blue-pale);">⏱️</div>
      <div>
        <div class="stat-val">4.5h</div>
        <div class="stat-lbl">Today Studied</div>
      </div>
    </div>
    <div class="stat-card">
      <div class="stat-icon" style="background:var(--cream);">📋</div>
      <div>
        <div class="stat-val">7 / 10</div>
        <div class="stat-lbl">Tasks Done</div>
      </div>
    </div>
    <div class="stat-card">
      <div class="stat-icon" style="background:var(--lav-pale);">🃏</div>
      <div>
        <div class="stat-val">84</div>
        <div class="stat-lbl">Cards Reviewed</div>
      </div>
    </div>
  </div>

  <!-- Row 1: Weekly Calendar + Focus Goal -->
  <div class="grid-top">

    <!-- Weekly Calendar -->
    <div class="card">
      <div class="card-header">
        <span class="card-title">Weekly Calendar</span>
        <span class="card-action">Full view →</span>
      </div>
      <div class="week-grid">

        <div class="day-col">
          <div class="day-header">Mon</div>
          <div class="day-num past">23</div>
          <div class="event-chip chip-sage">Calculus</div>
          <div class="event-chip chip-blue">Physics Lab</div>
        </div>

        <div class="day-col">
          <div class="day-header">Tue</div>
          <div class="day-num past">24</div>
          <div class="event-chip chip-cream">Algorithms</div>
          <div class="event-chip chip-lav">Mock Test</div>
        </div>

        <div class="day-col">
          <div class="day-header">Wed</div>
          <div class="day-num past">25</div>
          <div class="event-chip chip-blue">Data Structs</div>
          <div class="event-chip chip-sage">Revision</div>
        </div>

        <div class="day-col">
          <div class="day-header">Thu</div>
          <div class="day-num today">26</div>
          <div class="event-chip chip-peach">OS Concepts</div>
          <div class="event-chip chip-cream">Flashcards</div>
          <div class="event-chip chip-sage">Recall ×3</div>
        </div>

        <div class="day-col">
          <div class="day-header">Fri</div>
          <div class="day-num">27</div>
          <div class="event-chip chip-lav">Networks</div>
          <div class="event-chip chip-blue">Group Study</div>
        </div>

        <div class="day-col">
          <div class="day-header">Sat</div>
          <div class="day-num">28</div>
          <div class="event-chip chip-cream">Past Papers</div>
          <div class="event-chip chip-sage">Break 🌿</div>
        </div>

        <div class="day-col">
          <div class="day-header">Sun</div>
          <div class="day-num">29</div>
          <div class="event-chip chip-blue">Review Week</div>
        </div>

      </div>
    </div>

    <!-- Focus Goal of the Day -->
    <div class="focus-card">
      <div class="card-header" style="margin-bottom:10px;">
        <span class="card-title">Focus Goal of the Day</span>
        <span class="card-action">Edit</span>
      </div>
      <div class="focus-badge">⭐ Today · Thu 26</div>
      <div class="focus-goal-text">Complete OS Concepts Chapter 4 &amp; run 3 active recall sessions before 8 PM</div>
      <div class="focus-meta">
        <div class="meta-chip">🕐 6 hrs planned</div>
        <div class="meta-chip">📚 OS Concepts</div>
        <div class="meta-chip">🟢 On track</div>
      </div>
      <div class="progress-mini">
        <div class="prog-row">
          <span>Daily Progress</span>
          <span>65%</span>
        </div>
        <div class="prog-track"><div class="prog-fill" style="width:65%"></div></div>
      </div>
    </div>

  </div>

  <!-- Row 2: Time Block Tracker + Subject Progress + Recall -->
  <div style="display:grid;grid-template-columns:1fr 1fr 1fr;gap:20px;margin-bottom:20px;">

    <!-- Time Block Tracker -->
    <div class="card" style="grid-column: span 1;">
      <div class="card-header">
        <span class="card-title">Time Block Tracker</span>
        <span class="card-action">Today</span>
      </div>
      <div class="time-blocks">

        <div class="time-slot">
          <div class="time-label">7 AM</div>
          <div class="time-bar-wrap">
            <div class="block block-cream" style="flex:2">🌅 Morning Review</div>
            <div class="block block-empty" style="flex:1">Free</div>
          </div>
        </div>

        <div class="time-slot">
          <div class="time-label">9 AM</div>
          <div class="time-bar-wrap">
            <div class="block block-sage" style="flex:3">📘 Calculus — Chapter 7</div>
          </div>
        </div>

        <div class="time-slot">
          <div class="time-label">11 AM</div>
          <div class="time-bar-wrap">
            <div class="block block-blue" style="flex:2">🖥️ OS Concepts</div>
            <div class="block block-empty" style="flex:1">Break</div>
          </div>
        </div>

        <div class="time-slot">
          <div class="time-label">1 PM</div>
          <div class="time-bar-wrap">
            <div class="block block-cream" style="flex:1">🍱 Lunch</div>
            <div class="block block-lav" style="flex:2">🃏 Flashcards</div>
          </div>
        </div>

        <div class="time-slot">
          <div class="time-label">3 PM</div>
          <div class="time-bar-wrap">
            <div class="block block-sage" style="flex:2">✍️ Algorithms PS</div>
            <div class="block block-empty" style="flex:1">Free</div>
          </div>
        </div>

        <div class="time-slot">
          <div class="time-label">5 PM</div>
          <div class="time-bar-wrap">
            <div class="block block-blue" style="flex:3">🔁 Active Recall ×3</div>
          </div>
        </div>

        <div class="time-slot">
          <div class="time-label">7 PM</div>
          <div class="time-bar-wrap">
            <div class="block block-peach" style="flex:2">📝 Mock Questions</div>
            <div class="block block-cream" style="flex:1">Wind down</div>
          </div>
        </div>

      </div>
    </div>

    <!-- Subject Progress -->
    <div class="card">
      <div class="card-header">
        <span class="card-title">Subject Progress</span>
        <span class="card-action">All subjects →</span>
      </div>
      <div class="subject-list">

        <div class="subject-item">
          <div class="subject-row">
            <span class="subject-name">
              <span class="subj-dot" style="background:var(--sage)"></span>
              Calculus
            </span>
            <span class="subject-pct">78%</span>
          </div>
          <div class="subj-track"><div class="subj-fill" style="width:78%;background:var(--sage)"></div></div>
        </div>

        <div class="subject-item">
          <div class="subject-row">
            <span class="subject-name">
              <span class="subj-dot" style="background:var(--blue)"></span>
              OS Concepts
            </span>
            <span class="subject-pct">55%</span>
          </div>
          <div class="subj-track"><div class="subj-fill" style="width:55%;background:var(--blue)"></div></div>
        </div>

        <div class="subject-item">
          <div class="subject-row">
            <span class="subject-name">
              <span class="subj-dot" style="background:var(--cream-dark)"></span>
              Algorithms
            </span>
            <span class="subject-pct">90%</span>
          </div>
          <div class="subj-track"><div class="subj-fill" style="width:90%;background:var(--cream-dark)"></div></div>
        </div>

        <div class="subject-item">
          <div class="subject-row">
            <span class="subject-name">
              <span class="subj-dot" style="background:var(--lavender)"></span>
              Networks
            </span>
            <span class="subject-pct">42%</span>
          </div>
          <div class="subj-track"><div class="subj-fill" style="width:42%;background:var(--lavender)"></div></div>
        </div>

        <div class="subject-item">
          <div class="subject-row">
            <span class="subject-name">
              <span class="subj-dot" style="background:var(--peach)"></span>
              Data Structures
            </span>
            <span class="subject-pct">83%</span>
          </div>
          <div class="subj-track"><div class="subj-fill" style="width:83%;background:var(--peach)"></div></div>
        </div>

        <div class="subject-item">
          <div class="subject-row">
            <span class="subject-name">
              <span class="subj-dot" style="background:#a0c8c0"></span>
              Physics Lab
            </span>
            <span class="subject-pct">67%</span>
          </div>
          <div class="subj-track"><div class="subj-fill" style="width:67%;background:#a0c8c0"></div></div>
        </div>

      </div>
    </div>

    <!-- Active Recall Checklist -->
    <div class="card">
      <div class="card-header">
        <span class="card-title">Active Recall Sessions</span>
        <span class="card-action">+ Add</span>
      </div>
      <div class="recall-list" id="recallList">

        <div class="recall-item done" onclick="toggleRecall(this)">
          <div class="checkbox"><span class="checkbox-tick">✓</span></div>
          <div style="flex:1">
            <div class="recall-text">OS: Process Scheduling</div>
            <div class="recall-sub">Spaced repetition · 12 cards</div>
          </div>
          <div class="recall-tag chip-sage">Done</div>
        </div>

        <div class="recall-item done" onclick="toggleRecall(this)">
          <div class="checkbox"><span class="checkbox-tick">✓</span></div>
          <div style="flex:1">
            <div class="recall-text">Algo: Big-O Notation Quiz</div>
            <div class="recall-sub">Self-test · 8 questions</div>
          </div>
          <div class="recall-tag chip-sage">Done</div>
        </div>

        <div class="recall-item" onclick="toggleRecall(this)">
          <div class="checkbox"><span class="checkbox-tick">✓</span></div>
          <div style="flex:1">
            <div class="recall-text">Calculus: Derivatives Drill</div>
            <div class="recall-sub">Flashcard deck · 20 cards</div>
          </div>
          <div class="recall-tag chip-blue" style="background:var(--blue-light);color:#1e3f60;">Today</div>
        </div>

        <div class="recall-item" onclick="toggleRecall(this)">
          <div class="checkbox"><span class="checkbox-tick">✓</span></div>
          <div style="flex:1">
            <div class="recall-text">Networks: OSI Model Layers</div>
            <div class="recall-sub">Blurting method · 15 min</div>
          </div>
          <div class="recall-tag chip-blue" style="background:var(--blue-light);color:#1e3f60;">Today</div>
        </div>

        <div class="recall-item" onclick="toggleRecall(this)">
          <div class="checkbox"><span class="checkbox-tick">✓</span></div>
          <div style="flex:1">
            <div class="recall-text">Data Structs: Tree Traversal</div>
            <div class="recall-sub">Whiteboard practice · 25 min</div>
          </div>
          <div class="recall-tag" style="background:var(--lav-pale);color:#3c2e6e;border:1px solid var(--lavender);">Fri</div>
        </div>

      </div>
    </div>

  </div>

</main>

<script>
  function toggleRecall(el) {
    el.classList.toggle('done');
    const tag = el.querySelector('.recall-tag');
    if (el.classList.contains('done')) {
      tag.textContent = 'Done';
      tag.className = 'recall-tag chip-sage';
    } else {
      tag.textContent = 'Pending';
      tag.className = 'recall-tag chip-blue';
      tag.style.cssText = 'background:var(--blue-light);color:#1e3f60;';
    }
  }

  // Sidebar nav interaction
  document.querySelectorAll('.nav-item').forEach(item => {
    item.addEventListener('click', () => {
      document.querySelectorAll('.nav-item').forEach(i => i.classList.remove('active'));
      item.classList.add('active');
    });
  });
</script>

</body>
</html>
