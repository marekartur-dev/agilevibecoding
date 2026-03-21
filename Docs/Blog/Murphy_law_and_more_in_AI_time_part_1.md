# Murphy’s law and more in AI time - one by one with examples


<img src="./../Images/Whimsical software development machine in action.png" alt="Rune Goldberg machine and AVC" height="400"  style="float: right; margin-left: 10px;">

> Let me discuss a select set of the most relevant “laws” from this list, focusing on today’s software engineering, CI/CD, cloud, distributed systems, and AI-assisted coding.
> 
> I think that anyone who has been programming for a while will be able to confirm the truth of these laws also using their own practical examples.

## 1. Murphy’s Law

> “Anything that can go wrong will go wrong.”

### Why it’s more relevant now

With microservices, cloud infra, and AI-generated code, the number of failure points has exploded.

### Example
- AI generates a “working” function.
- It passes unit tests.
- In production:
  - edge-case input → crash
  - retry logic → duplicate messages
  - distributed system → cascading failure

### Modern version:
> [!IMPORTANT]
> 👉 **“Anything that can go wrong will go wrong — at scale and in parallel.”**

## 2. Hofstadter’s Law

> “It always takes longer than you expect, even when you take into account Hofstadter’s Law.”

### Why it hits harder today

AI makes things look faster—but integration, debugging, and correctness still dominate.

### Example
- AI scaffolds a feature in 2 hours
- You think: “Great, done by tomorrow”
- Reality:
  - unclear edge cases
  - inconsistent APIs
  - flaky tests
  - deployment quirks
  - **→ Takes 3 extra days**

### AI twist:
> [!IMPORTANT]
> 👉 **AI compresses coding time, not thinking time.**

## 3. Brooks’s Law

> “Adding manpower to a late software project makes it later.”

### Modern extension: adding AI

AI acts like a junior developer that:
- writes fast
- but needs review, correction, and integration

### Example
- Team behind schedule
- Adds:
  - more devs
  - more AI-generated code

Result:
- more inconsistencies
- more integration bugs
- more cognitive overhead

> [!IMPORTANT]
> 👉 **“Adding contributors (human or AI) increases coordination cost.”**

## 4. Conway’s Law

> “Organizations design systems that mirror their communication structure.”

### Still extremely accurate, which is why I have already devoted several articles to it.

Even with AI, architecture reflects:
- team boundaries
- ownership
- communication gaps

### Example
Separate teams for:
- payments
- notifications
- user profiles

→ You get:
- 3 microservices
- awkward APIs
- duplicated logic

