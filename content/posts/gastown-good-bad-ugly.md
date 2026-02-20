---
title: "Gas Town: The Good, The Bad, The Ugly"
date: 2026-02-19T12:00:00-08:00
draft: false
# weight: 1
tags: ["AI", "Agents", "Orchestration", "Review"]
author: "Tenzin Wangdhen"
showToc: true
TocOpen: false
hidemeta: false
comments: false
description: "Why I didn't become a citizen but learned a lot about agent orchestration"
disableHLJS: false # to disable highlightjs
disableShare: false
hideSummary: false
searchHidden: false
ShowReadingTime: true
ShowBreadCrumbs: true
ShowPostNavLinks: true
ShowWordCount: true
ShowRssButtonInSectionTermList: true
UseHugoToc: true
editPost:
    URL: "https://github.com/sinzin91/tesseract-blog/blob/master/content"
    Text: "Suggest Changes" # edit text
    appendFilePath: true # to append file path to Edit link
---

![Gas Town](/images/gastown/gastown-retro-noir-wasteland.png)

I used Gas Town for a week. 

I ended up with three mayors, 141 orphaned Claude Code processes and a new appreciation for why agent orchestration is hard. 

[Gas Town](https://github.com/steveyegge/gastown) is Steve Yegge's framework for orchestrating a fleet of coding agents. It shines when most of your work is pure code: lots of independent tasks, clear specs, and you mostly know what you want and don’t care how you get there. It falls apart when the work is heavily human-in-the-loop or ambiguous. Keeping Gas Town fed with [beads](https://github.com/steveyegge/beads) that align with your project goals is the hard part. 

When it works it is breathtaking. I hit the promised land briefly… then crashed into a Mad Max themed trough of disillusionment. I now use a different setup that borrows ideas from Gas Town.

---

## The Gas Town Universe

Yegge went full Mad Max with the naming. Here are the 6 terms you need to follow this post:

- **Mayor** 🎩: Your primary coordinator. You talk to the Mayor; it delegates to workers.
- **Polecats**: Ephemeral worker agents (Claude Code / Gemini CLI / Codex). One polecat per task, decommissioned after merge.
- **Beads:** Git-backed issues: the atomic unit of work. Each bead has an ID, description, status, and assignee.
- **Convoy:** A batch of related beads assigned together.
- **Rig:** A project container. One repo = one rig.
- **Refinery**: The merge queue. Handles PRs when multiple polecats finish at once, intelligently merging one at a time.

Full glossary at the end.

---

## What Is Gas Town, really?

Gas Town is an orchestration layer for parallel coding agents, with state tracked in git. Your main interface to it is a Claude Code session running in a tmux window. It tries to abstract away everything between idea and implementation even for very large projects. 

The flow:

1. You describe what you want done to the Mayor.
2. The Mayor defines work as beads (git-backed tasks with an ID + spec).
3. The Mayor slings beads to polecats (ephemeral agent workers in their own worktrees). You can also directly sling beads to polecats.
4. Polecats open PRs; the refinery merges them safely one at a time.
5. When a bead merges, the polecat is decommissioned and the rig moves on.

Gas Town is a framework, not a library. You either buy into it wholesale or you don’t. There are components of it that are modular though, such as beads. [Beads](https://github.com/steveyegge/beads) are git-backed, dependency-aware tasks for agents. It deserves its own post because it is the one thing that I continue to use. Turns out an ai-native, git-backed issue tracker is a very useful thing to coordinate work across ephemeral agents.

---

## The Good

Gas Town takes a maximalist view of agentic coding. You as the human do not look at the code, you just define the spec and task list and let Gas Town take care of the rest. It is vibe coding taken to its logical extreme. And it is magical when it works. 

> “In Gas Town, you let Claude Code do its thing. You are a Product Manager, and Gas Town is an Idea Compiler. You just make up features, design them, file the implementation plans, and then sling the work around to your polecats and crew. Opus 4.5 can handle any reasonably sized task, so your job is to make tasks for it. That’s it.” - Steve Yegge
> 

I love the concept of just talking to a main agent called the “mayor”. As someone that regularly runs 10+ claude code sessions across multiple terminal tabs and tmux panes, one of my pain points was keeping track of which agents were working on what task. The mayor handles tracking the polecats for me.

![The mayor of Gas Town](/images/gastown/gastown_mayor.png)

*The mayor of Gas Town, I assume*

The parallelization is also real. Each polecat works on its own git worktree, so they can work in parallel on beads. This is a massive speedup in development time. I extracted this idea into a skill called [smithers](https://github.com/noc0dev/smithers). One difference from other orchestrators is that each polecat is NOT a subagent, but a full Claude Code session running in another tmux window. It even supports swapping in Codex as the model via an `--agent` flag.

Part of the pain of coding with agents is that they often get blocked and won’t continue working unless you prod them along. Steve has apparently also had this issue, so he made sure Gas Town has mechanisms to account for this. The system's propulsion principle is supposed to keep agents moving — if there's work on a hook, it must run. 

Since all work is backed by git via beads, you don’t need to stress over the context of any individual polecat. A bead is only closed when a PR is merged, so until then any polecat can pick up and progress on the work. In an earlier epoch of vibe coding, we would feed a giant spec into a single agent’s context and hope to god it would follow through. Externalizing the spec via a dependency graph like beads allows the agent to focus all of its context on one bead at a time, thereby making it much more likely that the project doesn’t completely derail. 

Another clever idea is the concept of a mail system. Agents can send mail to each other, and agents check their mail. The main use case for this is that it allows polecats to send mail to the mayor when they run into issues, allowing the mayor to either address the issue or escalate to the human. 

The "aha" moment for me was when I slung 7 beads before dinner — things like "add a styled footer with alert badges," "implement fsnotify file watching," and "cache rendered views with debouncing" — all for a Go TUI dashboard I was building. Came back to find 6 PRs merged, and only one blocked on a dependency.

For projects where you know exactly what you want but it’s complex enough that you can't one-shot it, Gas Town might finish it in one night rather than a week.

---

## The Bad

Gas Town has some rough edges. This can be mainly attributed to the fact that this is an early stage, experimental project.

Stuff doesn't work out of the box. The mail system (polecat → Mayor status) needed patching when I first used it, and seemed to break down mysteriously. Background maintenance processes like the deacon (a background daemon that keeps agents working) and dogs (its cleanup helpers) simply weren’t running until I fixed them. Despite all this, agents still seem to need manual prodding to continue. 

Towards the end of my experiment with Gas Town, I noticed the ram usage on my 32gb Macbook Air was more elevated than normal. It turned out I had 141 orphaned claude code processes. The polecats were not being nuked correctly, so I needed to manually clean them up. To be fair, this issue has been [fixed](https://github.com/steveyegge/gastown/issues/736) since then. 

![Gas Town rig](/images/gastown/gastown_rig.png)

*That’s my project strapped to a Gas Town rig!*

Observability is thin. Sometimes 6 PRs merged and I had no idea when I'd slung them. Other times the system seemed stuck and I couldn't tell why. There's a web dashboard (`gt dashboard`), but I found myself in tmux panes anyway.

Tmux fluency is required. You talk to the mayor in a tmux window. Crews and polecats also run in their own tmux windows. When things break, you're diving into polecat sessions directly. I already knew tmux, but this will be a wall for those who don't.

A lot of this can be chalked up to “early stage, experimental project.” But some of the most unsettling parts are baked into the core philosophy of Gas Town.

---

## The Ugly

You must buy in to the philosophy of Gas Town to even be willing to use it. There may be nothing better at making GPUs screech in pain. Some have called it a slop factory, and for good reason if used poorly.

![Gas Town warning](/images/gastown/gastown_warning.png)

*Friendly warning from Yegge’s Gas Town blog post*

Steve does his best to warn away the faint of heart in his blog post. You need to be a level 7 vibe coder (managing 10+ agents manually) to even consider it. The “smart auto complete” crowd should show themselves out. 

> "Gas Town is an industrialized coding factory manned by superintelligent robot chimps, and when they feel like it, they can wreck your shit in an instant. They will wreck the other chimps, the workstations, the customers. They'll rip your face off” - Steve Yegge
> 

The core bargain is that you let go of the wheel. Throughput is valued over precision. Work will get lost, and you’re expected to be okay with it. You also aren’t supposed to read the code. Just hope that, somehow, the system produces something that does what you wanted.

Yegge is explicit that the system optimizes for throughput, not correctness: *most work gets done; some work gets lost.* I experienced this first hand when an agent overwrote a config file entirely. Thankfully it was recoverable via git. This is a feature, not a bug. If that makes you flinch, you’re not the target user.

> “Work becomes fluid, an uncountable substance that you sling around freely, like slopping shiny fish into wooden barrels at the docks. Most work gets done; some work gets lost. Fish fall out of the barrel. Some escape back to sea, or get stepped on. More fish will come. The focus is *throughput*: creation and correction at the speed of thought.” - Steve Yegge
> 

If it wasn’t already clear, you must be the type of person that is comfortable with running (multiple) Claude Code with the  `--dangerously-skip-permissions` flag, aka “yolo-mode”. This means Claude doesn’t ask you for permissions and will chug along even if it’s going to rm -rf your home directory. By default, Gas Town launches its agents (Mayor, polecats, etc.) with this flag enabled.

Gas Town can, for obvious reasons, get expensive. Yegge describes Gas Town as a "cash guzzler" and mentions needing multiple Claude Code accounts, each at $200/month. If you're on API billing with heavy parallel usage, costs can spiral fast. If you’ve done a poor job speccing your project and churn a lot of code, you’re effectively just burning money and tokens. Peter Steinberger, creator of OpenClaw, calls Gas Town the “[ultimate token burner](https://lexfridman.com/peter-steinberger-transcript/)”. 

---

## Why I stopped using it

I fit the bill for a Gas Town user in many ways. I have a lot of side projects and ideas that I want to validate quickly. I have 10+ Claude Code sessions at any given time. I’ve been “vibe coding” for years at this point. Despite this, I did not end up adopting Gas Town after trialing it for a week. 

![Chatting with the Mayor](/images/gastown/gastown_status.png)

*Chatting with the Mayor about chaos in my Gas Town*

I’m a believer in the Unix philosophy of tools doing one thing well. Gas Town is the opposite. I found it difficult to build a mental model of all of the moving parts. 

It is much too heavy for some projects. For something like “build a CRUD app”, where you know exactly what you want and therefore can simply create a large backlog of tickets and churn through them, it can be a great fit. For very small tasks like creating a cli tool, it often isn’t worth the hassle of creating a rig in Gas Town. For iterative tasks like building a UI, where you provide a lot of feedback before you get a good result, it is a terrible tool. 

My dream setup is the agents working autonomously and pinging me when they need direction, not me constantly prodding them to move along. Gas town showed me that is possible, but I still needed to prod the mayor(s) and it asked for too much in return.

I wanted the same parallelism and “mayor driven” workflow, but with less complexity and overhead. So I started borrowing the best ideas rather than adopting the whole system.

---

## The verdict

Gas Town is early and messy, but also brave and prophetic. It’s the first system I've used that made me feel like I was directing a team rather than babysitting terminals. It has several novel ideas that have inspired a lot of other agent orchestration tools. 

If you're already running 5-10+ coding agents regularly, are comfortable letting go of the wheel, and have large well-defined specs to crank through, Gas Town might be exactly what you’re looking for. As coding agents get more capable, I suspect people will see the need for this or related systems.

I’d skip it if you need fine-grained control and lots of back-and-forth with your agents, yolo mode makes you nervous, or are looking for *less* complexity. 

For all its chaos, Gas Town showed me what’s coming. I didn’t become a citizen, but I did leave with a new framework for managing teams of agents. I think that mental model is going to matter a lot this year.

---

## Appendix: Full Gas Town Glossary

For completeness, here's the rest of the Mad Max vocabulary:

- **Town** — Your workspace directory (e.g. `~/gt/`). The HQ for config + all projects.
- **Hook** — Git-worktree-based persistent storage for agent work. Lets work survive crashes/restarts.
- **Crew** 👷 — Longer-lived "personal agents" with sticky context. Unlike polecats, crew have persistent identities and aren't managed by the Witness. Good for design work with lots of back-and-forth.
- **Witness** 🦉 — Supervisor that checks on stuck polecats and nudges them. Runs patrols to keep things moving.
- **Deacon** 🐺 — Town-level daemon/beacon. Gets pinged every few minutes to "do your job" and propagates that signal to other workers.
- **Dogs** 🐶 — Deacon's helpers. Handle maintenance tasks like cleaning up stale branches.
- **Sling** — Assign a bead to an agent ("sling a bead to a polecat").
- **Convoy** — A batch of related beads assigned together.
- **GUPP** — "Gas Town Universal Propulsion Principle" — stated simply: *if there is work on your hook, you must run it.* This is what keeps agents moving instead of stalling.
- **Molecule** — A durable, chained multi-step workflow tracked as linked beads. Survives agent crashes and restarts. The core unit of Gas Town's workflow engine.
- **Wisp** — An ephemeral molecule destroyed after completion. Used for transient work that doesn't need to persist.
- **Formula** — A TOML workflow template that gets "cooked" into protomolecules, then instantiated into molecules or wisps. Think of it as the source code for workflows.
- **Protomolecule** — A template molecule (yes, it's an Expanse reference). Contains pre-built bead graphs with variable placeholders, instantiated into real workflows.
- **Nudge** — Real-time messaging (gt nudge) that pokes agents via tmux to check their hook and mail. The workaround for GUPP not always firing on its own.
- **Handoff** — Graceful session transfer (/handoff). Agent cleans up, restarts, and the successor picks up via GUPP.
- **Seance** — Talk to a worker's previous session (gt seance). Uses Claude Code's /resume to revive dead sessions and recover lost context.
- **Patrol** — A recurring workflow loop run by the Deacon and Witness. A well-defined sequence of checks encoded as linked beads.
- **Boot** 🐕 — A special Dog awakened every 5 minutes to check on the Deacon. Decides if the Deacon needs a heartbeat, nudge, restart, or to be left alone.
