---
layout: post
title: "How I went from one button to building entire Rails apps in 10 seconds"
date: 2026-02-10
description: ""
tags: [rapidfy, ai, rapidrails]
---

Two years ago I had a simple idea. Build UI components made only for Rails. Not ported from React. Not wrapped around some JavaScript library. Just pure Rails native components.

It started with a button. Literally one button component.

Today it's 32 components. Forms, steppers, tables, modals, navigation, dropdowns. All built with Rails 8, Turbo, and StimulusJS. No external JavaScript libraries. No outside frontend. They work with your Rails forms out of the box because they were built for Rails forms. https://rapidrails.cc/docs/

I'm using them in production right now, which is really good.
But something kept bugging me. A new idea!

Every week I see tools like v0 and Lovable letting developers describe what they want and get a working app back in seconds. Type a prompt, get a React app. It's impressive. But if you're a Rails developer, you get nothing. None of these tools generate Rails native code. None of them know about Turbo or Stimulus or Rails form or ViewComponents. You're on your own.

That felt wrong. Rails developers deserve the same thing.

So I asked myself a question. I already have 32 components that follow Rails conventions. What if an AI agent already knows how those components work, how they connect, and how Rails 8 expects things to be structured? What would happen if I pointed an agent at all of that and said "build me a blog"?

I tried it. And it worked.

Let me tell you what happens right now when you give Rapidfy a clear prompt.

You say "build me a blog with authors, posts, and comments." Rapidfy creates the models. Generates the migrations. Runs them. Builds the views using RapidRails UI components. Makes two commits. The whole thing takes about 10 seconds and ready for testing.
Ten seconds. That's faster than running "rails new".

You get a working Rails 8 application with real UI, real database tables, and real components. Not a prototype. Not a wireframe. A working app.

And everything it builds is Rails native. Turbo for page updates. Stimulus for interactivity. ViewComponents for the UI. No React anywhere. No npm install anything. Just Rails the way Rails is meant to be.

Why does this work?

It works because Rails is predictable. Convention over configuration means there are clear patterns for where things go and how they connect. Models go here. Views go there. Routes follow this pattern. Forms work this way. 
But Rails alone isn't enough. The agent also needs to understand the UI layer. That's where RapidRails UI makes the difference.
Every component follows a clear, consistent structure. A button works like a button. A stepper works like a stepper. The naming is predictable. The options are documented. The patterns repeat. An agent doesn't have to guess how to use a modal or figure out five different ways to build a form.

And the documentation wasn't built just for humans. It was built with AI agents in mind from the start. Every component has clear inputs, expected outputs, and usage examples that an agent can read and act on immediately. No ambiguity. No "it depends." Just structure an agent can follow.

That predictability is exactly what AI agents need. When the framework is consistent and the components follow conventions, the agent makes fewer mistakes. React has a thousand ways to do everything. Rails has one good way. RapidRails UI makes sure the frontend follows that same philosophy.
That's the advantage. The whole stack speaks the same language. 

I'm not saying this is perfect. It's not. Right now it needs a clear, specific prompt to do its best work. Complex apps with unusual requirements will trip it up. Edge cases exist. But it's a start. And it's a start that already works faster than most developers expect.

My goal is simple. I want Rapidfy to be for Rails what v0 is for React. You describe what you want. You get a working Rails app back. Built with Rails, for Rails.

I'm getting there.

If you want to see what 32 Rails native components look like, start here rapidrails.cc/docs/
And if you want to follow along as Rapidfy gets better, I'll be sharing demos and progress right here.

Meet Rapidfy 🚀