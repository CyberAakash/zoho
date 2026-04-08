---
title: IM Channel License Handler
description: Complete lifecycle guide — channel statuses, plan tier limits, upgrade/downgrade flows, department enable/disable scenarios.
weight: 2
---

<style>
.im-doc {
  --im-bg: #0d1117;
  --im-surface: #161b22;
  --im-surface-alt: #1c2333;
  --im-border: #30363d;
  --im-text: #e6edf3;
  --im-text-muted: #8b949e;
  --im-accent: #58a6ff;
  --im-green: #3fb950;
  --im-red: #f85149;
  --im-yellow: #d29922;
  --im-orange: #db6d28;
  --im-purple: #bc8cff;
  --im-cyan: #39d2c0;
  line-height: 1.7;
}

.im-doc h2 {
  font-size: 1.5rem;
  color: var(--im-accent);
  margin-top: 3rem;
  margin-bottom: 1rem;
  padding-bottom: 0.4rem;
  border-bottom: 1px solid var(--im-border);
}

.im-doc h3 {
  font-size: 1.15rem;
  color: var(--im-purple);
  margin-top: 1.75rem;
  margin-bottom: 0.75rem;
}

.im-doc p { margin-bottom: 1rem; }

.im-doc table {
  width: 100%;
  border-collapse: collapse;
  margin: 1rem 0 1.5rem;
  font-size: 0.92rem;
}

.im-doc th {
  background: var(--im-surface-alt);
  color: var(--im-accent);
  text-align: left;
  padding: 0.6rem 1rem;
  border: 1px solid var(--im-border);
  font-weight: 600;
}

.im-doc td {
  padding: 0.5rem 1rem;
  border: 1px solid var(--im-border);
  background: var(--im-surface);
}

.im-doc tr:hover td { background: var(--im-surface-alt); }

.im-doc pre {
  background: var(--im-surface);
  border: 1px solid var(--im-border);
  border-radius: 8px;
  padding: 1.25rem;
  overflow-x: auto;
  margin: 1rem 0 1.5rem;
  font-size: 0.88rem;
  line-height: 1.6;
  font-family: 'SF Mono', 'Fira Code', 'JetBrains Mono', monospace;
}

.im-doc code {
  font-family: 'SF Mono', 'Fira Code', 'JetBrains Mono', monospace;
  font-size: 0.88rem;
  background: var(--im-surface-alt);
  padding: 0.15rem 0.4rem;
  border-radius: 4px;
}

.im-doc pre code { background: none; padding: 0; }

.im-doc .badge {
  display: inline-block;
  padding: 0.15rem 0.6rem;
  border-radius: 12px;
  font-size: 0.8rem;
  font-weight: 600;
  letter-spacing: 0.02em;
}

.im-doc .badge-active   { background: rgba(63,185,80,0.15);   color: var(--im-green);     border: 1px solid rgba(63,185,80,0.3); }
.im-doc .badge-disabled  { background: rgba(139,148,158,0.15); color: var(--im-text-muted); border: 1px solid rgba(139,148,158,0.3); }
.im-doc .badge-lic-dis   { background: rgba(248,81,73,0.15);   color: var(--im-red);       border: 1px solid rgba(248,81,73,0.3); }
.im-doc .badge-unknown   { background: rgba(210,153,34,0.15);  color: var(--im-yellow);    border: 1px solid rgba(210,153,34,0.3); }

.im-doc .callout {
  border-left: 4px solid var(--im-accent);
  background: var(--im-surface);
  padding: 1rem 1.25rem;
  margin: 1.25rem 0;
  border-radius: 0 8px 8px 0;
}

.im-doc .callout-warn    { border-left-color: var(--im-yellow); }
.im-doc .callout-success { border-left-color: var(--im-green); }
.im-doc .callout strong          { color: var(--im-accent); }
.im-doc .callout-warn strong     { color: var(--im-yellow); }
.im-doc .callout-success strong  { color: var(--im-green); }

.im-doc .lifecycle-box {
  background: var(--im-surface);
  border: 1px solid var(--im-border);
  border-radius: 10px;
  padding: 1.5rem;
  margin: 1.25rem 0;
}

.im-doc .lifecycle-box h4 {
  color: var(--im-cyan);
  margin-bottom: 0.6rem;
  font-size: 1rem;
}

.im-doc .arrow-step {
  display: flex;
  align-items: flex-start;
  gap: 0.75rem;
  margin: 0.5rem 0;
}

.im-doc .arrow-step .step-icon {
  flex-shrink: 0;
  width: 26px;
  height: 26px;
  border-radius: 50%;
  display: flex;
  align-items: center;
  justify-content: center;
  font-weight: 700;
  font-size: 0.75rem;
  margin-top: 0.1rem;
}

