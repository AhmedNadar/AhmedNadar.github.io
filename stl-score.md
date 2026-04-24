---
layout: page
title: Speed-to-Lead Score
permalink: /stl-score/
description: "Find out how much your business is losing every month from slow lead follow-up. Free, 30 seconds, no spam."
---

<p class="page-description">
  Tell me three things about your business. I will tell you how much money slow follow-up is costing you per month — based on the same data Harvard Business Review used to study 2.24 million leads.
</p>

<div class="stl-calc">
  <form id="stl-score-form" class="stl-form" novalidate>

    <div style="position:absolute;left:-9999px" aria-hidden="true">
      <input type="text" name="website" tabindex="-1" autocomplete="off">
    </div>

    <div class="stl-row">
      <label for="stl-leads">How many quote requests do you get per month?</label>
      <input type="number" id="stl-leads" name="leads_per_month" min="1" max="500" inputmode="numeric" value="20" required>
      <span class="stl-hint">Forms, calls, DMs, walk-ins — everything counted.</span>
    </div>

    <div class="stl-row">
      <label for="stl-hours">On average, how long until you reply?</label>
      <select id="stl-hours" name="reply_hours" required>
        <option value="0.08">Under 5 minutes</option>
        <option value="0.5">Under 30 minutes</option>
        <option value="2">1–2 hours</option>
        <option value="6">Same day (6 hours)</option>
        <option value="24">Next day (24 hours)</option>
        <option value="42" selected>1–2 days (industry average)</option>
        <option value="72">3+ days</option>
      </select>
    </div>

    <div class="stl-row">
      <label for="stl-job">What is your average job size?</label>
      <select id="stl-job" name="job_size" required>
        <option value="500">$500 (small repair / service call)</option>
        <option value="2000">$2,000 (small project)</option>
        <option value="8000" selected>$8,000 (medium reno / install)</option>
        <option value="25000">$25,000 (kitchen / bath)</option>
        <option value="60000">$60,000 (full renovation)</option>
        <option value="150000">$150,000+ (custom build / major project)</option>
      </select>
    </div>

    <!-- Priority 2: result block with narrative output -->
    <div class="stl-result" id="stl-result" aria-live="polite">
      <div class="stl-result-label">Estimated monthly revenue lost</div>
      <div class="stl-result-amount" id="stl-result-amount">$0</div>
      <div class="stl-result-annual" id="stl-result-annual" style="display:none;"></div>
      <div class="stl-result-hint" id="stl-result-hint">Move the sliders above to see your number.</div>
      <div class="stl-result-narrative" id="stl-result-narrative" style="display:none;"></div>
    </div>

    <!-- Priority 1: dual-CTA cards, shown when result > 0 -->
    <div id="stl-cta-cards" class="stl-cta-cards" style="display:none;">
      <div class="stl-card stl-card-high">
        <div class="stl-card-head">Skip ahead — book a 20-min audit call</div>
        <p class="stl-card-body">I'll run this calculation with your actual form-submission data, identify the top 3 leaks, and give you a fix plan on the call. $497, full refund if it wasn't worth it.</p>
        <a href="https://cal.com/ahmed-nadar/stl" class="cta-button stl-audit-cta" id="stl-audit-btn" target="_blank" rel="noopener">Book the audit call →</a>
      </div>
      <div class="stl-card stl-card-low">
        <div class="stl-card-head">Not ready yet? Get the free report</div>
        <p class="stl-card-body">I'll send you a personalized 1-page plan within 24 hours. No spam — one email when the report is ready.</p>
        <button type="button" class="cta-button stl-report-cta" id="stl-report-btn">Send me the free report</button>
      </div>
    </div>

    <!-- Form fields: progressively disclosed when report card is clicked -->
    <div id="stl-form-fields" style="display:none;">
      <hr class="stl-divider">
      <p class="stl-pitch">
        <strong>One more step.</strong> Drop your email and I will send you a free
        <em>What to fix in 7 days</em> PDF tailored to a business your size. No spam — Toronto contractors only,
        one email when the report is ready.
      </p>

      <div class="stl-row">
        <label for="stl-email">Your email</label>
        <input type="email" id="stl-email" name="email" autocomplete="email" placeholder="you@yourcompany.com">
      </div>

      <div class="stl-row">
        <label for="stl-name">Your name (optional)</label>
        <input type="text" id="stl-name" name="name" autocomplete="name" placeholder="First name">
      </div>

      <div class="stl-row">
        <label for="stl-business">Business name (optional)</label>
        <input type="text" id="stl-business" name="business" autocomplete="organization" placeholder="Your business">
      </div>

      <div class="stl-row">
        <label for="stl-phone">Phone (optional — fastest way to follow up)</label>
        <input type="tel" id="stl-phone" name="phone" autocomplete="tel" inputmode="tel" placeholder="(416) 555-0199" pattern="[\d\s\+\-\(\)\.]{7,30}">
        <span class="stl-hint">If you give me your phone, you also get an instant AI reply within seconds — see how fast STL feels from the customer's side.</span>
      </div>

      <button type="submit" class="cta-button stl-submit">Send me the free report</button>
      <p class="stl-fineprint">By submitting you also subscribe to the STL newsletter — one short email a week with one Toronto SMB AI playbook. Unsubscribe any time.</p>
    </div>

  </form>

  <!-- Priority 4: instant success state — replaces "check back tomorrow" -->
  <div id="stl-success" class="stl-feedback stl-success-rich" style="display: none;">
    <h2>Here's what I can already tell you:</h2>
    <div id="stl-success-number" class="stl-success-number"></div>
    <div class="stl-success-section">
      <h3>Your 3 biggest leaks right now</h3>
      <ul id="stl-success-leaks" class="stl-success-list"></ul>
    </div>
    <div class="stl-success-section">
      <h3>What to fix in the next 7 days</h3>
      <ol id="stl-success-fixes" class="stl-success-list stl-success-fixes-list"></ol>
    </div>
    <p id="stl-success-dogfood" style="display:none;"><strong>If you gave me your phone, check your inbox in the next 60 seconds.</strong> You will get an instant AI reply — that is exactly what your customers experience when STL is running on your business. Same speed. Same warmth.</p>
    <p class="stl-success-footer">Your full PDF is in the queue — you will get it within 24 hours. If you want the audit call version, <a href="https://cal.com/ahmed-nadar/stl">book here.</a></p>
  </div>

  <div id="stl-error" class="stl-feedback stl-feedback-error" style="display: none;">
    <p>Something went wrong submitting the form. Email me directly at <a href="mailto:ahmed@ahmednadar.com">ahmed@ahmednadar.com</a> and I will send the report manually.</p>
  </div>
