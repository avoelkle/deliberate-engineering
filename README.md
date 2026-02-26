# Deliberate Engineering — A Developer's Guide
### Working Principles for Developers in the Dawning Agentic Era

---

## Preface

> **Remark:** This is intentionally detailed. You can skip directly to the seven principles below but you may want to come back to the preface later — it makes the reasoning behind the principles traceable, not just the principles themselves.

The way we work is changing. Coding agents can generate, refactor, and review code at a speed no individual developer can match. Much has been written about what this means for the market — the disruption of SaaS business models, the compression of software development costs, the reshaping of entire industries. That debate is important, but it is not the focus here. This is about the perspective of the developer: what it means for how you think, work, and take responsibility for what gets built.

This guide is neither a reaction to the slop loop movement nor a rejection of AI augmentation — it is a positive position. Call it **Deliberate Engineering**: a way of working that treats analytical rigour, judgment, and intentionality as the core of the craft, and AI agents as a powerful multiplier of those qualities rather than a replacement for them. It stands in equal opposition to the mindless velocity of the slop loop — the Gastown and Beads school of armies of parallel agents churning out code with minimal human oversight, dopamine-driven productivity theatre where speed is mistaken for progress and volume for value — and to the opposite tradition of Enterprise Software frameworks, with their endless layers of abstraction and indirection that make tracing a simple data flow almost impossible, and mistake architectural conformity for good engineering. Spring Boot at full enterprise ceremony and .NET's DI container conventions are the most familiar expressions of this. Both failure modes arrive at the same destination: complexity that wasn't earned, abstraction that wasn't needed, and code that nobody fully understands anymore. Both fail for the same reason: the human in the loop stopped thinking critically.

Deliberate Engineering works best in small enough teams where ownership is real, trust is earned, and analytical disposition is not just welcomed but required. This is not a coincidence. The approach demands capabilities and a process that were rarely incentivised before — the ability to reason about systems, question assumptions, direct agents precisely, and take full responsibility for what gets built. These are not skills or habits that frameworks or velocity metrics develop. They are the disposition this guide is written for.

An agent given a vague instruction by a confused developer produces confident, fast, wrong code. Your job is increasingly to think clearly, plan deliberately, direct precisely, and review critically. The principles below are not just about writing code. They are about working effectively as the intelligence that guides and corrects the machine.

There is a subtler version of this risk that the discourse rarely names. Cognitive capacity is finite. Every hour you spend on work an agent could do — mechanical synthesis, boilerplate, repetitive refactoring — is an hour not spent on hypothesis formation, design judgment, and architectural decisions. These aren't equivalent activities competing for the same slot; they're in direct competition for the same bounded resource. The developer who insists on doing commodity work manually isn't protecting quality — they're crowding out the cognitive work that actually determines it. Deliberate Engineering is in part an argument about where your attention goes.

The difficulty is compounded by the discourse surrounding these tools. A steady drumbeat of AI executives and investors — many with undeclared financial stakes in adoption — produce emotionally manipulative, algorithmically optimized content: existential framing, identity threat, urgency dressed as public service. It is designed to go viral, and it does. Deliberate Engineering is in part a response to this: a reasoned position derived from principles, not a reaction to fear or hype. The developer who panics into the slop loop and the developer who retreats into sceptical rejection have both been moved by the same external force. Neither is thinking clearly.

This guide was developed in the way it recommends. A coding agent was used throughout — for research, drafting, critique, and iteration — but every direction, pushback, and editorial judgment came from the human in the loop. The trajectory was seeded deliberately, at every step that mattered. The process and the product are consistent with each other. The output is evidence of the method.

```
Plan.
Instruct.
Review.
Contextualize.
Validate.
Ship.
Repeat.
```

Keep your energy on what requires a human mind. Steer by adding the business context, intent and constraints the agent can't know — manual edits are the exception, not the workflow.

---

## Principles

### 1. Understand Before You Act

A codebase is the materialized collective mental model — and accumulated confusion and artifact of the misunderstandings — of everyone who built it. Before writing a line — or instructing an agent — build yours. Trace the data. Follow the flow. Know *why* the system behaves as it does, not just *that* it does. Know what you know and know what you don't know. There is usually a significant cost to not-knowing or not-understanding. Unexamined gaps don't stay neutral — they become assumptions, assumptions become wrong conclusions, wrong conclusions become either endless iterations that never solve the bug, or new bugs shipped.