> [!IMPORTANT]
> 👉 **AI doesn’t fix this — it often reinforces it by generating code per team context.** 
>    - Agile design and communication must change with it.
>    - One of the tips on how to do this is [Agile Vibe Coding](https://www.linkedin.com/pulse/agile-vibe-coding-conways-law-marek-kubis-m0wpe/?trackingId=wNYc5fRxyx3oQGxE3KYx8Q%3D%3D).
>    - If the suggestion does not satisfy you, look for another one.

## 5. Parkinson’s Law

> “Work expands to fill the time available.”

### Modern version with AI

> AI reduces effort → teams increase scope.

### Example
- Task estimated: 2 weeks
- AI helps finish core in 3 days
- What happens?
  - more features added
  - more “nice-to-haves”
  - more refactoring
  - **→ Still takes 2 weeks**

> [!IMPORTANT]
> 👉 Efficiency gains often become scope creep.

## 6. Peter Principle

> “People rise to their level of incompetence.”

### In software/AI context

Developers may:
- rely on AI beyond their understanding
- operate at abstraction levels they can’t debug

### Example
Developer uses AI to build:
- distributed caching
- async pipelines
But doesn’t understand:
- race conditions
- consistency models

> [!IMPORTANT]
> 👉 System fails in production, no one can fix it quickly.

## 7. Goodhart’s Law

> “When a measure becomes a target, it ceases to be a good measure.”

**Extremely relevant with metrics-driven engineering**

### Example

Team KPI:
- “Increase code coverage to 90%”

Result:
- meaningless tests
- mocked everything
- zero real reliability improvement

### AI twist:
- **AI can generate tests that game the metric**

> [!IMPORTANT]
> 👉 Metrics become easier to fake.

## 8. Hanlon’s Razor

> “Never attribute to malice that which is adequately explained by stupidity.”

### Updated for AI

Replace “stupidity” with:
- misunderstanding
- bad prompts
- poor assumptions

### Example
- AI generates insecure SQL query
- Not malicious
- → **just pattern completion without context**

> [!IMPORTANT]
> 👉 Most bugs are still accidental, even when AI is involved.

## 9. Occam’s Razor

> “The simplest explanation is usually the correct one.”

### In debugging

Still gold.

### Example

System failure:
- suspected: distributed cache inconsistency
- actual cause: environment variable typo

### AI danger:
- AI often suggests complex explanations first

> [!IMPORTANT]
> 👉 Simplicity is still your best debugging tool.

## 10. Chesterton’s Fence

> “Don’t remove something until you understand why it exists.”

> [!CRITICAL]
> Critical in legacy + AI refactoring (migration projects!)

### Example

AI suggests:
- removing “redundant” retry logic

But:
- it protects against a known external API bug
**→ Production outage**

> [!IMPORTANT]
> 👉 **AI is especially prone to violating this law.**

## 11. Law of Leaky Abstractions (Joel Spolsky)

> “All non-trivial abstractions leak.”

> [!IMPORTANT]
> **Even worse today**

We stack abstractions:
- cloud
- containers
- frameworks
- AI-generated code

### Example

You use:
- ORM
- async framework
- message queue

Everything works… until:
- performance drops
- you must understand SQL, threads, and networking anyway

> [!IMPORTANT]
> 👉 AI increases abstraction layers → more leaks.

## 12. Pareto Principle (80/20 Rule)

> “80% of effects come from 20% of causes.”

### Still true in bugs and effort

### Example
- 80% of outages come from:
  - 20% of services
  - or a single integration point

> [!IMPORTANT]
> 👉 AI doesn’t change this—it may even:
>    - concentrate risk in generated boilerplate patterns

## 13. Cunningham’s Law

> “The best way to get the right answer is to post the wrong one.”

### Modern version: AI prompting

### Example
- Ask AI: vague question → mediocre answer
- Provide wrong code → AI corrects it precisely

> [!IMPORTANT]
> 👉 Iteration beats initial prompting.

## 14. Zawinski’s Law

> “Every program attempts to expand until it can read email.”

### Today:

> [!IMPORTANT]
> 👉 “Every system expands until it becomes a platform.”

### Example

Your simple service becomes:
- API
- dashboard
- analytics
- notifications
- AI integration
**→ Complexity explosion**

## 15. Postel’s Law

> “Be conservative in what you send, liberal in what you accept.”

### Under pressure today

Modern systems often:
- strictly validate everything
- fail fast

### Example

Strict API:
- rejects slightly malformed requests
  → breaks backward compatibility

> [!IMPORTANT]
> 👉 Many modern systems violate this—and suffer for it.

## 16. Linus’s Law

> “Given enough eyeballs, all bugs are shallow.”

### Challenged by AI

AI creates:
- more code
- faster than humans can review

### Example
- Large AI-generated PR
- superficially reviewed
- **→ hidden bug survives**

> [!IMPORTANT]
> 👉 More code ≠ more understanding.


## Key Takeaways
What’s changed with AI
- Speed ↑
- Volume ↑
- Surface area ↑

What hasn’t changed
- Complexity
- Human understanding bottleneck
- Need for good architecture
- The “Meta-Law” of Modern Software

If we combine all of these:

> [!IMPORTANT]
> 👉 “AI accelerates code creation, but not correctness, understanding, or responsibility.”




### See also:
- [Agile Vibe Coding Manifesto](https://agilevibecoding.org/)
- [The Agile Vibe Coding and Conway's Law](https://www.linkedin.com/pulse/agile-vibe-coding-conways-law-marek-kubis-m0wpe/?trackingId=wNYc5fRxyx3oQGxE3KYx8Q%3D%3D)
- [Using a digital banking solution to prove Conway’s Law in AI-Driven engineering - example 1](https://www.linkedin.com/pulse/using-digital-banking-solution-prove-conways-law-ai-driven-kubis-xqlre/)
- [Using a .NET 10 migration project to prove Conway’s Law in AI-Driven engineering - example 2](https://www.linkedin.com/pulse/using-net-10-migration-project-prove-conways-law-ai-driven-kubis-abqae/?trackingId=%2FAxxEBhQ3Kz4dbLMKDokgg%3D%3D)