</div>

<style>
  .stl-calc { max-width: 640px; margin: var(--space-md) auto var(--space-xl); }
  .stl-form { display: flex; flex-direction: column; gap: var(--space-sm); }
  .stl-row { display: flex; flex-direction: column; gap: 0.35rem; }
  .stl-row label { font-size: var(--text-sm); font-weight: 600; color: var(--color-text); }
  .stl-row input, .stl-row select {
    font: inherit; padding: 0.7rem 0.85rem; border: 1px solid var(--color-border, #d8d8d2);
    border-radius: 8px; background: var(--color-bg, #fff); color: var(--color-text);
  }
  .stl-row input:focus, .stl-row select:focus { outline: 2px solid var(--color-accent); outline-offset: 1px; }
  .stl-hint { font-size: 0.78rem; color: var(--color-text-muted); }

  /* Result block */
  .stl-result {
    margin-top: var(--space-sm);
    padding: var(--space-md);
    border: 1px solid var(--color-border, #e2e2dc);
    border-radius: 12px;
    background: var(--color-surface, #fafaf6);
    text-align: center;
  }
  .stl-result-label { font-size: 0.78rem; text-transform: uppercase; letter-spacing: 0.12em; color: var(--color-text-muted); margin-bottom: 0.4rem; }
  .stl-result-amount { font-size: 2.6rem; font-weight: 800; color: var(--color-accent); line-height: 1.1; }
  .stl-result-hint { font-size: 0.85rem; color: var(--color-text-muted); margin-top: 0.5rem; }

  /* Priority 2: annual line + narrative */
  .stl-result-annual {
    font-size: 0.95rem;
    color: var(--color-text-muted);
    margin-top: 0.35rem;
  }
  .stl-result-narrative {
    font-size: 0.92rem;
    color: var(--color-text);
    margin-top: 0.85rem;
    padding-top: 0.85rem;
    border-top: 1px solid var(--color-border, #e2e2dc);
    text-align: left;
    line-height: 1.65;
  }

  /* Priority 1: dual-CTA cards */
  .stl-cta-cards {
    display: flex;
    gap: var(--space-sm, 1rem);
    margin-top: var(--space-md);
  }
  .stl-card {
    flex: 1;
    display: flex;
    flex-direction: column;
    gap: 0.75rem;
    padding: var(--space-md);
    border: 1px solid var(--color-border, #e2e2dc);
    border-radius: 12px;
    background: var(--color-surface, #fafaf6);
  }
  .stl-card-high { border-color: var(--color-accent); }
  .stl-card-head {
    font-size: 0.97rem;
    font-weight: 700;
    color: var(--color-text);
    line-height: 1.3;
  }
  .stl-card-body {
    font-size: 0.86rem;
    color: var(--color-text-muted);
    flex: 1;
    line-height: 1.55;
    margin: 0;
  }
  .stl-audit-cta { align-self: flex-start; }
  .stl-report-cta {
    align-self: flex-start;
    background: transparent;
    color: var(--color-accent);
    border: 2px solid var(--color-accent);
    cursor: pointer;
  }
  .stl-report-cta:hover {
    background: var(--color-accent);
    color: #fff;
  }
  @media (max-width: 560px) {
    .stl-cta-cards { flex-direction: column; }
  }

  /* Form progressive disclosure */
  .stl-divider { border: none; border-top: 1px solid var(--color-border, #e2e2dc); margin: var(--space-md) 0; }
  .stl-pitch { font-size: 0.95rem; color: var(--color-text); }
  .stl-fineprint { font-size: 0.75rem; color: var(--color-text-muted); margin-top: 0.6rem; }
  .stl-submit { margin-top: 0.5rem; align-self: flex-start; }

  /* Shared feedback panel */
  .stl-feedback { margin-top: var(--space-md); padding: var(--space-md); background: var(--color-surface, #fafaf6); border-radius: 12px; }
  .stl-feedback-error { background: #fff4f0; }

  /* Priority 4: rich success state */
  .stl-success-rich h2 { margin-top: 0; }
  .stl-success-number {
    font-size: 1.1rem;
    font-weight: 600;
    color: var(--color-accent);
    padding: 0.85rem 1rem;
    background: var(--color-bg, #fff);
    border-radius: 8px;
    border-left: 4px solid var(--color-accent);
    margin: var(--space-sm) 0;
    line-height: 1.5;
  }
  .stl-success-section { margin-top: var(--space-md); }
  .stl-success-section h3 {
    font-size: 0.78rem;
    text-transform: uppercase;
    letter-spacing: 0.1em;
    color: var(--color-text-muted);
    margin-bottom: 0.65rem;
  }
  .stl-success-list {
    margin: 0;
    padding-left: 1.4rem;
    display: flex;
    flex-direction: column;
    gap: 0.55rem;
  }
  .stl-success-list li { font-size: 0.92rem; line-height: 1.55; }
  .stl-success-fixes-list li { font-size: 0.9rem; }
  .stl-success-footer {
    margin-top: var(--space-md);
    padding-top: var(--space-sm);
    border-top: 1px solid var(--color-border, #e2e2dc);
    font-size: 0.88rem;
    color: var(--color-text-muted);
  }
</style>

<script>
(function() {
  'use strict';

  // Conversion-rate baseline anchored to HBR / InsideSales.com 2.24M lead study.
  // Reply within 5 min ≈ baseline 21x odds; longer waits decay roughly logarithmically.
  // We treat 5-min reply as a 20% close rate (industry-strong) and decay from there.
  // The output is intentionally conservative — it should feel believable, not salesy.
  function closeRateForHours(hours) {
    if (hours <= 0.1) return 0.20;
    if (hours <= 0.5) return 0.16;
    if (hours <= 2)   return 0.12;
    if (hours <= 6)   return 0.08;
    if (hours <= 24)  return 0.05;
    if (hours <= 48)  return 0.03;
    return 0.015;
  }

  var form        = document.getElementById('stl-score-form');
  if (!form) {
    if (window.console) console.error('[stl-score] form element not found — script aborted');
    return;
  }
  var leadsEl     = document.getElementById('stl-leads');
  var hoursEl     = document.getElementById('stl-hours');
  var jobEl       = document.getElementById('stl-job');
  var amountEl    = document.getElementById('stl-result-amount');
  var annualEl    = document.getElementById('stl-result-annual');
  var hintEl      = document.getElementById('stl-result-hint');
  var narrativeEl = document.getElementById('stl-result-narrative');
  var ctaCards    = document.getElementById('stl-cta-cards');
  var formFields  = document.getElementById('stl-form-fields');
  var reportBtn   = document.getElementById('stl-report-btn');
  var emailEl     = document.getElementById('stl-email');
  var success     = document.getElementById('stl-success');
  var errorEl     = document.getElementById('stl-error');
  var submit      = form.querySelector('.stl-submit');

  // Solveto multi-tenant newsletter endpoint. Uses the same pattern as the
  // existing 'ahmednadar' subscribe list. Backend needs an 'stl-toronto' list
  // configured; until then submissions still arrive but tagged for routing.
  var ENDPOINT = 'https://solveto.ca/api/v1/newsletter/stl-toronto/subscribe';

  // STL Agent dogfood loop: if visitor provides phone, also create a real
  // lead in the STL Rails app. This triggers AutoRespondJob → the visitor
  // gets a personalized AI email within seconds AND I (Ahmed) get a Telegram
  // ping with their data. The visitor literally experiences the product.
  //
  // SECURITY NOTE: this api_key is exposed in client-side JS by design.
  // The Rails app should rate-limit /api/leads (Rack::Attack) and the key
  // belongs to a dedicated marketing-only contractor account. If abuse
  // happens, rotate the key in admin and update the line below.
  var STL_API_ENDPOINT = 'https://stl.ahmednadar.com/api/leads';
  var STL_API_KEY = 'REPLACE_WITH_AHMED_CONTRACTOR_API_KEY';

  // TODO (Priority 7 — meta-loop): when first paid client onboards, swap
  // STL_API_KEY above for your real marketing contractor API key from the
  // STL admin panel. This sends every calculator signup into STL as a live
  // lead and triggers the AI auto-responder within 60 seconds — the prospect
  // experiences the product as part of signing up. All wiring is already here.

  // Dev mode: when running on localhost, skip real network calls and simulate
  // success so the UX can be tested without depending on remote services.
  // Set window.STL_FORCE_LIVE = true in the console to override and use real
  // endpoints from localhost (useful for full integration testing).
  var IS_LOCAL = /^(localhost|127\.0\.0\.1|0\.0\.0\.0|.*\.local)$/i.test(window.location.hostname);
  var DEV_MODE = IS_LOCAL && !window.STL_FORCE_LIVE;

  // Hard timeout so the form never hangs silently if a network call stalls.
  var FETCH_TIMEOUT_MS = 8000;

  function log(msg, data) {
    if (window.console && console.log) {
      console.log('[stl-score] ' + msg, data === undefined ? '' : data);
    }
  }

  function withTimeout(promise, ms, label) {
    return new Promise(function(resolve, reject) {
      var timer = setTimeout(function() {
        reject(new Error(label + ':timeout'));
      }, ms);
      promise.then(
        function(v) { clearTimeout(timer); resolve(v); },
        function(e) { clearTimeout(timer); reject(e); }
      );
    });
  }

  log('init', { IS_LOCAL: IS_LOCAL, DEV_MODE: DEV_MODE, hostname: window.location.hostname });

  function format(n) {
    return '$' + Math.round(n).toLocaleString('en-CA');
  }

  // Priority 2: contextual comparison for the annual amount line
  function getContextualComparison(annual) {
    if (annual < 12000)  return 'a part-time assistant for the year';
    if (annual < 36000)  return 'a reliable work van plus a year of insurance';
    if (annual < 72000)  return 'two full-time hires or a serious expansion budget';
    if (annual < 120000) return "a strong general manager's salary";
    return 'a full six-figure revenue line';
  }

  // Priority 2: narrative paragraph from slider values
  function getNarrative(leads, hours, job, lostMonthly, lostYearly) {
    if (lostMonthly === 0) return '';
    var f = format;
    if (hours <= 0.08) {
      return "You're already responding faster than 95% of GTA contractors. Your leak is probably not speed — it's volume. STL helps you handle 3x more inquiries without adding staff.";
    }
    if (hours <= 0.5 && leads >= 10) {
      return "You're fast, but " + leads + " leads/month means you're juggling follow-ups while you're on the job. STL keeps every inquiry warm while you're on-site, so none slip to a competitor.";
    }
    if (hours <= 2) {
      return "Most GTA contractors reply in 1–2 days. You're ahead of them, but the top 10% reply in minutes — and they get first pick. That gap costs roughly " + f(lostMonthly) + "/month at your volume.";
    }
    if (hours <= 6) {
      return "You reply the same day — better than 60% of GTA contractors. The gap between same-day and same-hour is where " + f(lostMonthly) + "/month disappears, job by job.";
    }
    if (hours <= 24 && leads >= 10) {
      return "Next-day replies put you behind the 78% of homeowners who hire the first contractor to respond. At " + leads + " leads/month, that gap is costing you roughly " + f(lostMonthly) + "/month.";
    }
    if (hours <= 48) {
      return "You're getting " + leads + " quote requests a month and losing roughly 7 out of every 10 to faster responders. At your average job size, that's " + f(lostMonthly) + "/month and " + f(lostYearly) + "/year you'll never bill.";
    }
    // 3+ days
    return "You're effectively invisible to homeowners in urgent mode. 78% of them hire before you've even seen their inquiry. The good news: this is the easiest leak to fix.";
  }

  // Priority 4: three specific leaks based on slider values
  function getLeaks(hours, job, leads) {
    var leaks = [];
    if (hours > 24) {
      leaks.push("After-hours submissions — most homeowner inquiries arrive between 7–11 PM. By morning, the job is already gone.");
      leaks.push("On-site interruptions — new leads hit your phone while you're working a job. Each one that waits 4+ hours has a 78% chance of hiring someone else first.");
    } else if (hours > 6) {
      leaks.push("On-site interruptions — leads arriving while you're working go unanswered for hours, long enough for a faster competitor to lock them in.");
      leaks.push("Manual follow-up overload — chasing leads by phone while managing jobs means some inevitably fall through the cracks.");
    } else {
      leaks.push("Volume ceiling — you reply fast but can't manually follow up with every inquiry. Leads that don't hear back a second time assume you moved on.");
    }
    if (job >= 25000) {
      leaks.push("High-ticket expectations — homeowners spending $25k+ vet 3–4 contractors. A slow or generic first touch reads as disorganized and loses the shortlist spot.");
    } else if (job >= 8000) {
      leaks.push("Competitor speed advantage — for mid-size projects, homeowners get 3 quotes and usually hire the first to respond. You're competing on speed, not price.");
    } else {
      leaks.push("Convenience wins small jobs — homeowners hire whoever is easiest to reach. Speed matters more than price at this job size.");
    }
    while (leaks.length < 3) {
      leaks.push("Weekend and holiday inquiries going unanswered while competitors with auto-responses capture the lead before Monday morning.");
    }
    return leaks.slice(0, 3);
  }

  // Priority 4: three tactical fixes the contractor can do without hiring me
  function getFixes() {
    return [
      "Set up a Google Business Profile auto-reply: \"Thanks for reaching out — I'll call you back within 2 hours between 7am–8pm. Text me at [your number] for urgent jobs.\" Free, takes 10 minutes.",
      "Add a booking link to your voicemail: \"Can't take your call — book a 5-min callback at [Cal.com link].\" Serious homeowners use it. Cal.com free tier takes 20 minutes to set up.",
      "Install an email auto-responder with one qualifying question: \"What's the address and roughly when do you want this done?\" Keeps you top-of-mind and screens out tire-kickers before you call back."
    ];
  }

  function recompute() {
    var leads = parseFloat(leadsEl.value) || 0;
    var hours = parseFloat(hoursEl.value) || 0;
    var job   = parseFloat(jobEl.value)   || 0;
    var currentRate = closeRateForHours(hours);
    var idealRate   = closeRateForHours(0.08);
    var lostJobs    = leads * (idealRate - currentRate);
    var lostMonthly = Math.max(0, lostJobs * job);
    var lostYearly  = lostMonthly * 12;

    amountEl.textContent = format(lostMonthly);

    if (lostMonthly === 0) {
      hintEl.textContent = 'You are already responding fast. Most contractors lose $5,000–$25,000/month.';
      if (annualEl)    { annualEl.style.display    = 'none'; annualEl.textContent    = ''; }
      if (narrativeEl) { narrativeEl.style.display = 'none'; narrativeEl.textContent = ''; }
      if (ctaCards)    ctaCards.style.display = 'none';
    } else {
      hintEl.textContent = 'Roughly ' + lostJobs.toFixed(1) + ' lost jobs/mo · about ' + format(lostYearly) + '/year on the table.';

      // Priority 2: annual line
      if (annualEl) {
        annualEl.textContent = 'That\'s roughly ' + format(lostYearly) + ' per year — enough to cover ' + getContextualComparison(lostYearly) + '.';
        annualEl.style.display = 'block';
      }

      // Priority 2: narrative paragraph
      var narrative = getNarrative(leads, hours, job, lostMonthly, lostYearly);
      if (narrativeEl) {
        narrativeEl.textContent = narrative;
        narrativeEl.style.display = narrative ? 'block' : 'none';
      }

      // Priority 1: reveal CTA cards
      if (ctaCards) ctaCards.style.display = 'flex';
    }
  }

  [leadsEl, hoursEl, jobEl].forEach(function(el) {
    el.addEventListener('input', recompute);
    el.addEventListener('change', recompute);
  });
  recompute();

  // Priority 1: report card — progressive disclosure of the email form
  if (reportBtn && formFields && emailEl) {
    reportBtn.addEventListener('click', function() {
      formFields.style.display = 'block';
      emailEl.setAttribute('required', 'required');
      formFields.scrollIntoView({ behavior: 'smooth', block: 'start' });
      setTimeout(function() { emailEl.focus(); }, 400);
    });
  }

  // Priority 4: populate and show the rich success state
  function showSuccess(includesDogfood) {
    var leads = parseFloat(leadsEl.value) || 0;
    var hours = parseFloat(hoursEl.value) || 0;
    var job   = parseFloat(jobEl.value)   || 0;
    var currentRate = closeRateForHours(hours);
    var idealRate   = closeRateForHours(0.08);
    var lostMonthly = Math.max(0, leads * (idealRate - currentRate) * job);
    var lostYearly  = lostMonthly * 12;

    var numEl = document.getElementById('stl-success-number');
    if (numEl) {
      numEl.textContent = 'Based on your inputs, you\'re losing an estimated ' +
        format(lostMonthly) + '/month (' + format(lostYearly) + '/year) to slower competitors.';
    }

    var leaksList = document.getElementById('stl-success-leaks');
    if (leaksList) {
      leaksList.innerHTML = '';
      getLeaks(hours, job, leads).forEach(function(leak) {
        var li = document.createElement('li');
        li.textContent = leak;
        leaksList.appendChild(li);
      });
    }

    var fixesList = document.getElementById('stl-success-fixes');
    if (fixesList) {
      fixesList.innerHTML = '';
      getFixes().forEach(function(fix) {
        var li = document.createElement('li');
        li.textContent = fix;
        fixesList.appendChild(li);
      });
    }

    form.style.display = 'none';
    success.style.display = 'block';
    if (includesDogfood) {
      var dog = document.getElementById('stl-success-dogfood');
      if (dog) dog.style.display = 'block';
    }
    success.scrollIntoView({ behavior: 'smooth', block: 'center' });
  }

  function showError() {
    errorEl.style.display = 'block';
    submit.disabled = false;
    submit.textContent = 'Send me the free report';
  }

  function buildLeadMessage(leads, hours, job, lostMonthly) {
    return [
      'Submitted via /stl-score calculator on ahmednadar.com.',
      '',
      'Lead volume: ' + leads + '/month',
      'Current reply time: ' + hours + ' hours',
      'Avg job size: $' + Math.round(job).toLocaleString('en-CA'),
      'Estimated monthly loss: $' + Math.round(lostMonthly).toLocaleString('en-CA'),
      '',
      'Page: ' + window.location.href
    ].join('\n');
  }

  function postNewsletter(payload) {
    log('postNewsletter →', payload);
    if (DEV_MODE) {
      return new Promise(function(r) { setTimeout(function() { r('newsletter'); }, 600); });
    }
    return withTimeout(
      fetch(ENDPOINT, {
        method: 'POST',
        headers: { 'Content-Type': 'application/json' },
        body: JSON.stringify(payload)
      }).then(function(res) {
        if (!res.ok) throw new Error('newsletter:' + res.status);
        return 'newsletter';
      }),
      FETCH_TIMEOUT_MS,
      'newsletter'
    );
  }

  function postStlLead(payload) {
    log('postStlLead →', payload);
    if (DEV_MODE) {
      return new Promise(function(r) { setTimeout(function() { r('stl'); }, 600); });
    }
    if (!STL_API_KEY || STL_API_KEY === 'REPLACE_WITH_AHMED_CONTRACTOR_API_KEY') {
      log('postStlLead skipped — API key not configured');
      return Promise.reject(new Error('stl:not-configured'));
    }
    return withTimeout(
      fetch(STL_API_ENDPOINT, {
        method: 'POST',
        headers: { 'Content-Type': 'application/json' },
        body: JSON.stringify(payload)
      }).then(function(res) {
        if (!res.ok) throw new Error('stl:' + res.status);
        return 'stl';
      }),
      FETCH_TIMEOUT_MS,
      'stl'
    );
  }

  form.addEventListener('submit', function(e) {
    e.preventDefault();
    errorEl.style.display = 'none';
    submit.disabled = true;
    submit.textContent = 'Sending...';

    var leads = parseFloat(leadsEl.value) || 0;
    var hours = parseFloat(hoursEl.value) || 0;
    var job   = parseFloat(jobEl.value)   || 0;
    var currentRate = closeRateForHours(hours);
    var idealRate   = closeRateForHours(0.08);
    var lostMonthly = Math.max(0, leads * (idealRate - currentRate) * job);

    var emailValue    = form.email    ? form.email.value.trim()    : '';
    var nameValue     = form['name']  ? form['name'].value.trim()  : '';
    var businessValue = form.business ? form.business.value.trim() : '';
    var phoneValue    = form.phone    ? form.phone.value.trim()    : '';

    if (!emailValue) {
      submit.disabled = false;
      submit.textContent = 'Send me the free report';
      if (emailEl) emailEl.focus();
      return;
    }

    // Track 1: newsletter capture (always fires)
    var newsletterPayload = {
      email: emailValue,
      name: nameValue,
      business: businessValue,
      website: form.website.value,
      source: 'stl-score-calculator',
      tags: ['stl', 'toronto', 'contractor-audit-lead'],
      metadata: {
        leads_per_month: leads,
        reply_hours: hours,
        job_size: job,
        lost_monthly_estimate: Math.round(lostMonthly),
        page: window.location.pathname,
        has_phone: phoneValue.length > 0
      }
    };

    var promises = [postNewsletter(newsletterPayload)];

    // Track 2: STL dogfood lead (fires only if phone provided)
    if (phoneValue) {
      var leadName = nameValue || (emailValue.split('@')[0] || 'Calculator visitor');
      var serviceLabel = businessValue
        ? 'Speed-to-Lead Audit — ' + businessValue
        : 'Speed-to-Lead Audit';

      var stlPayload = {
        api_key: STL_API_KEY,
        name: leadName,
        phone: phoneValue,
        email: emailValue,
        service_needed: serviceLabel.substring(0, 255),
        message: buildLeadMessage(leads, hours, job, lostMonthly),
        urgency: 'normal',
        source: 'form'
      };

      promises.push(postStlLead(stlPayload));
    }

    log('submitting form', { phone: !!phoneValue, dev: DEV_MODE, promises: promises.length });

    // Show success if at least one channel succeeded; only error if BOTH failed
    Promise.allSettled(promises).then(function(results) {
      log('submit results', results);
      var anyOk = results.some(function(r) { return r.status === 'fulfilled'; });
      var stlOk = results.some(function(r) { return r.status === 'fulfilled' && r.value === 'stl'; });
      if (anyOk) {
        showSuccess(stlOk);
      } else {
        showError();
      }
    });
  });
})();
</script>