In agentic coding, this is empirically grounded. Large-scale analysis across 60,000 agent trajectories shows that runs diverge within the first 1% of tokens: a single word difference in early reasoning cascades into fundamentally different solution strategies, frequently with opposite outcomes (Bjarnason, Silva & Monperrus, *[On Randomness in Agentic Evals](https://arxiv.org/abs/2602.07150)*, arXiv:2602.07150, 2026). Your instruction seeds the trajectory. Vague input doesn't produce vague output — it produces a confidently wrong path, locked in early, that no downstream review fully recovers.

This is the mechanical indictment of the slop loop: spinning up armies of agents on a poorly understood problem doesn't multiply your chances of success — it multiplies your trajectories of confident wrongness, burning through endless tokens and benefitting mostly the AI provider.

Therefore: before instructing an agent, require it to demonstrate understanding first — restate the problem, trace the relevant code paths, surface what it doesn't know. An agent that cannot articulate the problem clearly has already diverged.

*Stop throwing spaghetti at the wall and watching what sticks.*

---

### 2. Build the Dual Mental Model

Understanding the problem is necessary. It isn't sufficient. You also need to understand how the agent will approach it — and those are two different things.

Call this the **dual mental model**: the conjunction of task-level understanding and system-level understanding. Task-level is knowing what the correct solution looks like — the right data structure, the right algorithm, the right architectural pattern for the problem at hand. System-level is knowing how the agent selects its approach — and specifically, where it will default to the most probable response from its training distribution rather than the most appropriate response to your specific problem.

This distinction matters because agents don't evaluate whether their chosen approach is sufficient. They produce what is most likely given the instruction. Give an agent a panel dataset and ask whether X affects Y, and it will apply fixed effects — because fixed effects is the overwhelmingly modal approach in the literature it was trained on. It won't ask whether fixed effects is enough. It won't notice the time-varying confounder. It will produce a well-structured, statistically detailed, confidently presented output. It will also be wrong.

The developer with only task-level understanding — who knows the right approach but doesn't understand the agent's default behavior — may still write an underspecified instruction, assuming that identifying the problem is sufficient. It isn't. The agent must be told not just *what* to achieve, but *how* to achieve it, constrained at precisely the points where its defaults diverge from the correct approach. That constraint can only be applied by someone who understands both the task and the system.

This has three practical implications. At instruction time: encode both your understanding of the problem and your understanding of the agent's defaults into the prompt — specify acceptance criteria, methodological constraints, the approach you want, and why. At review time: evaluate output not just against your expectation of what's correct, but against your knowledge of how the agent characteristically fails — plausible-but-wrong output is the failure mode to watch for, not obviously broken output. Between the two: monitor intermediate outputs for early divergence. A trajectory that has departed from the correct path at step two won't self-correct at step ten.

The practical payoff of the dual mental model is instruction efficiency. The alternative — iterative prompting, trial-and-error refinement — can eventually converge on the right answer, but it requires the developer to recognize errors in intermediate outputs at every round, which itself presupposes the domain knowledge that would have enabled a precise instruction in the first place. Front-loading that knowledge into the instruction is faster, cheaper, and produces better results.

*Knowing what the right answer looks like is not the same as knowing how the agent will miss it.*

---

### 3. Surface Confusion. Surface Tradeoffs.

Unresolved ambiguity doesn't stay abstract. It gets implemented — by whoever acts next, often implicitly, often wrongly. In a human team, that's a colleague making a judgment call without the context to make it well. With an agent, it's faster and quieter: the agent fills the gap with whatever its training distribution makes most probable, not what your specific problem requires. By the time you see the output, the wrong decision is already baked in.

So name the confusion. Say what you don't know — in a ticket, in a comment, to your team, in the instruction itself. Name the tradeoffs. Make the unknowns visible before they get decided for you. The instruction you write is the last point at which ambiguity is yours to resolve. After that, it belongs to the agent.

*Clarity is your responsibility. Ambiguity doesn't disappear — it just gets delegated.*

---

### 4. Form Hypotheses. Then Try to Break Them.

This is the scientific method applied to engineering. Before you touch anything — or hand anything to an agent — form an explicit hypothesis: a precise, stated belief about what is happening and why. Then do something Karl Popper would recognise: try to prove yourself wrong. You cannot confirm a hypothesis by finding evidence that supports it — you can only fail to disprove it. So actively look for the observation or experiment that would contradict it. What would have to be true for your explanation to be wrong? Run that test first.

This is how you fail fast usefully. A hypothesis that collapses under the first contradicting observation has still moved you forward — it has eliminated a wrong model cheaply, before you built on top of it. The spaghetti-thrower fails slowly, accumulating confusion. The deliberate engineer fails fast, deliberately, on the way to clarity.

*If you can't state what would prove your hypothesis wrong, you don't have a hypothesis — you have a preference. That's not a foundation to build on.*

---

### 5. Simplicity Is the Goal, Not the Compromise

You don't yet fully know what the solution needs to be — and this is not a weakness; it is the normal condition of any non-trivial problem with the minimal information set at the beginning of a trajectory. The same perspective that exposes enterprise framework over-commitment applies at the code level: investing in abstraction and generality before you understand the problem is the same mistake with a smaller blast radius. Start with the simplest thing that could possibly work — hard-code the config value, use the flat structure, skip the abstraction layer when only a single implementation is foreseeable — or if unavoidable, use the thinnest layer possible. Let the solution grow until its shape becomes clear through validated learning iterations, then decide how to structure it. Premature abstraction is just premature optimization with better vocabulary. The half-life of software is shorter than it has ever been, which means the cost of complexity you didn't need is increasingly likely to be paid in full and for nothing. Before you write — or before you start instructing — ask yourself as well as the agent: what is the simplest possible solution?

*Complexity should be earned by necessity, not installed in anticipation.*

---

### 6. Your Code Is a Message to the Future

Principles 1 and 5 apply to the code itself. Principle 1 says: build a mental model before you act. Principle 5 says: keep complexity from accumulating silently. Now apply both from the reader's perspective — the person, or the agent instructed to extend this code, who arrives later under pressure (human) or without enough context (agent), and has to reconstruct that mental model from scratch.

You are writing for that person — and possibly yourself. A clever solution that requires reverse-engineering your reasoning is not clever. A clear solution that makes the mental model obvious is the hard thing, and the right thing. Every unnecessary abstraction, every unexplained indirection, every piece of complexity that wasn't earned by necessity is cognitive load you are handing to someone else without their consent.

Sounds like clean code? Not quite. Clean code is focused on surface: naming conventions, formatting, short functions, self-documenting code, readability as prose. Rule-based and prescriptive. The question it answers: *does this code look right?*

The approach here is more in line with Simple Code (John Ousterhout, *A Philosophy of Software Design*): primarily about structure — reducing cognitive load, managing dependencies and obscurity, designing deep modules with simple interfaces that hide complex implementations. The question it answers: *how much does a developer need to hold in their head to work on this?*

In short: clean code is about how code looks. Simple code is about how complexity behaves.

*The mental model should be in the code, not in your head. If it needs explaining, it needs rewriting.*

---

### 7. Leave It Better Than You Found It

Let's end on a lighter note — this one is borrowed from the Boy Scouts: always leave the campsite better than you found it.

If you notice something small that can be cleaned up without risk, clean it up. Not as a separate project — just as part of being here. It usually doesn't need a ticket, a long discussion, or even a refactoring sprint. It needs two minutes and the right instinct.

*The codebase is a shared space. Act accordingly.*

---

## A Note on Origins

The ideas here are a mix of things. Some may be original. Many have been shaped by years of working alongside great engineers on hard problems, and by conversations that border on philosophy as much as engineering. Some are independently convergent with recent public thinking from people like Andrej Karpathy, whose January 2026 observations on LLM coding behaviour articulate much of what practitioners have long felt; Armin Ronacher, whose writing on agent psychosis and the slop loop is essential reading; and the GitHub resource compiled by Forrest Chang distilling Karpathy's principles into practice. The dual mental model — the conjunction of task-level and system-level understanding — is an original construct introduced in a companion working paper (Völkle, [*AI in Academic Research: Toward a Framework for Epistemically Responsible Deliberate Adoption*](https://papers.ssrn.com/sol3/papers.cfm?abstract_id=6300178), 2026), where it is grounded in a controlled experiment demonstrating that instruction precision driven by domain knowledge is the binding constraint on AI output quality. The simplicity imperative has deep roots — John Ousterhout's *A Philosophy of Software Design*, Dave Thomas and Andy Hunt's *The Pragmatic Programmer* (whose influence on how developers should think about their craft, not just their tools, runs through much of what is written here), and the Agile Manifesto's framing of simplicity as "maximizing the amount of work not done" are all ancestors of what is written here. This is not a claim of originality where none exists, but a synthesis from experience, shaped by these and others. This is my personal position. I hold it with conviction but not certainty — which, given Principle 4, seems appropriate.

In the same spirit as Hunt and Thomas, who paired *The Pragmatic Programmer*'s philosophy with concrete, tool-level practices — and Forrest Chang, whose GitHub resource distils Karpathy's principles into direct practitioner guidance — an `AGENTS.md` has been added to this repository. It translates these principles into instructions for coding agents directly, so the guide applies not just to how developers think, but to how they direct the tools they now work alongside. Consider it the deliberate alternative to the "get shit done and make assumptions" school of agent orchestration.


© Arndt Voelkle, 2026. MIT License — free to use and adapt with attribution.
