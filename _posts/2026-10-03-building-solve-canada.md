---
layout: post
title: "Building Solve Canada"
date: 2026-10-03
description: "What I told XO Ruby Toronto: seven cities on one Rails app, the one-line scope that stopped 1.4 TB of database transfer, and why I fired two AI agents. The slides are yours, free."
tags: [solvecanada, rails, ruby, civic-tech, talks, toronto]
---

Today I gave a talk at XO Ruby in Toronto, a room full of Rails developers at The Loft on King. This is the short version, and the slides are yours at the bottom.

I opened with two questions. What is a pothole fixed a week faster worth to you? And what is it worth to just know that someone saw it? Hold both numbers. I will come back for them.

## Nobody cared about the catch basins

For weeks I posted about features. AI that reads your photo and works out what is broken. A map of every catch basin in the city. Silence. Then I wrote one post about one pothole on my street, and within the week CBC and Global called. The code didn't change between those posts. I did. I stopped selling the machine and started talking about the relief: you report it, and someone tells you what happened.

So the app does one thing for a resident. Take a picture, like a selfie. It blurs faces and licence plates, writes the report, finds the ward and the councillor, and sends it to the city. One report on September 7 reached Toronto 311 in under twelve seconds. When the city answers, the app catches the reference number and brings the answer back to the person who reported it. This week we went one step further: the app only says "Fixed" when there is proof, a repair on the city's own record or a neighbour who went back to check. Closed is not the same as fixed.

## Three weeks, then over dinner

Toronto took me about three weeks. Mississauga took three days, and that was the refactor that made every later city cheap. Milton, a day. Kitchener and Waterloo, an afternoon. Ottawa and Peterborough, half an hour each, built by agents while I was at dinner. These are my own estimates, not timesheets (nobody was timing me at dinner), and they measure build time, not launch dates. Somewhere around the second city, technology stopped being the bottleneck. After that, the bottleneck was me.

The idea behind it is almost boring. A city is a row, not an if. Each city is a `City` record with its own settings, and views never ask which city they are in. They ask a small presenter:

```ruby
# in a view
current_city_copy.brand

# app/presenters/city_copy.rb
def brand
  fetch("brand")
end
```

The rule is no slug branches in views, mailers or controllers. There are still a few in the models, mostly "Toronto is the default", and the next step is to stop having a default city at all.

## Seven cities, one route table

All seven cities live on one domain: `/toronto/reports`, `/ottawa/reports`. I didn't touch `config/routes.rb`. A small Rack middleware takes the city off the front of the path and moves it into `SCRIPT_NAME`:

```ruby
def mount(env, slug, rest)
  env["SCRIPT_NAME"] = "#{env['SCRIPT_NAME']}/#{slug}"
  env["PATH_INFO"]   = rest
  env[ENV_KEY]       = slug
  env
end
```

`SCRIPT_NAME` is Rack's mount prefix, and Rails prepends it to every URL it generates. So every `link_to`, every `redirect_to` and every form stays inside the right city with zero route changes. A routing scope with `default_url_options` would have been a fair alternative on a new app. With 606 existing routes, the mount prefix won. The gotcha that cost me real time: mutate `env` in place. A copy made with `env.merge` cut the link that Rails uses to commit the CSRF token, and every form under a city prefix failed with a 422. That incident is now a test that submits a real form with forgery protection on.

## Seven names in a dropdown, 1.4 TB a month

Every city row carries its boundary, the shape of the city as a polygon. Toronto's is 336 KB. With one city, that was a fine design. Then the header dropdown loaded all seven full rows on every page to show seven names, and the lookup that works out which city you are in loaded the full Toronto row on every request. About a megabyte of polygon per page view, for every visitor, bots included. Database transfer reached about 1.4 TB that month, and the bill grew about eight times between March and August.

The fix starts with one line:

```ruby
scope :light, -> { select(*(column_names - ["boundary"])) }
```

Then the call sites that only need names use it, the dropdown reads from a small frozen list in memory, and anything that needs the polygon asks the database to do the geometry and return a number. Storing the boundary on the row was a good decision for the app I had. It was a terrible decision for the app I was building.

Fixing it once wasn't enough, so every integration test now listens to the SQL it runs and fails if a request pulls the boundary:

```ruby
def process(method, path, **args)
  result = nil
  leaks = BoundaryGuard.capture { result = super }
  raise Minitest::Assertion, "#{path} fetched cities.boundary" if leaks.any?
  result
end
```

The first run failed 28 test files. The worst one: every email a city sent us loaded all seven cities, polygons included. It is a guard for the query shapes that bit us, not a parser, and it has known gaps. It is still the reason the next person, including future me, cannot ship this mistake quietly.

## I fired two AI agents

Two AI agents watched production, one every fifteen minutes and one every hour. Together they were most of my AI bill. Then I read a week of their work. One saw failed jobs in 98 runs in a row and raised one alert. So I read their prompts. "There should be four processes." "If a report is stuck for five minutes, alert." That isn't intelligence. A rule list is an if statement. Now it's plain Ruby, with no model call, and it alerts on the first failure.

Use AI where the problem is fuzzy, like reading a photo. Don't use it where the answer is an if. And yes, you should still learn to code. AI can be smarter than me. It cannot decide what I put my name on.

## Back to your two numbers

A week faster, or knowing someone saw it. The city spends its money on the first, and it should. I only ever built the second, and that is the one people open every day. Building it, I stopped being only a developer. I'm the marketing, the content and the support too. I don't need to know everything. I need to know enough to move fast, and code is one of the jobs.

The code got easy. Caring stayed hard.
<div class="deck-cta">
  <p class="deck-cta-label">The full talk, the way I gave it at XO Ruby Toronto.</p>
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
      <p><strong>It's yours.</strong> The download should start automatically. If it does not, <a href="/assets/downloads/building-solve-canada-xoruby.pdf" download>grab it here</a>.</p>
    </div>
    <div id="deck-error" class="contact-feedback contact-feedback-error" style="display:none">
      <p>Something went wrong. Try again, or just <a href="/assets/downloads/building-solve-canada-xoruby.pdf" download>download the slides directly</a>.</p>
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
  var PDF = '/assets/downloads/building-solve-canada-xoruby.pdf';

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
