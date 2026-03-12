# The Agile Vibe Coding, AVC ceremonies approach compared to a simple one-prompt-per-task approach 

Let's try find an aswer on some questions:
- What specific benefit AVC brings in an AI-agent environment?
- How AVC ceremonies maps (or doesn’t) to traditional Agile?
- Why AVC ceremonies are more critical with AI?

Humans can implement loosely defined stories and fill gaps intuitively. But AI:
- requires decomposed, well-scoped work
- performs better on smaller, atomic tasks
- loses quality exponentially as scope expands.

Therefore: 
- Large vague story and AI = inconsistent architecture.
- Small precise task and AI = high quality.


## The Agile Vibe Coding definition

Agile Vibe Coding defines ceremonies such as:
- **Sponsor Call** — sets context and project brief
- **Sprint Planning** — decomposes project scope into `epics/stories/tasks/subtasks`
- **Seed** — breaks stories into atomic work units
- **AI Coding** — generation phase
- **Context Retrospective** — reflection and improvement


## One-Prompt vs Ceremonies

### ⛔ One-Prompt approach

A “one prompt” approach means:
- User writes a single prompt
- LLM generates code (and maybe `infra/pipeline/tests`)
- Output is deployed

Pros
- Fastest possible turnaround
- Low cognitive overhead

Cons
- No structural breakdown of work
- No shared human understanding beyond the prompt
- Hard to validate AI output reliably
- No integrated mapping from `vision → architecture → tests`

> [!TIP]
This often becomes ___“roll the dice coding”___ — a process driven by repeat prompting and inspection rather than intentional design. 

Research has shown vibe coding without structure often leads to stochastic results and unpredictable quality because it prioritizes generation over rigorous review and decomposition.


### ✅ Ceremonies approach — better aligned with engineering discipline


#### 1. Breaks down work purposefully

> - ℹ️ **In traditional Agile, sprint planning selects what will be built and how**. 
> - ℹ️ **In AVC, sprint planning creates epics and stories with context before AI writes anything** — so AI agents don’t just generate code out of nowhere.

This means:
- Requirements are explicit
- Scope is decomposed
- Context is propagated down to tasks/subtasks

> [!IMPORTANT]
> This is essential for complex software delivery and QA.

#### 2. Encourages shared understanding

> ℹ️ Traditional Scrum ceremonies exist for alignment and inspection/adaptation (e.g., sprint planning, review, retrospective). 

Each event has a clear purpose: 
- planning, 
- executing, 
- reflecting, 
- improving.

In AVC:
- Ceremonies create structured context that AI relies on
- Human stakeholders contribute before generation
- Plans are reviewed before code is generated

**This reduces the risk of misaligned AI outputs**.

#### 3. Promotes Validation and Adaptive Feedback

A one-prompt approach skips feedback loops and pushes the product directly into execution. Ceremonies, by contrast, embed inspection and adaptation — core principles of agile. They prevent brittle outputs by ensuring:
- sprint stays aligned with vision
- changes are reviewed
- quality gates are applied

> [!NOTE]
> This mirrors why ceremonies are used in traditional Agile to keep teams aligned.

#### 4. Prevents Drift and Technology Entropy

> ❗️ One of the biggest risks of AI-generated code is **architectural drift** — where the system’s structure slowly deviates from initial design due to uncoordinated prompts. 
> - AVC Ceremonies act as checkpoints to maintain consistency. 
> - In a one-prompt workflow, the AI cannot maintain system-wide context unless all context is rewritten into the prompt every time — costly and error-prone. 


### 🧠 When Sprint Planning Is Useful in AVC

In traditional Agile, Sprint Planning:
- Sets sprint goals
- Determines what can be delivered
- Breaks stories into tasks and estimates effort
- Creates a shared sprint backlog with commitment from the team

In AVC, the adapted Sprint Planning:
- Breaks large requirements into domain-specific units
- Populates context so AI agents can generate work items
- Produces refined work items with inherited context
- Ensures each generated artifact has a place in the larger plan

> [!CAUTION]
> ℹ️ So rather than planning human developer activity, this form of planning structures the input context that AI agents will use — a crucial distinction.

### 🤔 Is This Better Than One Prompt?

**Yes** — when:
- ✅ The software is non-trivial
- ✅ Multiple AI agents and humans must collaborate
- ✅ Architectural consistency matters
- ✅ Quality, security, and traceability are required
- ✅ The product is intended for more than short-lived prototypes

> [!NOTE]
> Ceremonies bring structure that helps scale beyond single developers and one-off code generation.

**No** — one prompt might be enough when:
- ❌ You are building tiny utilities
- ❌ No long-term maintenance is envisioned
- ❌ Scope is extremely limited and well understood
- ❌ There is no need to integrate with broader systems

> [!NOTE]
> In such cases, formal ceremonies may feel “heavy”.


### ⚖️ How the Ceremonial Approach Affects Software Production

<pre><code>
Aspect			  One-Prompt		Ceremonies (AVC) 
----------------------------------------------------------------
Scope definition	  Minimal		Explicit
Context propagation	  Manual, fragile	Systematic       
Alignment & vision	  Implicit		Explicit         
Quality checkpoints	  Ad hoc		Integrated       
Architectural governance  Difficult		Structured       
Team collaboration	  Weak or missing	Central         
Traceability		  Hard			Built-in         
</code></pre>    


### 🧩 Final Assessment

The ceremonies approach:
- Leads to better alignment — because planning and decomposition occur before generation. ✔ 
- Improves quality and accountability — because outputs follow planning, review, and context. ✔ 
- Scales to real production applications — where multiple agents, constraints, and stakeholders are involved. ✔ 

> [!NOTE]
> Compared to a single-prompt approach, ceremonies are inherently more structured, predictable, and robust — especially for complex products or long-lived systems.

## Summary Statement

> [!NOTE]
> A single prompt is a narrow tactic for rapid generation. 

 
> [!IMPORTANT]
> **Ceremonies are a strategic framework enabling AI-assisted development to operate with the discipline, inspection, adaptation, and team alignment that good engineering requires.**

#### 🔄 This mirrors why agile teams use **ceremonies** like sprint planning in traditional software development — they add cadence, clarity, and continuous improvement — not merely to generate work, but to ensure delivery quality and team cohesion.

[Agile Vibe Coding Manifesto](https://agilevibecoding.org/)

![AVC developer](./../Images/vibe_coding_2.png)