.im-doc .step-num    { background: var(--im-accent); color: var(--im-bg, #0d1117); }
.im-doc .step-result { background: var(--im-green);  color: var(--im-bg, #0d1117); }

.im-doc .flow-diagram {
  background: var(--im-surface);
  border: 1px solid var(--im-border);
  border-radius: 10px;
  padding: 1.5rem;
  margin: 1.25rem 0;
  font-family: 'SF Mono', 'Fira Code', monospace;
  font-size: 0.85rem;
  line-height: 1.8;
  overflow-x: auto;
  white-space: pre;
  text-align: left;
}

.im-doc .section-divider {
  border: none;
  height: 1px;
  background: linear-gradient(to right, transparent, var(--im-border), transparent);
  margin: 3rem 0;
}

.im-doc .status-grid {
  display: grid;
  grid-template-columns: repeat(auto-fit, minmax(230px, 1fr));
  gap: 1rem;
  margin: 1.25rem 0;
}

.im-doc .status-card {
  background: var(--im-surface);
  border: 1px solid var(--im-border);
  border-radius: 10px;
  padding: 1.1rem;
  border-top: 3px solid;
}

.im-doc .status-card h4   { font-size: 1rem; margin-bottom: 0.4rem; }
.im-doc .status-card p    { font-size: 0.88rem; color: var(--im-text-muted); margin-bottom: 0; }

.im-doc .status-card.active   { border-top-color: var(--im-green); }
.im-doc .status-card.active h4  { color: var(--im-green); }
.im-doc .status-card.disabled  { border-top-color: var(--im-text-muted); }
.im-doc .status-card.disabled h4 { color: var(--im-text-muted); }
.im-doc .status-card.lic-dis  { border-top-color: var(--im-red); }
.im-doc .status-card.lic-dis h4 { color: var(--im-red); }
.im-doc .status-card.unknown  { border-top-color: var(--im-yellow); }
.im-doc .status-card.unknown h4 { color: var(--im-yellow); }

.im-doc .tier-bar {
  display: flex;
  align-items: center;
  gap: 0;
  margin: 1.25rem 0;
}

.im-doc .tier-segment {
  padding: 0.8rem 1.25rem;
  text-align: center;
  font-weight: 600;
  font-size: 0.85rem;
  border: 1px solid var(--im-border);
}

.im-doc .tier-segment:first-child { border-radius: 8px 0 0 8px; }
.im-doc .tier-segment:last-child  { border-radius: 0 8px 8px 0; }

.im-doc .tier-free { background: rgba(139,148,158,0.12); color: var(--im-text-muted); flex: 1; }
.im-doc .tier-std  { background: rgba(58,166,255,0.12);  color: var(--im-accent);     flex: 1; }
.im-doc .tier-prof { background: rgba(188,140,255,0.12); color: var(--im-purple);     flex: 1.5; }
.im-doc .tier-ent  { background: rgba(63,185,80,0.12);   color: var(--im-green);      flex: 2; }

.im-doc .tier-segment .tier-limit {
  display: block;
  font-size: 1.3rem;
  margin-top: 0.2rem;
}

.im-doc .comparison-grid {
  display: grid;
  grid-template-columns: 1fr 1fr;
  gap: 1rem;
  margin: 1.25rem 0;
}

.im-doc .comparison-card {
  background: var(--im-surface);
  border: 1px solid var(--im-border);
  border-radius: 10px;
  padding: 1.25rem;
}

.im-doc .comparison-card h4   { margin-bottom: 0.75rem; font-size: 1rem; }
.im-doc .comparison-card ul   { list-style: none; padding: 0; }
.im-doc .comparison-card li   { padding: 0.3rem 0; font-size: 0.9rem; color: var(--im-text-muted); }
.im-doc .comparison-card li::before { margin-right: 0.5rem; font-weight: 700; }
.im-doc .comparison-card.card-disabled li::before { content: '—'; color: var(--im-text-muted); }
.im-doc .comparison-card.card-lic-dis li::before  { content: '⚡'; color: var(--im-red); }

.im-doc .trigger-card {
  background: var(--im-surface);
  border: 1px solid var(--im-border);
  border-radius: 10px;
  padding: 1.25rem;
  margin: 0.75rem 0;
}

.im-doc .trigger-card h4           { color: var(--im-cyan);       font-size: 0.95rem; margin-bottom: 0.5rem; }
.im-doc .trigger-card .trigger-path { font-family: 'SF Mono', 'Fira Code', monospace; font-size: 0.82rem; color: var(--im-text-muted); margin-bottom: 0.4rem; }
.im-doc .trigger-card .trigger-uses { font-size: 0.85rem; color: var(--im-purple); }

@media (max-width: 700px) {
  .im-doc .comparison-grid { grid-template-columns: 1fr; }
  .im-doc .status-grid     { grid-template-columns: 1fr 1fr; }
  .im-doc .tier-bar        { flex-wrap: wrap; }
  .im-doc .tier-segment    { flex: 1 1 45%; border-radius: 8px !important; margin: 2px; }
}
</style>

<div class="im-doc">

<!-- ═══════════════════════════════════════════════════════ -->
<h2>1. Channel Status Lifecycle</h2>

<h3>The 4 Statuses</h3>

<div class="status-grid">
  <div class="status-card active">
    <h4>ACTIVE <span class="badge badge-active">ID: 1</span></h4>
    <p>Channel is live, accepting messages</p>
  </div>
  <div class="status-card disabled">
    <h4>DISABLED <span class="badge badge-disabled">ID: 0</span></h4>
    <p>Manually turned off, or department was disabled</p>
  </div>
  <div class="status-card lic-dis">
    <h4>LICENSE_DISABLED <span class="badge badge-lic-dis">ID: 2</span></h4>
    <p>Turned off because license limit was hit or feature was disabled</p>
  </div>
  <div class="status-card unknown">
    <h4>UNKNOWN <span class="badge badge-unknown">ID: 3</span></h4>
    <p>Status not yet determined — needs SDK reconciliation</p>
  </div>
</div>

<h3>Two Types of Records</h3>

<div class="comparison-grid">
  <div class="lifecycle-box">
    <h4>Existing Customers <span class="badge badge-unknown">UNKNOWN</span></h4>
    <p style="font-size:0.9rem; color:var(--im-text-muted);">
      Before the STATUS column existed, channels had no status.
      <code>dd-changes</code> added the column with <strong style="color:var(--im-yellow);">DEFAULT = 3</strong>,
      so all existing records got <strong style="color:var(--im-yellow);">UNKNOWN</strong>.
    </p>
    <p style="font-size:0.9rem; color:var(--im-text-muted); margin-bottom:0;">
      These channels <em>might</em> be active in SDK, <em>might</em> not.
      We don't know until we check (reconciliation).
    </p>
  </div>
  <div class="lifecycle-box">
    <h4>New Customers <span class="badge badge-active">ACTIVE</span></h4>
    <p style="font-size:0.9rem; color:var(--im-text-muted);">
      <code>CustomChannel.java</code> sets the field default:
      <strong style="color:var(--im-green);">channelStatus = ChannelStatus.ACTIVE</strong>
    </p>
    <p style="font-size:0.9rem; color:var(--im-text-muted); margin-bottom:0;">
      New channels are created with status ACTIVE. No reconciliation needed — status is accurate.
    </p>
  </div>
</div>

<div class="callout callout-warn">
  <strong>Why UNKNOWN exists:</strong> When the STATUS column was added, existing channels were already running in production.
  We couldn't blindly mark them ACTIVE or DISABLED — we don't know their real state.
  So DEFAULT=3 (UNKNOWN) means: <em>"check with SDK before deciding."</em>
  <br><br>
  Every handler resolves UNKNOWN by:<br>
  1. Looking up the channel in SDK → is it active there?<br>
  2. If active in SDK → decide based on context (enable/disable/limit)<br>
  3. If not in SDK → mark DISABLED (it's dead)
</div>

<div class="callout">
  <strong>Important:</strong> UNKNOWN channels can exist in any department <em>before</em> the first license
  or department event processes it. After that first event, all UNKNOWN in that department
  are resolved — and they never come back (no Java code creates UNKNOWN).<br><br>
  <strong>UNKNOWN and LICENSE_DISABLED cannot coexist in the same department.</strong>
  Every handler that creates LICENSE_DISABLED also resolves ALL UNKNOWN
  in the same invocation. So in any scenario, a department has either:<br>
  • UNKNOWN channels (not yet processed), OR<br>
  • LICENSE_DISABLED channels (already processed)<br>
  • Never both.
</div>

<h3>Valid State Transitions</h3>

<table>
  <tr><th>From</th><th></th><th>To</th><th>Triggered By</th></tr>
  <tr>
    <td><span class="badge badge-unknown">UNKNOWN (3)</span></td>
    <td>→</td>
    <td><span class="badge badge-active">ACTIVE (1)</span></td>
    <td>Reconcile: SDK active + room available</td>
  </tr>
  <tr>
    <td><span class="badge badge-unknown">UNKNOWN (3)</span></td>
    <td>→</td>
    <td><span class="badge badge-disabled">DISABLED (0)</span></td>
    <td>Reconcile: not in SDK (dead channel)</td>
  </tr>
  <tr>
    <td><span class="badge badge-unknown">UNKNOWN (3)</span></td>
    <td>→</td>
    <td><span class="badge badge-lic-dis">LICENSE_DISABLED (2)</span></td>
    <td>Reconcile: SDK active + no room, or feature off</td>
  </tr>
  <tr><td colspan="4" style="background:transparent; border-color:transparent; height:4px; padding:0;"></td></tr>
  <tr>
    <td><span class="badge badge-active">ACTIVE (1)</span></td>
    <td>→</td>
    <td><span class="badge badge-disabled">DISABLED (0)</span></td>
    <td>Manual deactivate, department disabled</td>
  </tr>
  <tr>
    <td><span class="badge badge-active">ACTIVE (1)</span></td>
    <td>→</td>
    <td><span class="badge badge-lic-dis">LICENSE_DISABLED (2)</span></td>
    <td>License limit exceeded, feature disabled</td>
  </tr>
  <tr><td colspan="4" style="background:transparent; border-color:transparent; height:4px; padding:0;"></td></tr>
  <tr>
    <td><span class="badge badge-disabled">DISABLED (0)</span></td>
    <td>→</td>
    <td><span class="badge badge-active">ACTIVE (1)</span></td>
    <td>Manual activate, department enabled</td>
  </tr>
  <tr><td colspan="4" style="background:transparent; border-color:transparent; height:4px; padding:0;"></td></tr>
  <tr>
    <td><span class="badge badge-lic-dis">LICENSE_DISABLED (2)</span></td>
    <td>→</td>
    <td><span class="badge badge-active">ACTIVE (1)</span></td>
    <td>License limit increased, feature enabled, manual activate</td>
  </tr>
</table>

<div class="callout" style="margin-top:0.5rem;">
  <strong>Note:</strong> DISABLED and LICENSE_DISABLED never transition to each other directly.
</div>

<hr class="section-divider">

<!-- ═══════════════════════════════════════════════════════ -->
<h2>2. Plan Tiers &amp; Limits</h2>

<div class="tier-bar">
  <div class="tier-segment tier-free">Free<span class="tier-limit">0</span></div>
  <div class="tier-segment tier-std">Standard<span class="tier-limit">1</span></div>
  <div class="tier-segment tier-prof">Professional<span class="tier-limit">10</span></div>
  <div class="tier-segment tier-ent">Enterprise<span class="tier-limit">50</span></div>
</div>

<div class="callout">
  <strong>Per-IST means:</strong> WhatsApp limit is independent of Telegram limit.
  Enterprise with 50 = up to 50 WhatsApp channels <em>AND</em> up to 50 Telegram channels.
</div>

<hr class="section-divider">

<!-- ═══════════════════════════════════════════════════════ -->
<h2>3. Plan Upgrade/Downgrade Lifecycles</h2>

<!-- Lifecycle 1 -->
<h3>Lifecycle 1: Free → Enterprise → Free</h3>

<div class="lifecycle-box">
  <h4>State: Free (no IM feature)</h4>
  <p style="font-size:0.9rem; color:var(--im-text-muted);">
    All channels have <span class="badge badge-unknown">UNKNOWN</span> status (from dd-changes migration).
    License handler NOT invoked (feature doesn't exist).
  </p>
</div>

<div class="callout callout-success">
  <strong>⬆ UPGRADE to Enterprise (maxPerIST=50)</strong> — Trigger: <code>handleFeatureEnable(max=50)</code>
</div>

<div class="lifecycle-box">
  <div class="arrow-step">
    <div class="step-icon step-num">1</div>
    <div><strong>Resolve UNKNOWN in disabled depts</strong><br>
      <span style="color:var(--im-text-muted); font-size:0.9rem;">
        UNKNOWN → check SDK → active?<br>
        Yes → deActivate in <strong>IM SDK</strong> → <strong>Desk DB</strong>: LICENSE_DISABLED<br>
        No → <strong>Desk DB</strong>: DISABLED
      </span>
    </div>
  </div>
  <div class="arrow-step">
    <div class="step-icon step-num">2</div>
    <div><strong>Resolve UNKNOWN in active depts (limit=50)</strong><br>
      <span style="color:var(--im-text-muted); font-size:0.9rem;">
        UNKNOWN → check SDK → active?<br>
        Yes + room → <strong>Desk DB</strong>: ACTIVE (SDK already active, no SDK call)<br>
        Yes + no room → deActivate in <strong>IM SDK</strong> → <strong>Desk DB</strong>: LICENSE_DISABLED<br>
        No → <strong>Desk DB</strong>: DISABLED<br>
        (with 50 limit, almost all fit)
      </span>
    </div>
  </div>
  <div class="arrow-step">
    <div class="step-icon step-num">3</div>
    <div><strong>Re-enable LICENSE_DISABLED in active depts (up to 50)</strong><br>
      <span style="color:var(--im-text-muted); font-size:0.9rem;">LICENSE_DISABLED → activate in <strong>IM SDK</strong> → <strong>Desk DB</strong>: ACTIVE</span>
    </div>
  </div>
  <div class="arrow-step">
    <div class="step-icon step-result">✓</div>
    <div><strong style="color:var(--im-green)">Result:</strong> Channels come alive up to 50 per IST. All UNKNOWN resolved.</div>
  </div>
</div>

<div class="callout" style="border-left-color: var(--im-red);">
  <strong style="color:var(--im-red)">⬇ DOWNGRADE to Free</strong> — Trigger: <code>handleFeatureDisable()</code>
</div>

<div class="lifecycle-box">
  <div class="arrow-step">
    <div class="step-icon step-num">1</div>
    <div><strong>LICENSE_DISABLE all active channels</strong><br>
      <span style="color:var(--im-text-muted); font-size:0.9rem;">ACTIVE → deActivate in <strong>IM SDK</strong> → <strong>Desk DB</strong>: LICENSE_DISABLED</span>
    </div>
  </div>
  <div class="arrow-step">
    <div class="step-icon step-num">2</div>
    <div><strong>Resolve ALL UNKNOWN</strong> → 0 found <span style="color:var(--im-text-muted); font-size:0.9rem;">(all resolved during upgrade)</span></div>
  </div>
  <div class="arrow-step">
    <div class="step-icon step-result">✓</div>
    <div><strong style="color:var(--im-green)">Result:</strong> ZERO active. All LICENSE_DISABLED (or DISABLED from manual deactivation).</div>
  </div>
</div>

<!-- Lifecycle 2 -->
<h3>Lifecycle 2: Enterprise → Free → Enterprise</h3>

<div class="lifecycle-box">
  <h4>State: Enterprise (maxPerIST=50)</h4>
  <p style="font-size:0.9rem; color:var(--im-text-muted);">Many channels ACTIVE across departments. May have UNKNOWN channels if customer was Enterprise pre-migration (dd-changes set all existing channels to UNKNOWN, no license event fires for customers already on a paid plan).</p>
</div>

<div class="callout" style="border-left-color: var(--im-red);">
  <strong style="color:var(--im-red)">⬇ DOWNGRADE to Free</strong> — Trigger: <code>handleFeatureDisable()</code>
</div>

<div class="lifecycle-box">
  <div class="arrow-step">
    <div class="step-icon step-num">1</div>
    <div><strong>LICENSE_DISABLE all active channels</strong><br>
      <span style="color:var(--im-text-muted); font-size:0.9rem;">ACTIVE → deActivate in <strong>IM SDK</strong> → <strong>Desk DB</strong>: LICENSE_DISABLED</span>
    </div>
  </div>
  <div class="arrow-step">
    <div class="step-icon step-num">2</div>
    <div><strong>Resolve ALL UNKNOWN</strong><br>
      <span style="color:var(--im-text-muted); font-size:0.9rem;">UNKNOWN → check SDK → active? deActivate → LICENSE_DISABLED. Not active? → DISABLED<br>(0 found if a prior license/dept event already resolved them)</span></div>
  </div>
  <div class="callout" style="margin-top:0.5rem;">
    <strong>Why LICENSE_DISABLED (not DISABLED)?</strong> The channel itself is fine, only the LICENSE prevents it. If they upgrade again, we restore them.
  </div>
  <div class="arrow-step">
    <div class="step-icon step-result">✓</div>
    <div><strong style="color:var(--im-green)">Result:</strong> 0 active. All LICENSE_DISABLED.</div>
  </div>
</div>

<div class="callout callout-success">
  <strong>⬆ UPGRADE back to Enterprise (max=50)</strong> — Trigger: <code>handleFeatureEnable(max=50)</code>
</div>

<div class="lifecycle-box">
  <div class="arrow-step">
    <div class="step-icon step-num">1</div>
    <div><strong>Resolve UNKNOWN in disabled depts</strong> → 0 found <span style="color:var(--im-text-muted); font-size:0.9rem;">(downgrade resolved all)</span></div>
  </div>
  <div class="arrow-step">
    <div class="step-icon step-num">2</div>
    <div><strong>Resolve UNKNOWN in active depts</strong> → 0 found <span style="color:var(--im-text-muted); font-size:0.9rem;">(downgrade resolved all)</span></div>
  </div>
  <div class="arrow-step">
    <div class="step-icon step-num">3</div>
    <div><strong>Re-enable LICENSE_DISABLED in active depts</strong><br>
      <span style="color:var(--im-text-muted); font-size:0.9rem;">LICENSE_DISABLED → activate in <strong>IM SDK</strong> → <strong>Desk DB</strong>: ACTIVE<br>All channels per IST come back (up to 50)!</span>
    </div>
  </div>
  <div class="arrow-step">
    <div class="step-icon step-result">✓</div>
    <div><strong style="color:var(--im-green)">Result:</strong> Channels restored exactly as before.</div>
  </div>
</div>

<div class="callout callout-success">
  <strong>Key insight:</strong>
  LICENSE_DISABLED preserves the channel for re-activation.
  DISABLED means "user chose to turn it off" — license handler doesn't touch it.
</div>

<!-- Lifecycle 3 -->
<h3>Lifecycle 3: Standard → Enterprise → Standard</h3>

<div class="lifecycle-box">
  <h4>State: Standard (maxPerIST=1)</h4>
  <p style="font-size:0.9rem; color:var(--im-text-muted);">1 WhatsApp channel ACTIVE per IST. May have UNKNOWN channels if customer was Standard pre-migration.</p>
</div>

<div class="callout callout-success">
  <strong>⬆ UPGRADE to Enterprise (max=50)</strong> — Trigger: <code>handleFeatureLimitUpdate(max=50)</code>
</div>

<div class="lifecycle-box">
  <div class="arrow-step">
    <div class="step-icon step-num">1</div>
    <div><strong>Disable ACTIVE in disabled depts</strong> → LICENSE_DISABLED</div>
  </div>
  <div class="arrow-step">
    <div class="step-icon step-num">2</div>
    <div><strong>Resolve UNKNOWN in disabled depts</strong><br>
      <span style="color:var(--im-text-muted); font-size:0.9rem;">(0 found if prior event resolved them, else resolves to LICENSE_DISABLED/DISABLED)</span></div>
  </div>
  <div class="arrow-step">
    <div class="step-icon step-num">3</div>
    <div><strong>Build active count</strong></div>
  </div>
  <div class="arrow-step">
    <div class="step-icon step-num">4</div>
    <div><strong>Resolve UNKNOWN in active depts</strong><br>
      <span style="color:var(--im-text-muted); font-size:0.9rem;">(0 found if prior event resolved them, else resolves to ACTIVE/LICENSE_DISABLED/DISABLED)</span></div>
  </div>
  <div class="arrow-step">
    <div class="step-icon step-num">5</div>
    <div><strong>Trim excess?</strong> 1 active, limit 50 → no excess</div>
  </div>
  <div class="arrow-step">
    <div class="step-icon step-num">6</div>
    <div><strong>Re-enable LICENSE_DISABLED in active depts</strong><br>
      <span style="color:var(--im-text-muted); font-size:0.9rem;">LICENSE_DISABLED → activate in <strong>IM SDK</strong> → <strong>Desk DB</strong>: ACTIVE</span>
    </div>
  </div>
  <div class="arrow-step">
    <div class="step-icon step-result">✓</div>
    <div><strong style="color:var(--im-green)">Result:</strong> User can now activate up to 50 channels per IST.</div>
  </div>
  <p style="font-size:0.9rem; color:var(--im-text-muted); margin-top:0.5rem; margin-bottom:0;">
    <em>(User creates 10 WhatsApp channels over time — all get ACTIVE by default)</em>
  </p>
</div>

<div class="callout" style="border-left-color: var(--im-red);">
  <strong style="color:var(--im-red)">⬇ DOWNGRADE to Standard (max=1)</strong> — Trigger: <code>handleFeatureLimitUpdate(max=1)</code>
</div>

<div class="lifecycle-box">
  <div class="arrow-step">
    <div class="step-icon step-num">1</div>
    <div><strong>Disable ACTIVE in disabled depts</strong><br>
      <span style="color:var(--im-text-muted); font-size:0.9rem;">ACTIVE → deActivate in <strong>IM SDK</strong> → <strong>Desk DB</strong>: LICENSE_DISABLED</span>
    </div>
  </div>
  <div class="arrow-step">
    <div class="step-icon step-num">2</div>
    <div><strong>Resolve UNKNOWN in disabled depts</strong><br>
      <span style="color:var(--im-text-muted); font-size:0.9rem;">(0 found if prior event resolved them, else resolves to LICENSE_DISABLED/DISABLED)</span></div>
  </div>
  <div class="arrow-step">
    <div class="step-icon step-num">3</div>
    <div><strong>Build active count:</strong> 10 WA active in active depts</div>
  </div>
  <div class="arrow-step">
    <div class="step-icon step-num">4</div>
    <div><strong>Resolve UNKNOWN in active depts</strong><br>
      <span style="color:var(--im-text-muted); font-size:0.9rem;">(0 found if prior event resolved them, else resolves to ACTIVE/LICENSE_DISABLED/DISABLED)</span></div>
  </div>
  <div class="arrow-step">
    <div class="step-icon step-num">5</div>
    <div><strong>TRIM EXCESS:</strong><br>
      <span style="color:var(--im-text-muted); font-size:0.9rem;">
        10 active, limit 1 → excess = 9<br>
        Disable from TAIL: ch10, ch9, ch8… ch2<br>
        → deActivate in <strong>IM SDK</strong> → <strong>Desk DB</strong>: LICENSE_DISABLED<br>
        ch1 survives (first created)
      </span>
    </div>
  </div>
  <div class="arrow-step">
    <div class="step-icon step-num">6</div>
    <div><strong>Re-enable LICENSE_DISABLED?</strong><br>
      <span style="color:var(--im-text-muted); font-size:0.9rem;">0 room left → nothing restored</span>
    </div>
  </div>
  <div class="arrow-step">
    <div class="step-icon step-result">✓</div>
    <div><strong style="color:var(--im-green)">Result:</strong> 1 ACTIVE, 9 LICENSE_DISABLED (restorable on upgrade).</div>
  </div>
</div>

<!-- Lifecycle 4 -->
<h3>Lifecycle 4: Enterprise → Standard → Enterprise</h3>

<div class="lifecycle-box">
  <h4>State: Enterprise (max=50) — 10 WhatsApp channels ACTIVE</h4>
  <p style="font-size:0.9rem; color:var(--im-text-muted);">May have UNKNOWN channels if customer was Enterprise pre-migration.</p>
</div>

<div class="callout" style="border-left-color: var(--im-red);">
  <strong style="color:var(--im-red)">⬇ DOWNGRADE to Standard (max=1)</strong> — Trigger: <code>handleFeatureLimitUpdate(max=1)</code>
</div>

<div class="lifecycle-box">
  <div class="arrow-step">
    <div class="step-icon step-num">1</div>
    <div><strong>Disable ACTIVE in disabled depts</strong><br>
      <span style="color:var(--im-text-muted); font-size:0.9rem;">(0 found if no disabled depts, else ACTIVE → LICENSE_DISABLED)</span></div>
  </div>
  <div class="arrow-step">
    <div class="step-icon step-num">2</div>
    <div><strong>Resolve UNKNOWN in disabled depts</strong><br>
      <span style="color:var(--im-text-muted); font-size:0.9rem;">(0 found if prior event resolved them, else resolves to LICENSE_DISABLED/DISABLED)</span></div>
  </div>
  <div class="arrow-step">
    <div class="step-icon step-num">3</div>
    <div><strong>Build active count:</strong> 10 WA active in active depts</div>
  </div>
  <div class="arrow-step">
    <div class="step-icon step-num">4</div>
    <div><strong>Resolve UNKNOWN in active depts</strong><br>
      <span style="color:var(--im-text-muted); font-size:0.9rem;">(0 found if prior event resolved them, else resolves to ACTIVE/LICENSE_DISABLED/DISABLED)</span></div>
  </div>
  <div class="arrow-step">
    <div class="step-icon step-num">5</div>
    <div><strong>TRIM EXCESS:</strong><br>
      <span style="color:var(--im-text-muted); font-size:0.9rem;">
        Excess = 10 − 1 = 9.<br>
        ch10→ch2 → deActivate in <strong>IM SDK</strong> → <strong>Desk DB</strong>: LICENSE_DISABLED. ch1 stays ACTIVE.
      </span>
    </div>
  </div>
  <div class="arrow-step">
    <div class="step-icon step-num">6</div>
    <div><strong>Re-enable LICENSE_DISABLED?</strong> 0 room left → nothing restored</div>
  </div>
  <div class="arrow-step">
    <div class="step-icon step-result">✓</div>
    <div><strong style="color:var(--im-green)">Result:</strong> 1 ACTIVE, 9 LICENSE_DISABLED.</div>
  </div>
</div>

<div class="callout callout-success">
  <strong>⬆ UPGRADE back to Enterprise (max=50)</strong> — Trigger: <code>handleFeatureLimitUpdate(max=50)</code>
</div>

<div class="lifecycle-box">
  <div class="arrow-step">
    <div class="step-icon step-num">1</div>
    <div><strong>Disable ACTIVE in disabled depts</strong> → 0 found <span style="color:var(--im-text-muted); font-size:0.9rem;">(downgrade resolved all)</span></div>
  </div>
  <div class="arrow-step">
    <div class="step-icon step-num">2</div>
    <div><strong>Resolve UNKNOWN in disabled depts</strong> → 0 found <span style="color:var(--im-text-muted); font-size:0.9rem;">(downgrade resolved all)</span></div>
  </div>
  <div class="arrow-step">
    <div class="step-icon step-num">3</div>
    <div><strong>Build active count:</strong> 1 WA active</div>
  </div>
  <div class="arrow-step">
    <div class="step-icon step-num">4</div>
    <div><strong>Resolve UNKNOWN in active depts</strong> → 0 found <span style="color:var(--im-text-muted); font-size:0.9rem;">(downgrade resolved all)</span></div>
  </div>
  <div class="arrow-step">
    <div class="step-icon step-num">5</div>
    <div><strong>Trim?</strong> 1 active, limit 50 → no excess</div>
  </div>
  <div class="arrow-step">
    <div class="step-icon step-num">6</div>
    <div><strong>Re-enable LICENSE_DISABLED in active depts</strong><br>
      <span style="color:var(--im-text-muted); font-size:0.9rem;">
        Room = 50 − 1 = 49.<br>
        9 LICENSE_DISABLED → activate in <strong>IM SDK</strong> → <strong>Desk DB</strong>: ACTIVE.
      </span>
    </div>
  </div>
  <div class="arrow-step">
    <div class="step-icon step-result">✓</div>
    <div><strong style="color:var(--im-green)">Result:</strong> All 10 channels active again. Nothing lost.</div>
  </div>
</div>

<hr class="section-divider">

<!-- ═══════════════════════════════════════════════════════ -->
<h2>4. Department Enable/Disable Lifecycles</h2>

<!-- Lifecycle 5a -->
<h3>Lifecycle 5a: Dept ON → Disable → Enable (with UNKNOWN channels)</h3>
<p style="font-size:0.9rem; color:var(--im-text-muted);">
  This happens when the department event is the <strong>first event</strong> to process these channels after migration. No license event has run yet for this org.
</p>

<div class="lifecycle-box">
  <h4>State: Dept 1 is ON</h4>
  <p style="font-size:0.9rem; color:var(--im-text-muted);">Has 3 WA channels, all <span class="badge badge-unknown">UNKNOWN</span> (from dd-changes migration). No license event has processed this org yet.</p>
</div>

<div class="callout" style="border-left-color: var(--im-red);">
  <strong style="color:var(--im-red)">DISABLE Department 1</strong> — Trigger: Kafka async → <code>DepartmentDisableIMChannelHandler</code><br>
  Handler fetches: <strong>ACTIVE + UNKNOWN</strong>
</div>

<div class="lifecycle-box">
  <div class="arrow-step">
    <div class="step-icon step-num">1</div>
    <div><strong>ACTIVE channels:</strong> none found (all are UNKNOWN)</div>
  </div>
  <div class="arrow-step">
    <div class="step-icon step-num">2</div>
    <div><strong>Resolve UNKNOWN channels:</strong><br>
      <span style="color:var(--im-text-muted); font-size:0.9rem;">
        ch1 (UNKNOWN) → check SDK → active? deActivate in <strong>IM SDK</strong> → <strong>Desk DB</strong>: DISABLED<br>
        ch2 (UNKNOWN) → same → <strong>Desk DB</strong>: DISABLED<br>
        ch3 (UNKNOWN) → same → <strong>Desk DB</strong>: DISABLED
      </span>
    </div>
  </div>
  <div class="callout callout-warn" style="margin-top:0.75rem;">
    <strong>Note:</strong> Dept handler always sets <span class="badge badge-disabled">DISABLED</span> (not LICENSE_DISABLED). All UNKNOWN resolved.
  </div>
  <div class="arrow-step">
    <div class="step-icon step-result">✓</div>
    <div><strong style="color:var(--im-green)">Result:</strong> 0 ACTIVE, 3 DISABLED in dept 1.</div>
  </div>
</div>

<div class="callout callout-success">
  <strong>ENABLE Department 1</strong> — Trigger: Sync call → <code>DepartmentEnableIMChannelHandler</code><br>
  Handler fetches: <strong>DISABLED + UNKNOWN</strong>
</div>

<div class="lifecycle-box">
  <div class="arrow-step">
    <div class="step-icon step-num">1</div>
    <div><strong>Check license limit:</strong> maxPerIST = 0? → Exit (Free plan)</div>
  </div>
  <div class="arrow-step">
    <div class="step-icon step-num">2</div>
    <div><strong>Restore DISABLED channels:</strong><br>
      <span style="color:var(--im-text-muted); font-size:0.9rem;">
        Group by IST. room = maxPerIST − currentActiveCount (across ALL depts)<br>
        ch1 (DISABLED) → activate in <strong>IM SDK</strong> → <strong>Desk DB</strong>: ACTIVE. Room decremented.<br>
        ch2 (DISABLED) → room left? Yes → activate in <strong>IM SDK</strong> → <strong>Desk DB</strong>: ACTIVE<br>
        ch3 (DISABLED) → room left? No → stays DISABLED
      </span>
    </div>
  </div>
  <div class="arrow-step">
    <div class="step-icon step-num">3</div>
    <div><strong>No UNKNOWN to resolve</strong> (all resolved during disable)</div>
  </div>
  <div class="arrow-step">
    <div class="step-icon step-result">✓</div>
    <div><strong style="color:var(--im-green)">Result:</strong> Channels come back (within license limit).</div>
  </div>
</div>

<!-- Lifecycle 5b -->
<h3>Lifecycle 5b: Dept ON → Disable → Enable (with LICENSE_DISABLED channels)</h3>
<p style="font-size:0.9rem; color:var(--im-text-muted);">
  This happens when a license downgrade already ran on this org, creating LICENSE_DISABLED channels in this active department. Then later the dept gets disabled.
</p>

<div class="lifecycle-box">
  <h4>State: Dept 1 is ON, Enterprise plan (max=50)</h4>
  <p style="font-size:0.9rem; color:var(--im-text-muted);">
    Has 5 WA channels: 4 <span class="badge badge-active">ACTIVE</span> + 1 <span class="badge badge-lic-dis">LICENSE_DISABLED</span>
    <br>(the 1 LICENSE_DISABLED was set by a previous license downgrade)
  </p>
</div>

<div class="callout" style="border-left-color: var(--im-red);">
  <strong style="color:var(--im-red)">DISABLE Department 1</strong> — Trigger: Kafka async → <code>DepartmentDisableIMChannelHandler</code><br>
  Handler fetches: <strong>ACTIVE + UNKNOWN only</strong>. LICENSE_DISABLED is invisible.
</div>

<div class="lifecycle-box">
  <div class="arrow-step">
    <div class="step-icon step-num">1</div>
    <div><strong>ACTIVE channels:</strong><br>
      <span style="color:var(--im-text-muted); font-size:0.9rem;">ch1-ch4 (ACTIVE) → deActivate in <strong>IM SDK</strong> → <strong>Desk DB</strong>: DISABLED</span>
    </div>
  </div>
  <div class="arrow-step">
    <div class="step-icon step-num">2</div>
    <div><strong>Resolve UNKNOWN</strong> → 0 found <span style="color:var(--im-text-muted); font-size:0.9rem;">(all resolved by prior license event)</span></div>
  </div>
  <div class="callout callout-warn" style="margin-top:0.5rem;">
    <strong>Note:</strong> LICENSE_DISABLED channel (ch5) is NOT fetched. NOT processed. Stays LICENSE_DISABLED. Dept handler is blind to LICENSE_DISABLED.
  </div>
  <div class="arrow-step">
    <div class="step-icon step-result">✓</div>
    <div><strong style="color:var(--im-green)">Result:</strong> 4 DISABLED + 1 LICENSE_DISABLED in dept 1.</div>
  </div>
</div>

<div class="callout callout-success">
  <strong>ENABLE Department 1</strong> — Trigger: Sync call → <code>DepartmentEnableIMChannelHandler</code><br>
  Handler fetches: <strong>DISABLED + UNKNOWN only</strong>. LICENSE_DISABLED is invisible.
</div>

<div class="lifecycle-box">
  <div class="arrow-step">
    <div class="step-icon step-num">1</div>
    <div><strong>Check license limit:</strong> maxPerIST = 50 → proceed</div>
  </div>
  <div class="arrow-step">
    <div class="step-icon step-num">2</div>
    <div><strong>Restore DISABLED channels (room = 50 − activeCount):</strong><br>
      <span style="color:var(--im-text-muted); font-size:0.9rem;">ch1-ch4 (DISABLED) → activate in <strong>IM SDK</strong> → <strong>Desk DB</strong>: ACTIVE</span>
    </div>
  </div>
  <div class="arrow-step">
    <div class="step-icon step-num">3</div>
    <div><strong>Resolve UNKNOWN</strong> → 0 found <span style="color:var(--im-text-muted); font-size:0.9rem;">(all resolved by prior license event)</span></div>
  </div>
  <div class="callout callout-warn" style="margin-top:0.5rem;">
    <strong>Note:</strong> LICENSE_DISABLED channel (ch5) is NOT fetched. NOT processed. Stays LICENSE_DISABLED.
  </div>
  <div class="arrow-step">
    <div class="step-icon step-result">⚠</div>
    <div><strong style="color:var(--im-green)">Result:</strong> 4 ACTIVE + 1 LICENSE_DISABLED → now also restored!<br>
      <span style="font-size:0.9rem; color:var(--im-text-muted);">
        <strong>FIXED:</strong> DeptEnableHandler now fetches LICENSE_DISABLED and restores if room available.<br>
        ch5 (LICENSE_DISABLED) → activate in <strong>IM SDK</strong> → <strong>Desk DB</strong>: ACTIVE. Final: 5 ACTIVE.
      </span>
    </div>
  </div>
</div>

<!-- Lifecycle 6a -->
<h3>Lifecycle 6a: Dept OFF → Enable → Disable (with UNKNOWN channels)</h3>
<p style="font-size:0.9rem; color:var(--im-text-muted);">
  Department was disabled BEFORE migration. dd-changes set all its channels to UNKNOWN. Now the dept gets enabled for the first time.
</p>

<div class="lifecycle-box">
  <h4>State: Dept 2 is OFF</h4>
  <p style="font-size:0.9rem; color:var(--im-text-muted);">
    Has 3 WA channels, all <span class="badge badge-unknown">UNKNOWN</span> (from dd-changes migration). No handler has ever processed these.
  </p>
</div>

<div class="callout callout-success">
  <strong>ENABLE Department 2</strong> — Trigger: Sync call → <code>DepartmentEnableIMChannelHandler</code><br>
  Handler fetches: <strong>DISABLED + UNKNOWN</strong><br>
  maxPerIST=5, currently 3 WA active (in other depts). Room = 5 − 3 = 2.
</div>

<div class="lifecycle-box">
  <div class="arrow-step">
    <div class="step-icon step-num">1</div>
    <div><strong>Check license limit:</strong> maxPerIST = 5 → proceed</div>
  </div>
  <div class="arrow-step">
    <div class="step-icon step-num">2</div>
    <div><strong>No DISABLED channels to restore</strong></div>
  </div>
  <div class="arrow-step">
    <div class="step-icon step-num">3</div>
    <div><strong>Resolve UNKNOWN channels (room=2):</strong><br>
      <span style="color:var(--im-text-muted); font-size:0.9rem;">
        ch1 (UNKNOWN) → SDK active? Yes + room → <strong>Desk DB</strong>: ACTIVE. Room left: 1<br>
        ch2 (UNKNOWN) → SDK active? Yes + room → <strong>Desk DB</strong>: ACTIVE. Room left: 0<br>
        ch3 (UNKNOWN) → SDK active? Yes + <strong>no room</strong> → deActivate in <strong>IM SDK</strong> → <strong>Desk DB</strong>: LICENSE_DISABLED<br>
        <em style="font-size:0.85rem;">(or if not in SDK → Desk DB: DISABLED)</em>
      </span>
    </div>
  </div>
  <div class="arrow-step">
    <div class="step-icon step-result">✓</div>
    <div><strong style="color:var(--im-green)">Result:</strong> 2 ACTIVE (ch1,ch2) + 1 LICENSE_DISABLED (ch3). All UNKNOWN resolved.</div>
  </div>
</div>

<div class="callout" style="border-left-color: var(--im-red);">
  <strong style="color:var(--im-red)">DISABLE Department 2</strong> — Trigger: Kafka async → <code>DepartmentDisableIMChannelHandler</code><br>
  Handler fetches: <strong>ACTIVE + UNKNOWN only</strong>. LICENSE_DISABLED is invisible.
</div>

<div class="lifecycle-box">
  <div class="arrow-step">
    <div class="step-icon step-num">1</div>
    <div><strong>ACTIVE channels:</strong><br>
      <span style="color:var(--im-text-muted); font-size:0.9rem;">ch1, ch2 (ACTIVE) → deActivate in <strong>IM SDK</strong> → <strong>Desk DB</strong>: DISABLED</span>
    </div>
  </div>
  <div class="arrow-step">
    <div class="step-icon step-num">2</div>
    <div><strong>Resolve UNKNOWN</strong> → 0 found <span style="color:var(--im-text-muted); font-size:0.9rem;">(all resolved during enable)</span></div>
  </div>
  <div class="callout callout-warn" style="margin-top:0.5rem;">
    <strong>Note:</strong> LICENSE_DISABLED channel (ch3) is NOT fetched. NOT processed. Stays LICENSE_DISABLED.
  </div>
  <div class="arrow-step">
    <div class="step-icon step-result">✓</div>
    <div><strong style="color:var(--im-green)">Result:</strong> 2 DISABLED + 1 LICENSE_DISABLED in dept 2.</div>
  </div>
</div>

<!-- Lifecycle 6b -->
<h3>Lifecycle 6b: Dept OFF → Enable → Disable (with LICENSE_DISABLED channels)</h3>
<p style="font-size:0.9rem; color:var(--im-text-muted);">
  A license downgrade created LICENSE_DISABLED channels while the dept was ON, then the dept was later disabled. All UNKNOWN were already resolved by the license event.
</p>

<div class="lifecycle-box">
  <h4>State: Dept 2 is OFF</h4>
  <p style="font-size:0.9rem; color:var(--im-text-muted);">
    Has 4 WA channels:<br>
    • ch1, ch2, ch3 → <span class="badge badge-disabled">DISABLED</span> (set by dept disable handler)<br>
    • ch4 → <span class="badge badge-lic-dis">LICENSE_DISABLED</span> (set by license downgrade before dept was disabled)
  </p>
</div>

<div class="callout callout-success">
  <strong>ENABLE Department 2</strong> — Trigger: Sync call → <code>DepartmentEnableIMChannelHandler</code><br>
  Handler fetches: <strong>DISABLED + UNKNOWN only</strong>. LICENSE_DISABLED is invisible.<br>
  maxPerIST=5, currently 2 WA active (in other depts). Room = 5 − 2 = 3.
</div>

<div class="lifecycle-box">
  <div class="arrow-step">
    <div class="step-icon step-num">1</div>
    <div><strong>Check license limit:</strong> maxPerIST = 5 → proceed</div>
  </div>
  <div class="arrow-step">
    <div class="step-icon step-num">2</div>
    <div><strong>Restore DISABLED channels (room=3):</strong><br>
      <span style="color:var(--im-text-muted); font-size:0.9rem;">
        ch1 (DISABLED) → activate in <strong>IM SDK</strong> → <strong>Desk DB</strong>: ACTIVE. Room left: 2<br>
        ch2 (DISABLED) → activate in <strong>IM SDK</strong> → <strong>Desk DB</strong>: ACTIVE. Room left: 1<br>
        ch3 (DISABLED) → activate in <strong>IM SDK</strong> → <strong>Desk DB</strong>: ACTIVE. Room left: 0
      </span>
    </div>
  </div>
  <div class="arrow-step">
    <div class="step-icon step-num">3</div>
    <div><strong>No UNKNOWN to resolve</strong></div>
  </div>
  <div class="arrow-step">
    <div class="step-icon step-num">4</div>
    <div><strong>LICENSE_DISABLED channel (ch4):</strong><br>
      <span style="color:var(--im-text-muted); font-size:0.9rem;">NOT fetched. NOT processed. Stays LICENSE_DISABLED.</span>
    </div>
  </div>
  <div class="arrow-step">
    <div class="step-icon step-result">⚠</div>
    <div><strong style="color:var(--im-green)">Result:</strong> 3 ACTIVE + 1 LICENSE_DISABLED → now also restored!<br>
      <span style="font-size:0.9rem; color:var(--im-text-muted);">
        <strong>FIXED:</strong> DeptEnableHandler now fetches LICENSE_DISABLED and restores if room available.<br>
        ch4 (LICENSE_DISABLED) → activate in <strong>IM SDK</strong> → <strong>Desk DB</strong>: ACTIVE. Final: 4 ACTIVE.
      </span>
    </div>
  </div>
</div>

<div class="callout" style="border-left-color: var(--im-red);">
  <strong style="color:var(--im-red)">DISABLE Department 2</strong> — Trigger: Kafka async → <code>DepartmentDisableIMChannelHandler</code><br>
  Handler fetches: <strong>ACTIVE + UNKNOWN only</strong>. LICENSE_DISABLED is invisible.
</div>

<div class="lifecycle-box">
  <div class="arrow-step">
    <div class="step-icon step-num">1</div>
    <div><strong>ACTIVE channels:</strong><br>
      <span style="color:var(--im-text-muted); font-size:0.9rem;">ch1, ch2, ch3 (ACTIVE) → deActivate in <strong>IM SDK</strong> → <strong>Desk DB</strong>: DISABLED</span>
    </div>
  </div>
  <div class="arrow-step">
    <div class="step-icon step-num">2</div>
    <div><strong>Resolve UNKNOWN</strong> → 0 found <span style="color:var(--im-text-muted); font-size:0.9rem;">(all resolved by prior license event)</span></div>
  </div>
  <div class="callout callout-warn" style="margin-top:0.5rem;">
    <strong>Note:</strong> LICENSE_DISABLED channel (ch4) is NOT fetched. NOT processed. Stays LICENSE_DISABLED (still stranded).
  </div>
  <div class="arrow-step">
    <div class="step-icon step-result">✓</div>
    <div><strong style="color:var(--im-green)">Result:</strong> 3 DISABLED + 1 LICENSE_DISABLED (ch4 still stranded) in dept 2.</div>
  </div>
</div>

<hr class="section-divider">

<!-- ═══════════════════════════════════════════════════════ -->
<h2>5. Manual Channel Operations</h2>

<div class="lifecycle-box">
  <h4>Manual Activate (REST API → <code>activateChannel</code>)</h4>
  <p style="font-size:0.9rem; color:var(--im-text-muted);">User clicks "Activate" on a channel in UI.</p>
  <div class="arrow-step">
    <div class="step-icon step-num">1</div>
    <div><strong>Lookup channel</strong> by externalId</div>
  </div>
  <div class="arrow-step">
    <div class="step-icon step-num">2</div>
    <div><strong>Check license:</strong><br>
      <span style="color:var(--im-text-muted); font-size:0.9rem;">
        Already ACTIVE? → Skip check<br>
        UNKNOWN + active in IM SDK? → Skip check (just resolving status)<br>
        DISABLED / LICENSE_DISABLED → Check license limit → Throws <code>DeskLicenseException</code> if over limit
      </span>
    </div>
  </div>
  <div class="arrow-step">
    <div class="step-icon step-num">3</div>
    <div><strong>Activate</strong> in <strong>IM SDK</strong></div>
  </div>
  <div class="arrow-step">
    <div class="step-icon step-num">4</div>
    <div><strong>Desk DB</strong> → ACTIVE</div>
  </div>
  <div class="arrow-step">
    <div class="step-icon step-result">✓</div>
    <div><strong>Result:</strong> IM SDK: active, Desk DB: ACTIVE</div>
  </div>
</div>

<div class="lifecycle-box">
  <h4>Manual Deactivate (REST API → <code>deActivateChannel</code>)</h4>
  <p style="font-size:0.9rem; color:var(--im-text-muted);">User clicks "Deactivate" on a channel in UI.</p>
  <div class="arrow-step">
    <div class="step-icon step-num">1</div>
    <div><strong>Deactivate</strong> in <strong>IM SDK</strong></div>
  </div>
  <div class="arrow-step">
    <div class="step-icon step-num">2</div>
    <div><strong>Redis cleanup:</strong> Delete <code>channelId + ONLINE_AGENTS</code></div>
  </div>
  <div class="arrow-step">
    <div class="step-icon step-num">3</div>
    <div><strong>Desk DB</strong> → DISABLED (always DISABLED, never LICENSE_DISABLED)</div>
  </div>
  <div class="arrow-step">
    <div class="step-icon step-result">✓</div>
    <div><strong>Result:</strong> IM SDK: inactive, Desk DB: DISABLED, Redis: cache cleared</div>
  </div>
</div>

<div class="lifecycle-box">
  <h4>Lean Activate/Deactivate (Internal — used by license &amp; dept handlers)</h4>
  <p style="font-size:0.9rem; color:var(--im-text-muted);">
    Handlers call these internally, then update Desk DB themselves via <code>channelQueryAPI.updateChannelStatus()</code>.
  </p>
  <table>
    <tr><th>Operation</th><th>What it does</th><th>What it does NOT do</th></tr>
    <tr>
      <td><code>activate(channelId)</code></td>
      <td>IM SDK only</td>
      <td>No license check. No Desk DB update.</td>
    </tr>
    <tr>
      <td><code>deActivate(channelId)</code></td>
      <td>IM SDK only</td>
      <td>No Desk DB. No Redis cleanup.</td>
    </tr>
  </table>
  <div class="callout" style="margin-top:1rem;">
    <strong>Why separate?</strong> Each caller sets a <em>different</em> Desk DB status after the same SDK call:
  </div>
  <table style="margin-top:0.5rem;">
    <tr><th>Caller</th><th>IM SDK call</th><th>Desk DB status</th><th>Redis cleanup?</th></tr>
    <tr><td>License handler</td><td>deActivate</td><td>LICENSE_DISABLED (2)</td><td>No</td></tr>
    <tr><td>Dept disable handler</td><td>deActivate</td><td>DISABLED (0)</td><td>No</td></tr>
    <tr><td>Manual deactivate</td><td>deActivate</td><td>DISABLED (0)</td><td>Yes</td></tr>
    <tr><td>License handler</td><td>activate</td><td>ACTIVE (1)</td><td>No</td></tr>
    <tr><td>Dept enable handler</td><td>activate</td><td>ACTIVE (1)</td><td>No</td></tr>
    <tr><td>Manual activate</td><td>activate</td><td>ACTIVE (1)</td><td>No</td></tr>
  </table>
</div>

<hr class="section-divider">

<!-- ═══════════════════════════════════════════════════════ -->
<h2>6. Who Calls What (Trigger Map)</h2>

<div class="trigger-card">
  <h4>Plan Change (upgrade / downgrade / enable / disable)</h4>
  <div class="trigger-path">License Framework → IMChannelLicenseHandler → handleFeatureEnable / handleFeatureDisable / handleFeatureLimitUpdate</div>
  <div class="trigger-uses">Uses: lean activate/deActivate + updateChannelStatus</div>
</div>

<div class="trigger-card">
  <h4>Department Disabled</h4>
  <div class="trigger-path">Kafka async (departmentOffload.json) → DepartmentDisableIMChannelHandler → disableHandler(MicrozEvent)</div>
  <div class="trigger-uses">Uses: lean deActivate + updateChannelStatus(DISABLED)</div>
</div>

<div class="trigger-card">
  <h4>Department Enabled</h4>
  <div class="trigger-path">Sync call from DepartmentAPIImpl.enable() → DepartmentEnableIMChannelHandler → enableChannels(deptId)</div>
  <div class="trigger-uses">Uses: lean activate + updateChannelStatus(ACTIVE)</div>
</div>

<div class="trigger-card">
  <h4>User Clicks Activate/Deactivate in UI</h4>
  <div class="trigger-path">REST endpoint → DeskIMChannelAuthorizedAPIImpl → activateChannel / deActivateChannel</div>
  <div class="trigger-uses">Full flow: license check + SDK + DB + Redis</div>
</div>

<div class="trigger-card">
  <h4>License Usage Query</h4>
  <div class="trigger-path">License Framework → IMChannelLicenseHandler.getUsedCount() → deskIMChannelReadOnlyAPI.getChannelsCount()</div>
  <div class="trigger-uses">Read-only: returns active channel count per IST</div>
</div>

<div class="callout">
  <strong>Execution Order:</strong>
  Department handler (<code>order=1</code>) runs FIRST.
  License handler (<code>order=5</code>) runs AFTER.
  <br><br>
  <em>Why?</em> License handler assumes departments are already enabled/disabled by the time it runs.
  It uses <code>getAllActiveDepartments()</code> to split channels into "active dept" vs "disabled dept" buckets.
</div>

<hr class="section-divider">

<!-- ═══════════════════════════════════════════════════════ -->
<h2>7. DISABLED vs LICENSE_DISABLED — The Key Distinction</h2>

<div class="comparison-grid">
  <div class="comparison-card card-disabled">
    <h4 style="color:var(--im-text-muted);">DISABLED (0)</h4>
    <ul>
      <li>Set by: Manual deactivate, Dept disable</li>
      <li>Meaning: User/admin chose to turn this off</li>
      <li>License handler: Does NOT touch these</li>
      <li>Dept enable: WILL try to restore</li>
      <li>Manual activate: WILL work (with license check)</li>
    </ul>
  </div>
  <div class="comparison-card card-lic-dis">
    <h4 style="color:var(--im-red);">LICENSE_DISABLED (2)</h4>
    <ul>
      <li>Set by: License limit exceeded, Feature disabled</li>
      <li>Meaning: Channel is fine, license prevents it</li>
      <li>License handler: WILL restore on limit increase</li>
      <li>Dept enable: Does NOT touch these</li>
      <li>Manual activate: WILL work (with license check)</li>
    </ul>
  </div>
</div>

<div class="callout callout-success">
  <strong>Why this matters:</strong><br>
  Enterprise(50) → Standard(1): 49 channels become LICENSE_DISABLED.<br>
  Standard(1) → Enterprise(50): Those 49 <em>auto-restore</em> to ACTIVE.<br><br>
  If we used DISABLED instead, the license handler wouldn't know which channels to restore on upgrade.
</div>

<hr class="section-divider">

<!-- ═══════════════════════════════════════════════════════ -->
<h2>8. SDK Truth Source — SDKChannelMapBuilder</h2>

<div class="flow-diagram">
               ┌──────────────┐
               │   <span style="color:var(--im-cyan)">IM SDK</span>     │
               │ (ZohoIM API) │
               └──────┬───────┘
                      │
           sdkChannelMapProvider.build()
                      │
               ┌──────▼───────┐
               │  Map&lt;Long,   │
               │ <span style="color:var(--im-purple)">SDKChannelInfo</span>&gt;│
               │              │
               │ key: channelId│
               │ val: <span style="color:var(--im-cyan)">IST</span> +   │
               │      <span style="color:var(--im-green)">isActive</span> │
               └──────────────┘
</div>

<div class="callout">
  <strong>Used for:</strong><br>
  • Determining which IST a channel belongs to (WhatsApp, Telegram, etc.)<br>
  • Checking if UNKNOWN channel is actually active in SDK<br>
  • Grouping channels by IST for per-IST limit enforcement<br><br>
  Not directly unit-testable (static SDK calls).
  Callers mock <code>SDKChannelMapProvider</code> interface instead.
</div>

<hr class="section-divider">

<!-- ═══════════════════════════════════════════════════════ -->
<h2>9. Complete Visual: Every Transition</h2>

<div class="flow-diagram" style="text-align:center;">
                         ┌─────────┐
                         │  <span style="color:var(--im-text-muted)">FREE</span>   │
                         │ (max=0) │
                         └────┬────┘
                              │
              <span style="color:var(--im-green)">handleFeatureEnable(max=1)</span>
                              │
                         ┌────▼────┐
                         │<span style="color:var(--im-accent)">STANDARD</span> │
                         │ (max=1) │
                         └────┬────┘
                              │
              <span style="color:var(--im-green)">handleFeatureLimitUpdate(max=10)</span>
                              │
                         ┌────▼────┐
                         │  <span style="color:var(--im-purple)">PROF</span>   │
                         │(max=10) │
                         └────┬────┘
                              │
              <span style="color:var(--im-green)">handleFeatureLimitUpdate(max=50)</span>
                              │
                         ┌────▼─────┐
                         │<span style="color:var(--im-green)">ENTERPRISE</span>│
                         │ (max=50) │
                         └──────────┘

   Each transition:

     ↑ <span style="color:var(--im-green)">UPGRADE</span> = handleFeatureLimitUpdate
                  (or handleFeatureEnable for Free→Paid)
       → Resolve UNKNOWN → Trim excess → Re-enable LICENSE_DISABLED

     ↓ <span style="color:var(--im-red)">DOWNGRADE</span> = handleFeatureLimitUpdate
                   (or handleFeatureDisable for Paid→Free)
       → Disable excess → Resolve UNKNOWN → LICENSE_DISABLE remaining
</div>

</div>
