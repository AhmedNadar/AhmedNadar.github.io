---
layout: post
title: "Not Acceptable"
date: 2026-07-28
description: "A resident told me my civic platform rejected his browser. The cause was a Rails default I had never read, and five of the six reasons it gave turned out not to apply to my app."
tags: [rails, ruby, engineering, accessibility, civic-tech, debugging]
---

The chair of a residents' association emailed me yesterday to say he had tried to visit the platform I build and could not get in. He was gracious about it and assumed the fault was his. "My browsers were Not Acceptable," he wrote. "Probably too old, I don't keep up with OS and browser updates."

He had quoted the error page back to me without realising it. Not Acceptable is HTTP 406. That is not a browser failing to render something. That is my server looking at his request and refusing to answer it.

I went and reproduced it with four old user agents. Every one came back 406. Then I opened the file I had never really read.

```ruby
class ApplicationController < ActionController::Base
  # Only allow modern browsers supporting webp images, web push, badges,
  # import maps, CSS nesting, and CSS :has.
  allow_browser versions: :modern
end
```

That line ships in the Rails 8 application template. It had been sitting in my controller since the day I generated the app, and I had never once asked what `:modern` meant. It means Safari 17.2, Chrome 120, Firefox 121, Opera 106. Roughly, anything released before December 2023. Everyone else gets `public/406-unsupported-browser.html` and no argument.

For a lot of products that is a defensible trade. For a civic platform it is a bad one. The people most likely to be running an old browser are the people on an ageing phone, or a library machine, or a work computer where IT decides what gets updated. Those are not edge cases in this line of work. They are a meaningful part of the point.

## The comment lists six reasons. I checked all six.

The instinct is to delete the line and move on. I nearly did, and I was wrong to want to, because the comment is making a technical claim: this app needs webp, web push, badges, import maps, CSS nesting and CSS `:has`. If any of that is true, removing the gate does not help anybody. It just replaces a clear error page with a broken one, which is worse, because now the user thinks the site is garbage rather than that their browser is old.

So I checked what the app actually serves.

CSS nesting and `:has` were the easy ones. Pull down every stylesheet the page loads and count.

```bash
for f in application blog dashboard map public reports tailwind; do
  curl -s "https://example.ca/assets/$f.css" -o c.css
  echo "$f  :has=$(grep -o ':has(' c.css | wc -l)  nest=$(grep -oE '\{[^{}]*&' c.css | wc -l)"
done
```

CSS nesting: zero occurrences, across all seven stylesheets, about 630KB of compiled CSS. The build tool had already flattened every nested rule into plain selectors, which is what build tools are for. The gate was protecting a feature the browser never sees.

CSS `:has()`: ten rules in the whole 630KB. Ten. And unsupported selectors are not a failure mode in CSS, they are the specification. A browser that does not understand `:has()` skips those rules and renders everything else. Ten skipped rules out of thousands is a cosmetic difference nobody would report as a bug.

WebP has been supported since Safari 14 and Chrome 32, both far older than any floor I would set. Web push and badges are opt-in features; a browser without them gets no push notifications and nothing else changes.

That is five of the six reasons, and none of them survived contact with the actual output.

## The sixth one was real, and worse than the comment suggested

Import maps are different. I have seventeen pins and `javascript_importmap_tags` in every layout. A browser that does not support import maps natively does not degrade. It runs no JavaScript at all. No Turbo, no Stimulus, no forms, no maps. A blank-eyed page that looks fine and does nothing.

There used to be a safety net. importmap-rails 1.x shipped the es-module-shims polyfill so older browsers got a working page anyway. Version 2 dropped it. I confirmed which side of that line I was on by looking at what production actually sends:

```bash
curl -s https://example.ca/ | grep -oE '<script[^>]*(es-module-shims|importmap)[^>]*>'
# <script type="importmap" data-turbo-track="reload" nonce="...">
```

One tag. No shim. So import maps are a hard requirement for this app, and the comment was right about exactly one of its six claims.

Native import map support arrives at Chrome 89 in March 2021, Firefox 108 in December 2022, and Safari 16.4 in March 2023. Compare that to what `:modern` was demanding and the gap is stark. The preset was turning away up to two and a half years of browsers that would have run the application perfectly.

## The fix, and the trap inside it

The answer was not to remove the gate. It was to set it to the truth.

```ruby
allow_browser versions: {
  safari: 16.4, chrome: 89, firefox: 108, edge: 89, opera: 76, ie: false
}
```

That `ie: false` is the part I would have missed if I had trusted the change instead of testing it. `allow_browser` only blocks browsers you name. Anything not in the hash is allowed through, on the reasonable theory that you should not block a browser you have never heard of. Internet Explorer is a browser I have heard of, and it has no import map support at any version, so without that line it sails past the gate and gets the JavaScript-less page I had just spent an afternoon trying to prevent.

I found that because I wrote a throwaway integration test rather than reasoning about it:

```ruby
test "report the gate outcome for each browser" do
  UAS.each do |label, ua|
    get root_path, headers: { "HTTP_USER_AGENT" => ua }
    puts format("%-24s %s", label, response.status == 406 ? "BLOCKED" : "allowed")
  end
end
```

```
Safari 9 (2015)          BLOCKED
Safari 16.4 (Mar 2023)   allowed
Firefox 52 (2017)        BLOCKED
Firefox 115 (2023)       allowed
Chrome 89 (Mar 2021)     allowed
IE 8                     allowed     ← before ie: false
```

Six lines of output, one of them wrong, caught in about a minute. Then I deleted the test, because its job was to answer a question and the question was answered.

## What I actually take from this

The framework default was not wrong. It was written for a general case and it is honest about what it assumes, right there in the comment. What I did wrong was inherit an assumption and never audit it, on a product whose entire premise is that residents should not be shut out of civic systems by technology decisions made without them.

The comment named six requirements. One was real. I would not have known which one without going and counting, and the counting took less than an hour.

Somebody has to be the person who reads the default. On a civic platform, it should probably be me, and it should probably not take a stranger writing me a polite email to make it happen.
