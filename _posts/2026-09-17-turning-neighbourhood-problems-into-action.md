---
layout: post
title: "Turning Neighbourhood Problems into Action"
date: 2026-09-17
description: "The talk I gave to Civic Tech Brampton at Algoma University: one pothole, seven cities, and the moment the city started writing back. The slides are yours, free."
tags: [solvecanada, civic-tech, talks, open-data, brampton, canada]
---

Tonight I gave a talk to Civic Tech Brampton, hosted at Algoma University on Queen Street, from my desk in Toronto. This is the short version, and the slides are yours at the bottom if you want to take the whole thing with you.

## It still starts with a pothole

For about eight months I drove past the same pothole near my house. Every day. I tried to tell the city the proper way: a phone line I gave up on, then a twelve-minute form, five pages, dropdown lists I could not make sense of. I got a reference number and a status page that never changed. That is where most people quit, and I nearly did. The cost of caring got higher than the value of caring.

There is a story Rory Sutherland tells about Volkswagen refusing to put cupholders in their cars for years. The car was a temple, not a coffee table. A hundred rational decisions and one human blind spot. Every civic form I have ever filled out was built by the cupholder engineers.

## What it became

One photo. The report drafts itself and you send it in about thirty seconds. Or five spoken words at a red light. It goes to the city and to your councillor. The only rule I gave myself was to make caring cheap again.

The app is the small part. The data was always there. Across seven cities the governments have been publishing it for decades, far more than anyone uses. So far I have pulled over three million pieces of public infrastructure onto one map, catch basins back to the 1840s, and that is a fraction of what those cities publish, sitting in the open, free. Nobody had connected it. So I started to. The technology was never the bottleneck. We were.

## The city writes back

The part I am proudest of this year is not something you can see on a screen. When you send a report, we deliver it to the city and your councillor, and then we do the thing you used to do yourself: we catch the city's own reference number, so you never chase it. When the city marks it fixed, the answer comes back to you, on your report, on the record.

A litter bin overflowing at 989 Dovercourt Road was reported on September 7. Toronto 311 confirmed it fixed on September 11. Four days, checked against the city's records, not taken on faith. Hundreds of reports have closed this way now, and nobody phoned anyone. That is what the thirty seconds buys you. Not a complaint sent into the void. A record, and an answer.

## It does not stop at city streets

The room tonight was a university. A campus is a small town that someone runs. Thousands of people walk past the broken things every day, a dead light in a stairwell, a cracked step, a spill in the hall, and the people whose job it is to fix them cannot see what the finders see. The same tool works on private property, a campus, a hospital, a mall, a main street. If you can draw it on a map, it can be live in a week.

## The seventh city was a Tuesday afternoon

I used to say the fifth city would be a Tuesday afternoon. In July it was: Waterloo and Kitchener in one afternoon, then Peterborough and Ottawa. Seven cities now, on one map, under Solve Canada. Singapore runs one platform for an entire country. Canada ranks forty-seventh for digital government.

And Brampton is not on that map yet. The city publishes about 274 open datasets, one of the richest portals in the GTA. Putting Brampton on the map is an afternoon. Closing the loop with Brampton 311 takes one person inside the city saying yes. That is where a room like tonight's matters.

Obsession or stubbornness? Both. You do not need permission, and you do not need to be the smartest person in the room. You just have to refuse to accept that broken is normal.

<div class="deck-cta">
  <p class="deck-cta-label">The full talk, the way I gave it to Civic Tech Brampton.</p>
  <button type="button" class="cta-button" data-open-deck>Take the talk with you</button>
</div>

<!-- ===== Deck download modal ===== -->
<div class="deck-modal" id="deck-modal" aria-hidden="true">
  <div class="deck-modal-backdrop" data-close-deck></div>
  <div class="deck-modal-card" role="dialog" aria-modal="true" aria-labelledby="deck-modal-title">
    <button type="button" class="deck-modal-close" data-close-deck aria-label="Close">&times;</button>
    <h3 id="deck-modal-title">Take the talk with you</h3>
    <p class="deck-modal-sub">Drop your email and the slides are yours. I will also send the occasional note about what I am building. Unsubscribe anytime.</p>

    <form id="deck-form" novalidate>
      <div style="position:absolute;left:-9999px" aria-hidden="true">
        <input type="text" name="website" tabindex="-1" autocomplete="off">
      </div>
      <div class="deck-field">
        <label for="deck-name">Name <span class="deck-optional">(optional)</span></label>
        <input type="text" id="deck-name" name="name" autocomplete="name" placeholder="Your name">
      </div>
      <div class="deck-field">
        <label for="deck-email">Email</label>
        <input type="email" id="deck-email" name="email" required autocomplete="email" placeholder="you@email.com">
      </div>
      <button type="submit" class="cta-button deck-submit">Send me the slides</button>
    </form>

    <div id="deck-success" class="contact-feedback" style="display:none">
      <p><strong>It's yours.</strong> The download should start automatically. If it does not, <a href="/assets/downloads/turning-neighbourhood-problems-into-action-brampton.pdf" download>grab it here</a>.</p>
    </div>
    <div id="deck-error" class="contact-feedback contact-feedback-error" style="display:none">
      <p>Something went wrong. Try again, or just <a href="/assets/downloads/turning-neighbourhood-problems-into-action-brampton.pdf" download>download the slides directly</a>.</p>
    </div>
  </div>
