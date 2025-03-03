---
title: "Will AI Displace Software Engineers?"
date: 2025-02-27T23:35:28-06:00
draft: false
tags: ["ai", "economics"]
author: "Tenzin Wangdhen"
showToc: true
TocOpen: false
hidemeta: false
comments: false
description: "AI, Jevons Paradox, and the Future of Software Engineering"
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

## AI, Jevons Paradox, and the Future of Software Engineering

The year is 1994. A groundbreaking technology is proliferating—one that would expose all of human knowledge to everyone, anywhere in the world. Industries relying on information scarcity trembled. Newspapers, encyclopedias, travel agents, and countless others faced an existential threat.

That technology was the **Internet**, and while it did eliminate many jobs, it created exponentially more. Entire categories of work—social media managers, SEO specialists, app developers—emerged that would have been impossible to predict.

Fast forward to 2025, and a similar panic is unfolding. This time, the technology is **AI**. With each new model release demonstrating capabilities that seemed impossible just months earlier, most now believe mass displacement is inevitable—ironically, even for those primarily responsible for automation: software engineers. As one viral post put it: "The last job automated will be automating jobs."

In this post, I'll explore the history of technological job displacement, how it often creates a net _increase_ in jobs through mechanisms like Jevons Paradox, what parts of engineering work remain stubbornly human, and how to position yourself for the AI-augmented future of our profession. 

### Technological Unemployment Is Not New

Since antiquity, automation has displaced jobs but created new ones. 

Water Mills in the Roman Empire (1st-4th centuries CE) provide one of the clearest early cases. The introduction of water-powered grain mills displaced many manual grain grinders, who traditionally were often enslaved workers or lower-class laborers. 

However, new specialized roles were created. There was a need for millwrights who designed and maintained the complex gear mechanisms, hydraulic engineers who designed water management systems, and contractors who built aqueducts and water channels to power the mills, to name a few.

This pattern has repeated for other major inventions and automations: railroads, the printing press, textile mechanization, and computers. In every case, the incumbent workers trained only in the old paradigm end up losing out.

Due to this pattern of net job creation, labor's share of GDP has remained stable even as new industries emerge for at least the [last 200 years](https://www.maximum-progress.com/p/agi-will-not-make-labor-worthless). 

