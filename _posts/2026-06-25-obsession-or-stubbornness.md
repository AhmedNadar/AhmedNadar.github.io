---
layout: post
title: "Obsession or Stubbornness?"
date: 2026-06-25
description: "The talk I gave at Curio's Smarter Cities Through Open Data: how one pothole turned into a kind of intelligence for a whole city. The slides are yours, free."
tags: [solveto, civic-tech, talks, open-data]
published: false
---

Tonight I gave a talk at Curio's *Smarter Cities Through Open Data*, in the spot Arthur Smith handed me. This is the short version of it, and the slides are yours at the bottom if you want to take the whole thing with you.

## It started with a pothole

For about eight months I drove past the same pothole near my house. Every single day. And I became, honestly, a little obsessed with it. By the end I could not tell if it was obsession or plain stubbornness.

You learn which lane to avoid. After a while you stop seeing it as a problem at all and start treating it like the weather, just something you live with. The whole time I kept thinking, the city does not know this is here. Not because they do not care. Because nobody ever told them in a way they could act on.

So I tried to tell them the proper way. I called the line and sat on hold. I filled out the form, twelve minutes, five pages, strange dropdown lists I could not make sense of. I pushed through and submitted it, and yes, I got a reference number. But then the only way to know if anything was happening was to go check that number myself, over and over, and nothing ever came back to me. That is the moment most people quit, me included. The problem was never that people do not care about their city. The cost of caring had simply gotten higher than the value of caring.

## The cupholder problem

There is a story Rory tells. For years, Volkswagen's engineers refused to put cupholders in their cars. It was beneath them. To them the car was a temple, a precision machine, not a coffee table. Meanwhile cupholders were one of the top things American buyers actually cared about, ranked above how the car took a corner. The engineers got a hundred rational things right and lost on the one human thing.

That is exactly what happened to civic reporting. Smart people built these forms the way they believed a form should be built, and nobody ever asked the one question that matters. What does the person filling this out actually need, to do it once, and want to come back and do it again?

This is not really about me. My only specialty is refusing to walk past a problem. This time it happened to be a pothole. Next time it will be something else entirely.

## What it became

One photo. The AI drafts the whole report and you send it in about thirty seconds. Or if you are stopped at a light and cannot take a photo, five spoken words is enough. All I was trying to do was make caring cheap again.

But the app is the small part. Here is the bigger thing, and it is the part this room came for. [The data was always there](/the-data-was-always-there/). The city has been collecting it for decades, over a million pieces of public infrastructure, sitting in the open, free, for years. Nobody had simply connected it. The technology was never the bottleneck. We were.

So this is not a complaint box. It is a city intelligence layer. The reports are the input, the city's own open data is the context, and [Pulse](/fly-over-your-city/) is how you see all of it at once: a living map of the city you can fly over, where every report glows and the whole thing replays through time so you watch the city quietly fix itself.

And when something gets fixed, the map celebrates it. A flag goes up. The people who did the work, the city's own crews and the private contractors alongside them, almost never get named for it. So we put their name on it. Solved, and solved by you.

## The distance is a choice

Singapore runs one platform for an entire country. Canada ranks forty-seventh in the world for digital government. The second city was hard. The fifth city is a Tuesday afternoon. So the gap between where we are and one platform for every Canadian city is not a money problem, and it is not a talent problem. It is an architecture problem, which means it is a choice. I have already proven the hard part works, and I am not waiting for permission to build the rest.

So, obsession or stubbornness? Honestly, it was both. And that is the point. You do not need permission, and you do not need to be the smartest person in the room. You just have to refuse to accept that broken is normal.

<div class="deck-cta">
  <p class="deck-cta-label">The full talk, the way I gave it tonight.</p>
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
      <p><strong>It's yours.</strong> The download should start automatically. If it does not, <a href="/assets/downloads/obsession-or-stubbornness-talk.pdf" download>grab it here</a>.</p>
    </div>
    <div id="deck-error" class="contact-feedback contact-feedback-error" style="display:none">
      <p>Something went wrong. Try again, or just <a href="/assets/downloads/obsession-or-stubbornness-talk.pdf" download>download the slides directly</a>.</p>
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
  var PDF = '/assets/downloads/obsession-or-stubbornness-talk.pdf';

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