</div>

<style>
  .deck-cta { margin: var(--space-lg) 0; padding: var(--space-md); border: 1px solid var(--color-border); border-radius: 10px; background: var(--color-bg-warm); text-align: center; }
  .deck-cta-label { margin: 0 0 var(--space-sm); font-family: var(--font-serif); font-size: var(--text-lg); color: var(--color-text); }
  .deck-modal { position: fixed; inset: 0; z-index: 1000; display: none; align-items: center; justify-content: center; padding: var(--space-md); }
  .deck-modal.is-open { display: flex; }
  .deck-modal-backdrop { position: absolute; inset: 0; background: rgba(20,15,10,0.6); backdrop-filter: blur(3px); }
  .deck-modal-card { position: relative; z-index: 1; width: 100%; max-width: 30rem; background: var(--color-bg); border: 1px solid var(--color-border); border-radius: 14px; padding: var(--space-lg); box-shadow: 0 30px 80px rgba(0,0,0,0.25); }
  .deck-modal-close { position: absolute; top: 0.6rem; right: 0.9rem; background: none; border: none; font-size: 1.8rem; line-height: 1; color: var(--color-text-light); cursor: pointer; }
  .deck-modal-close:hover { color: var(--color-accent); }
  #deck-modal-title { font-family: var(--font-serif); font-size: var(--text-2xl); margin: 0 0 var(--space-xs); }
  .deck-modal-sub { color: var(--color-text-muted); font-size: var(--text-sm); margin: 0 0 var(--space-md); }
  .deck-field { margin-bottom: var(--space-sm); }
  .deck-field label { display: block; font-size: var(--text-sm); font-weight: 600; margin-bottom: 0.3rem; }
  .deck-optional { font-weight: 400; color: var(--color-text-light); }
  .deck-field input { width: 100%; font-family: var(--font-sans); font-size: var(--text-base); color: var(--color-text); background: var(--color-bg); border: 1.5px solid var(--color-border); border-radius: 6px; padding: 0.7rem 0.9rem; transition: border-color var(--transition); }
  .deck-field input:focus { outline: none; border-color: var(--color-accent); }
  .deck-submit { width: 100%; margin-top: var(--space-xs); border: none; cursor: pointer; }
</style>

<script>
(function () {
  'use strict';
  var modal = document.getElementById('deck-modal');
  if (!modal) return;
  var form = document.getElementById('deck-form');
  var success = document.getElementById('deck-success');
  var error = document.getElementById('deck-error');
  var submit = form.querySelector('.deck-submit');
  var ENDPOINT = 'https://solveto.ca/api/v1/newsletter/ahmednadar/subscribe';
  var PDF = '/assets/downloads/turning-neighbourhood-problems-into-action-brampton.pdf';

  function openModal() { modal.classList.add('is-open'); modal.setAttribute('aria-hidden', 'false'); setTimeout(function () { document.getElementById('deck-email').focus(); }, 50); }
  function closeModal() { modal.classList.remove('is-open'); modal.setAttribute('aria-hidden', 'true'); }

  document.querySelectorAll('[data-open-deck]').forEach(function (b) { b.addEventListener('click', openModal); });
  document.querySelectorAll('[data-close-deck]').forEach(function (b) { b.addEventListener('click', closeModal); });
  document.addEventListener('keydown', function (e) { if (e.key === 'Escape') closeModal(); });

  function triggerDownload() {
    var a = document.createElement('a');
    a.href = PDF; a.setAttribute('download', '');
    document.body.appendChild(a); a.click(); document.body.removeChild(a);
  }

  form.addEventListener('submit', function (e) {
    e.preventDefault();
    error.style.display = 'none';
    if (form.website.value) { return; } // honeypot
    submit.disabled = true;
    submit.textContent = 'Sending...';

    fetch(ENDPOINT, {
      method: 'POST',
      headers: { 'Content-Type': 'application/json' },
      body: JSON.stringify({ email: form.email.value.trim(), name: form.name.value.trim(), website: form.website.value })
    })
    .then(function (res) { if (!res.ok) throw new Error(res.status); })
    .then(function () {
      form.style.display = 'none';
      success.style.display = 'block';
      triggerDownload();
    })
    .catch(function () {
      error.style.display = 'block';
      submit.disabled = false;
      submit.textContent = 'Send me the slides';
    });
  });
})();
</script>