The question is whether [this time is different](https://www.lesswrong.com/posts/KFFaKu27FNugCHFmh/by-default-capital-will-matter-more-than-ever-after-agi#The_default_outcome_). Due to AI, capital can finally break free of the need for labor, perhaps even for those creating and optimizing the AI systems.

## What is a job?  

Before we dive into the question of whether "AI will take all jobs," we should first define what we mean by a job. A job, specifically white-collar professions such as "software engineer," is a collection of tasks carried out towards some broader goal. Your employer probably didn't hire you specifically just to write code, unless you are junior or the task is very complex (such as foundation model training). 

Each task that makes up a job can be more or less liable to be replaced by an AI system. 

Usually, from the outside, your mental model of a job is probably marked by some stereotype you have about the role, and then you ask yourself whether you see an AI able to perform that stereotype. 

You imagine a therapist sitting on a couch, asking probing questions about how you feel, and you say sure, an AI can totally do that. For software engineers, people typically visualize a sun-starved young man in a hoodie hunched over a laptop in a dark room as green text flickers across the screen. Unfortunately, the reality is more banal. In a large company, you don't have the luxury of dreaming up some feature and then getting it into production after some blissful coding sessions. 

To properly understand AI's impact on software engineering, we need to examine what current AI systems can and cannot do effectively.

### Current AI Strengths and Weaknesses

Currently, AI excels at handling isolated coding challenges, such as those found on LeetCode or function generation tasks, where the problem is well-defined, and the solution follows recognizable patterns. It is particularly effective at automating repetitive tasks, including generating boilerplate code, writing tests, and even drafting documentation, as seen in tools like GitHub Copilot and Cursor. My own experience demonstrates this evolution - while a couple years ago I was merely [experimenting](https://tenzinwangdhen.com/posts/semantic-movie-search-with-gpt/) with 'vibe-coding' by copy-pasting from GPT-4, today I can generate a working POC in under an hour with Cursor Composer and Claude 3.7, complete with tests, polished Tailwind CSS, and a functioning backend, all primarily through natural language. The productivity gains for greenfield projects and research-driven tasks are substantial, with tools like Glean making knowledge discovery faster and more efficient.

AI struggles with contextual reasoning, particularly when it comes to deciding _what_ to code. Understanding company-specific nuances—such as interpreting Slack discussions, navigating legacy systems, and aligning with long-term business goals—remains beyond AI's capabilities. Tasks like quarterly planning, RFC processes, and incident management still require human oversight, as they involve judgment, negotiation, and adaptation to evolving priorities. Additionally, AI faces significant challenges when working with legacy systems. Integrating AI-generated code into monolithic, decades-old architectures is far from straightforward, as these systems often contain undocumented dependencies, unconventional design patterns, and business logic that AI cannot easily infer.

What I've realized through this rapid evolution is that the bottleneck has shifted - the most time-consuming part of the development process is now planning and providing the correct context to the AI. Effective prompt engineering has become a crucial skill - the ability to concisely explain tasks and demonstrate sufficient technical knowledge to narrow the solution space. For example, specifically requesting React and Tailwind yields much faster results than vaguely asking for a "nice looking UI." This shift highlights how engineering roles are evolving from implementation-focused to more design and direction-oriented work.

To better understand how these AI capabilities translate to real-world engineering work, let's examine a typical day for a senior engineer and identify which tasks are ripe for AI assistance and which remain human.

### Day in the Life of a Senior Engineer

One of the tasks of a senior engineer is to propose and implement ideas that will improve something about the company. The ideas that are likely to be adopted depend on many variables: pain points, management priorities, staffing, costs, etc. Thinking up viable ideas given these factors is currently best left to humans due to the context required.

Once you've identified a promising project, you propose it in a planning process. This involves meetings where your idea competes with other priorities. This task is difficult to automate.

Assuming your project is approved, you develop an RFC document describing importance, technical requirements, implementation plan, timeline, and alternatives. The actual writing of the RFC is likely best left to AI with human oversight. LLMs need sufficient context about your organization, either through prompt context or RAG systems.

After the RFC is approved, you implement the idea. Some coding tasks are definitely automatable - LeetCode-style problems, greenfield projects in popular languages, and test writing are ideal for AI generation.

A feature requiring large-scale refactoring on a million+ line codebase is more challenging due to LLM context limitations. The AI needs to search for relevant code, which is harder with poor documentation and naming.

Once code is written, you solicit PR reviews from peers. There are tools aiming to automate PR review with AI already. Human review becomes more important when the code was AI-generated to catch subtle bugs or hallucinations.

Finally, you deploy your change. Deployment should use traditional automation, with AI potentially helping detect issues via anomaly detection. If your change causes a production incident, root cause analysis requires context about your tech stack and access to logs, metrics, and dashboards that current AI systems lack.

Here are the major tasks identified, and whether AI can currently replace that task:

| Engineering Task | Current AI Capability | Human Involvement | Notes |
|-----------------|---------------|-------------------|-------|
| Idea inception | Limited | Mostly human | Requires understanding organizational context, historical knowledge, and business priorities |
| Quarterly planning | Very limited | Fully human | Involves negotiation, politics, and cross-team coordination |
| RFC document writing | Strong | Human + AI | AI can draft content but humans must provide context and review |
| Coding (standard tasks) | Very strong | Minimally human | Particularly effective for greenfield projects and standard patterns |
| Refactoring legacy code | Moderate | Mostly human | Limited by context windows and understanding complex interdependencies |
| Test writing | Strong | Minimally human | Excellent at generating comprehensive test cases |
| PR review | Moderate | Mostly human | Tools exist but human judgment needed for architectural decisions |
| Incident management | Limited | Mostly human | Root cause analysis requires deep system understanding |

These tasks are just within the context of creating a feature. There are several other tasks I have not covered: team standups, presentations, etc. 

There are also some new tasks and skills that are starting to become necessary for engineers:

- Prompt engineering
- Tradeoffs between LLM providers and APIs
- Vector databases
- Retrieval Augmented Generation
- Effective use of coding copilot systems
- AI content management
- Building Agentic workflows

While this workflow represents traditional engineering environments, a new breed of companies is emerging that fundamentally reimagines how software is built in the AI era.

### AI First Companies

The engineering workflow described above primarily reflects processes in established enterprises. In contrast, AI-first companies operate with different organizational structures and development paradigms.

These organizations leverage AI capabilities extensively throughout their development lifecycle, significantly compressing the time between ideation and deployment. By integrating AI tools into core engineering processes, these companies maintain leaner technical teams composed of engineers who excel at AI-augmented development.

AI-first companies typically exhibit several defining characteristics:

- Streamlined engineering teams with higher leverage per engineer
- Integrated AI tools across the entire development pipeline
- Reduced organizational overhead and coordination requirements
- Accelerated product iteration cycles

These companies often extend AI adoption beyond engineering to functions like marketing, customer support, and operations. This comprehensive approach reduces cross-functional dependencies and administrative overhead, enabling more agile product development.

Engineers at AI-first companies require a distinct skill profile that combines traditional software engineering expertise with proficiency in AI-augmented development tools. This includes mastery of prompt engineering, AI development frameworks, and efficient collaboration with AI systems.

The emergence of these organizations represents a structural shift in how software products are conceived, built, and delivered rather than merely a tactical adjustment to existing processes.

Given how both traditional and AI-first companies are evolving, what does this mean for engineers at different career stages?

### Impact on Engineering Roles

#### Junior Engineers
For junior engineers, the baseline expectations are shifting. Pure coding ability—once the core of entry-level roles—is becoming less critical as AI tools handle routine tasks. Instead, success depends more on abstract reasoning, problem decomposition, and effective collaboration with AI. Those who fail to integrate AI into their workflow risk displacement, as companies may opt for a smaller team of AI-augmented developers rather than hiring large cohorts of junior engineers. 

However, AI can also be a powerful learning tool, accelerating skill development for those who embrace it. Understanding the fundamentals of computer engineering, such as data structures and algorithms, as well as general skill in problem-solving will still be important. Junior devs who only use the tools without understanding the fundamentals won't know when and why to stray from what the LLM is generating, or come up with novel solutions that are not in the training set. Those who understand the fundamentals as well as how to use the tools will be on the fast track to becoming senior engineers.

#### **Senior+ Engineers**

For senior engineers, AI doesn't eliminate responsibilities—it shifts them. The emphasis moves toward problem-framing, system architecture, and cross-functional collaboration, areas where human judgment is irreplaceable. Senior engineers shift to becoming "product owners" that understand the business context and assign tasks given that context to AI agents. Engineers who adapt well may find AI accelerating their career progression, as increased productivity and strategic thinking could shorten the time to promotion. However, AI cannot be held accountable for architectural decisions, security risks, or ethical trade-offs. The responsibility for those choices—and the consequences—still falls on senior engineers. The line between senior engineers and product managers is becoming more blurred - senior engineers can no longer simply take tasks from product managers and implement them. Instead, they need to be able to understand the business context and assign tasks to AI agents.

With these role transformations in mind, let's examine the broader economic arguments for and against AI's impact on total engineering employment.

## Bull and Bear Case for Eng Jobs
### Jevons Paradox and the "Expanding Pie"

The bull case for engineering jobs is the concept of "[Jevons Paradox](https://en.wikipedia.org/wiki/Jevons_paradox)," which is the idea that increased efficiency leads to increased consumption. Adding another lane to a 5-lane highway can ironically _increase_ traffic, because drivers that otherwise would have taken public transport or carpooled may be lured to attempt to drive themselves. 

In fact, I feel I have gotten _more_ busy after LLMs than before. I am now tackling more technically challenging projects that I would have shied away from due to lack of knowledge, and am more able to retain a flow state while working since I don't have to spend any time looking up syntax, and much less time looking up docs. My pattern is starting to develop into: kickoff the ideation with deep research, come up with a spec, break it down into prompts with a reasoning model, pass each prompt to Cursor composer, and just start iterating. I can usually go from idea to working POC within an hour this way. This approach is described in detail [here](https://harper.blog/2025/02/16/my-llm-codegen-workflow-atm/). 

With a lower barrier to entry for creating software, more software will inevitably be created. This means more engineers could be needed to support this increasing pool of software systems, which still need cloud services, monitoring tools, security, etc. 

New classes of tasks that did not exist a couple of years ago are being created. There are already a few entirely new jobs as well, such as "AI Engineer." 

Some venture capitalists argue that AI fundamentally reduces the need for large engineering teams. Investors like Chamath Palihapitiya, through firms like [8090](https://x.com/chamath/status/1745542094696145103), suggest that AI-driven efficiency means startups can achieve the same output with fewer developers. 

The counterpoint to this argument is _ambition inflation_—as AI reduces the cost of software development, companies may simply scale their ambitions instead of reducing headcount. If a team that once built a product with 10 features can now deliver 100, the demand for skilled engineers remains strong. Instead of eliminating jobs, AI may push the industry toward building more complex, ambitious software at a faster pace.

Regardless of whether the total number of engineering jobs increases or decreases, one thing is clear: the nature of these jobs is changing rapidly, and adaptation is essential for career longevity.

### Adapting to the AI Era
The engineers who thrive won't be those who resist AI tools, but those who learn to use them, and develop a sense for their strengths and weaknesses. This means mastering AI-augmented IDEs like Cursor, developing the meta-skill of prompt engineering, and becoming adept at debugging the sometimes-flawed outputs these systems generate. Yet paradoxically, as AI capabilities expand, distinctly human skills become more valuable, not less. The ability to frame problems correctly before an AI even sees them, to collaborate across teams with different priorities and knowledge bases, and to make ethical judgments that reflect our values rather than statistical patterns in data—these become the differentiators. Perhaps most importantly, complacency represents the greatest career risk in this environment. Those who assume their current skills will retain value without adaptation risk displacement by more adaptable colleagues. Continuous learning isn't just beneficial; it's non-negotiable in a landscape where the technological ground shifts beneath our feet almost daily.

While current AI systems are already transforming engineering work, the ultimate question looms: what happens when AI reaches or exceeds general human capabilities?

---
### AGI and the Long-Term Engineering Landscape

While most of this article addresses the immediate impacts of current AI systems, it's worth considering how Artificial General Intelligence (AGI) might reshape software engineering more fundamentally. Unlike today's specialized systems, AGI would theoretically match or exceed human capabilities across all cognitive tasks.

#### Economic Constraints Will Still Matter

Even as AGI becomes technically feasible, economic constraints will likely determine its adoption patterns. Training costs for advanced AI systems remain substantial - AlphaGo's $35 million training expense provides a reference point. These costs suggest that human-AI collaboration will persist in domains where the marginal benefit of full automation doesn't justify the expense. 

Software engineers working on problems with lower economic value (like maintaining internal tools or building systems for smaller markets) may retain their roles longer than those in high-value domains where AGI investment would yield significant returns. The principle of comparative advantage suggests humans may specialize in areas where our cost-to-capability ratio remains competitive.

#### Task Evolution, Not Job Elimination

As with current AI tools, AGI will likely transform engineering tasks rather than eliminate engineering roles wholesale. Staff+ engineers might shift from writing code to defining and refining system objectives, establishing performance criteria, and ensuring alignment between technical capabilities and business needs. This represents a continuation of the trend we already see, where increasing automation elevates human work to higher levels of abstraction.

Historical patterns support this view. Despite centuries of productivity improvements, the 40-hour workweek persists, as do professional roles. Technology has consistently transformed the nature of work rather than eliminated it entirely. Engineering will likely follow this pattern, with AGI handling implementation details while humans manage higher-order concerns.

#### New Engineering Specializations

AGI would likely create entirely new engineering specializations. We might see roles emerge around:

1. **AGI Alignment Engineering** - Ensuring AGI systems maintain appropriate goal alignment
2. **System Orchestration** - Designing workflows that optimally combine AGI capabilities
3. **Human-AGI Interfaces** - Creating effective collaboration mechanisms between humans and AGI systems
4. **AGI Supervision** - Monitoring AGI outputs for accuracy, bias, and alignment with intended outcomes
5. **Context Engineering** - Understanding the business context and ensure the context provided to the AGI is correct and up to date

Rather than engineering jobs disappearing, they would transform into roles focused on defining problems clearly, evaluating solutions critically, and directing AGI resources effectively. The core engineering skills of systems thinking, trade-off analysis, and problem decomposition would remain valuable, even as the technical implementation shifts to AGI systems.

In this scenario, the competitive advantage goes to engineers who develop metacognitive skills - understanding how to frame problems in ways that AGI can optimally address, recognizing the limitations of automated solutions, and maintaining the human judgment necessary to evaluate AGI outputs critically.

---

## **Conclusion**  
AI is here and is at a level where it can do some tasks much more proficiently than human engineers. However, it is still a ways off from doing the whole job as there are tasks within the job that are difficult to automate. Over time the tasks will be automated, but more, higher-level tasks could also be created, such as tuning the AI prompts or providing the right context to the AI. Every time technology has increased efficiency in the past, there is a subsequent increase in overall demand, though incumbents and laggards are liable to be replaced. As it currently stands, AI won't replace engineers, but engineers that are proficient with AI tools will replace those that are not. Perhaps the best advice here is to remain on the forefront of adopting AI tools and being adaptable as the skills required change. As William Gibson famously said, "The future is already here—it's just not evenly distributed."
