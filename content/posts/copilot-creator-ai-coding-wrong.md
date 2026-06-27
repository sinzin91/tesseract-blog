---
title: "The Power of Focus in Agentic Coding"
date: 2026-06-27T15:33:40-07:00
draft: false
tags: ["ai", "agentic-coding", "productivity", "startups", "opinion"]
author: "Tenzin Wangdhen"
showToc: true
TocOpen: false
hidemeta: false
comments: false
description: "Alex Graveley, the creator of GitHub Copilot, showed me how I've been thinking about AI coding wrong."
disableHLJS: false # to disable highlightjs
disableShare: false
hideSummary: false
searchHidden: false
ShowReadingTime: true
ShowBreadCrumbs: true
ShowPostNavLinks: true
ShowWordCount: true
ShowRssButtonInSectionTermList: true
---

Alex Graveley, the guy behind GitHub Copilot, showed me how I'd been thinking about agentic coding wrong.

He used pen and paper.

Alex dropped by our Mission Street office on a Tuesday. I told him we had about a week of work left to get self-serve working.

"No. That should take 4 hours."

He pulled out a sheet of paper, wrote down ten steps, and handed it to me. That was the plan.

---

I've been through every phase: ChatGPT copy/paste, Cursor, Claude Code, Codex, back to Claude Code, subagents, orchestration frameworks, 10 parallel sessions. The assumption was that more tooling meant more output.

That assumption is half-right. The real constraint is undirected scope, not the model or the tools. Without a fixed list, parallel agents multiply your WIP, not your throughput. You end up with ten sessions, five half-finished features, and nothing shipped.

Parallelism only helps when every agent is pointed at the right thing.

---

Agentic coding makes hard things look like low-hanging fruit. A feature will only take an hour, so you fire off a session. Then another. Suddenly you're ten commits deep in a niche optimization nobody asked for. Alex calls this "fake work"—all the motion that doesn't move a metric or unblock a user. In startups, the only thing that matters is getting users and removing blockers to that goal.

Alex's process is blunt: ten steps on paper, two Claude Code sessions, nothing else. Work through the list until done. The checklist is a contract with yourself about what ships today.

He starts every project with a fresh CLAUDE.md. He only adds lines when something annoys him. Claude keeps asking whether to commit? Update the file. He calls it annoyance-driven development: you don't pre-load rules, you earn them through friction. Here's where we ended up by the end of our session:

- keep everything as concise as possible
- no unnecessary abstractions
- no unnecessary comments
- when the next step is obvious, do it without asking
- always be as general as possible
- parallelize independent steps whenever possible

On prompting: I stuff agents with context, design docs, previous sessions. He keeps prompts short. For tasks a thousand teams have already solved (ECS, auth, DB migrations), more context means more noise. The model already knows the generic path; what it doesn't know are your sharp constraints, which is where you spend your context budget.

On early optimizations: I default to thinking about reliability and scaling way ahead of when it matters. "Building for scale with a handful of users is avoidance."

---

This changes scope discipline, not the quality bar. The ten-step list is a forcing function, not a shortcut around quality. When you ship a step, you verify it works. The discipline is in writing the list tightly enough that each step has a clear done-state: something you can check, not just declare.

What it does kill is architectural drift from over-scoped agents. Short prompts, tight scope, explicit review at each step. That's the governance. Simple, but it holds.

---

By the end of our session we were on step 6 of 10.

"Don't stop until you finish the list. Tomorrow, rinse and repeat."

By night, anyone who requested access could onboard end-to-end.

---

In the age of agentic coding, focus is the superpower. Scope creep that *feels* like velocity is eating most of your AI-coding gains. More sessions, more output, less of what actually matters shipped.

I came into that conversation thinking the bottleneck was tooling. More parallelism, richer context, smarter orchestration.

I left with a crossed-off list on a sheet of paper. The milestone shipped.

Look at what you're working on right now. Can you write it down in clear steps on a sheet of paper? If not, you're running on a vibe, not an explicit plan.
