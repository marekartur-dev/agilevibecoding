# Why containers, Docker and Kubernetes are a bad idea? - Part 5: Practical Engineering and Architecture Decision Framework 

<img src="./../Images/Our_dress_code.png" alt="Containers & Kubernetes — An Engineering Perspective" width="400"  style="float: right; margin-left: 10px;">

_In Parts 1, 2, 3, and 4, we covered the operational and business aspects of using containers, Docker, and Kubernetes._
_In Part 5, we'll discuss how to recognise when Kubernetes makes sense from a software engineering, metrics, and architecture perspective._

## Kubernetes as an operational leverage system

Most discussions about Kubernetes fail because they stay qualitative:
- “scalable”
- “cloud-native”
- “enterprise-grade”
- “future-proof”.

As practitioners, programmers should base architectural decisions on specifics and use quantitative analysis..

> The uncomfortable reality is:
>
> **for the overwhelming majority of business applications, Kubernetes is economically irrational.**

Not technically impossible. Economically irrational.

The key is understanding:
- operational cost,
- organisational maturity,
- scaling characteristics,
- failure cost,
- engineering throughput.

If the operational leverage gained is smaller than the operational complexity introduced, Kubernetes becomes negative ROI.

So, let's start discussion from constraints instead of tools.

> [!IMPORTANT]
> The real questions are:
> - “At what organisational scale does orchestration complexity become cheaper than human coordination complexity?”
> and
> - “What operational and business problem are we trying to solve, and what complexity budget do we actually have?”

that crossover point is where Kubernetes begins to pay off.

Our current critique is based on the observation that many organisations have adopted FAANG infrastructure patterns without addressing the problems Kubernetes was designed to solve. Let's explore this.

### How do we identify and define FAANG issues?

> “FAANG issues” are problems that primarily exist at hyperscale but are often prematurely imported into ordinary systems.

Typical characteristics:
```
Characteristic      Real FAANG-scale problem  Typical SME reality
------------------------------------------------------------------
Traffic               Millions/billions      Thousands/minutes
                      of requests 
Deployment frequency  Hundreds/day           Weekly/monthly
Team count            Hundreds of engineers  3–20 developers
Availability target   Multi-region           “Don’t crash 
                      99.99–99.999%          during office hours”
Infrastructure scale  Thousands of nodes     1–20 servers
Failure assumptions   Constant partial       Occasional outages
                      failure
Data scale            Petabytes              GB/TB
Cost of downtime      Millions/hour          Manageable 
                                             business
											 interruption
```

Examples of FAANG issues:
- Multi-region active-active routing
- Massive horizontal autoscaling
- Service mesh complexity
- Distributed tracing across hundreds of services
- Cross-region consensus
- Multi-cluster orchestration
- Sophisticated chaos engineering
- Per-service deployment independence at large scale

A useful heuristic:
- If our system can comfortably run on one reasonably powerful machine, we probably do not yet have FAANG problems.
- If operational complexity grows faster than business value, we are importing FAANG architecture into non-FAANG reality.

**This is extremely common with premature Kubernetes adoption.**

### How do we determine when containers, Docker, and Kubernetes are a good application architecture?

> [!NOTE]
> These technologies solve real problems. The key is understanding ___which___ problems.

_Docker is usually justified earlier than Kubernetes._

Docker provides:
- Environment consistency
- Dependency isolation
- Easier onboarding
- Simplified CI/CD
- Packaging standardisation

**For many teams Docker alone is sufficient.**

We can run:
- Docker Compose
- Single VM deployments
- Small orchestration setups
- Managed container services

without introducing Kubernetes.

> Kubernetes becomes valuable when operational coordination complexity begins to dominate system development and deployment.

Kubernetes is justified when we genuinely need:

**A) Independent scaling**

❗️ Different subsystems scale differently.

Example:
- API: moderate traffic
- Background processing: huge spikes
- AI pipeline: GPU-heavy

**B) Frequent deployments**

❗️ Many teams deploying independently.

**C) Multi-team ownership**

❗️ Clear service boundaries with independent release cycles.

**D) High resilience requirements**

❗️ We require:
- self-healing
- automated failover
- rolling upgrades
- high availability

**E) Infrastructure abstraction**

❗️ We need:
- cloud portability
- declarative infrastructure
- dynamic scheduling

**F) Operational maturity**

❗️ We already have:
- DevOps/SRE capability
- Observability
- CI/CD maturity
- Incident management
- Infrastructure governance

> 👉 Without operational maturity **Kubernetes amplifies organisational chaos**.


### How do we evaluate the overhead associated with templates versus code development?

Templates and infrastructure abstractions create hidden cognitive cost.

This includes:
- Helm charts
- YAML
- Bicep/Terraform
- GitHub Actions/Azure Pipelines
- Operators
- CRDs
- Service meshes

The key trade-off:
```
Approach	Advantage               Cost
-----------	-----------------------	----------------------
Direct code	Explicit, debuggable	Less reusable
Templates	Standardisation         Abstraction complexity
Platforms	Massive reuse           Heavy operational burden
```

Evaluate overhead in four dimensions

	1. Cognitive overhead

	👉 How difficult is it to understand the deployment?

	Example:
	- 200-line Dockerfile → understandable
	- 40 Helm templates + overlays → difficult

	2. Debugging overhead

	👉 Can developers reason about failures?

> [!NOTE]
> ❌ **A common anti-pattern:** 
> 
> YAML generates YAML that configures YAML

	3. Change amplification

	👉 How many places must change for a simple modification?

	4. Expertise concentration risk

	👉 Does only one “Kubernetes wizard” understand the system?

	**If yes, operational fragility exists**.

> [!NOTE]
> ✔️ **A practical rule: **
> 
> If infrastructure abstraction becomes harder to understand than the business application itself, abstraction has gone too far.


### What is the threshold above which operational overhead becomes worthwhile?

There is no universal threshold, but there are practical signals.

**Operational complexity becomes worthwhile when:**
```
Signal                            Meaning
--------------------------------  --------------------------------------
Deployments are bottlenecked      Monolith slowing teams
Teams block each other            Need release independence
Scaling characteristics diverge	  Need isolated scaling
Downtime cost is high             Need resilience
Infrastructure changes frequently Need orchestration
Manual operations dominate        Automation justified
```

**Warning signs complexity is NOT justified:**

	1. Small team

	👉 2–10 developers.

	2. Low deployment frequency

	👉 Weekly or monthly releases.

	3. Single business domain

	Tightly coupled workflows.

	4. Modest traffic

	👉 Single-server capability sufficient.

	5. Weak observability

	👉 No logs/metrics/traces culture.

	6. Lack of operational ownership

	👉 Developers throw systems “over the wall”.

> [!NOTE]
> ✔️ One of the best heuristics:
> 
> **If Kubernetes requires hiring specialists before delivering business value, adoption is probably premature.**

### What type of project qualifies as a containerised application?

> ✔️ Containers are packaging technology, not architecture.

> ✔️ Almost any application can be containerised.

> [!NOTE]
> The real question is:
> 
> Does containerisation reduce operational friction?

**Good candidates**

	- Web APIs 
	  - excellent fit
	  - stateless services are ideal

	- Background workers 
	  - very good fit
	  - queue consumers, schedulers, event processors

	- Batch processing
	  - good fit for ephemeral workloads

	- AI/ML pipelines
	  - often strong candidates because of dependency isolation

	- Integration-heavy systems
	  - containers help standardise environments

**More difficult candidates**

	- Stateful applications
	  - Databases, stateful clusters
	  - Possible, but operationally harder

	- Desktop applications
	  - Usually poor fit unless remote-hosted

	- Low-resource edge systems
	  - Container overhead may not justify value

	- Monolithic business systems
	  - Sometimes containerisation helps deployment without requiring microservices

> ✔️ A monolith inside a container is still a perfectly valid architecture.

> ✔️ **Containerisation does not require microservices.**


### What should we consider when deciding whether an application should be a distributed platform application?

Distributed systems introduce fundamental complexity:
- Partial failure
- Network latency
- Retries
- Eventual consistency
- Observability challenges
- Transaction boundaries
- Versioning problems
- Message ordering
- Data duplication
- Operational complexity

> [!WARNING]
> 👉 The key question:
> 
> “Is distribution solving a real business scaling problem?”

**Good reasons for distribution:**

	- Independent scaling
	  ✔️ Some workloads scale differently.

	- Team autonomy
	  ✔️ Large organisations.

	- Fault isolation
	  ✔️ Failures must remain isolated.

	- Geographic distribution
	  ✔️ Multi-region requirements.

	- Technology diversity
	  ✔️ Different runtimes genuinely needed.

	- Event-driven business workflows
	  ✔️ Asynchronous domains.

**Bad reasons for distribution:**

	- “Modern architecture”
	✔️ Trend-driven engineering.

	- Resume-driven development
	✔️ Engineers optimising CVs.

	- Premature optimisation
	  ✔️ Preparing for scale that may never arrive.

	- Misunderstanding modularity
	  ✔️ A modular monolith is often enough.


## Architecture decisions

### First Principle

There is no direct relationship between
- number of users and
- need for Kubernetes.

> [!NOTE]
> 👉 A badly designed system with 5,000 users may need orchestration earlier than a well-designed monolith serving 5 million users.

Examples:
- Stack Overflow historically handled enormous traffic with relatively simple architecture.
- Many enterprise Kubernetes platforms barely serve internal HR portals.

So the real variables are:
```
Variable                    Importance
-------------------------   ----------
Deployment frequency        Very high
Operational complexity      Very high
Scaling asymmetry           Very high
Team count                  High
Failure isolation needs     High
Traffic volume              Moderate
CPU usage                   Moderate
Database TPS                Moderate
User count                  Low
```

### The Wrong Trigger

> [!WARNING]
> Many teams think: “CPU is high, therefore we need Kubernetes”. 
> **❌ Usually, that assumption is wrong**.

High CPU often means:
- poor SQL,
- missing cache,
- inefficient serialization,
- N+1 queries,
- synchronous I/O,
- memory leaks,
- oversized containers,
- poor batching,
- chatty APIs.

> [!IMPORTANT]
> **✔️ Kubernetes does not solve inefficient software.**

Kubernetes merely automates deployment of inefficient software.

### What Actually Justifies Kubernetes?

> [!NOTE]
> 👉 As it was already mentioned, Kubernetes becomes economically reasonable when 
> **operational automation saves more money than the platform costs**.

_This is the REAL threshold._

The platform itself introduces:
- platform engineers,
- observability tooling,
- CI/CD complexity,
- cluster upgrades,
- YAML maintenance,
- security management,
- networking complexity,
- ingress management,
- storage orchestration,
- RBAC governance.

> [!WARNING]
> **📌 That operational tax is substantial.**

### Rough Economic Thresholds

**Usually not justified**

These ranges are illustrative heuristics, not industry standards. Typical profile:
```
Metric                      Typical Range
-------------------------   -------------
Team size                   1–10 developers
Deployments                 < daily
Services                    < 5
Requests/sec                < 100–300 RPS
Infrastructure nodes	    < 5
Traffic pattern	            Predictable
Availability target	        99.5–99.9%
Revenue impact of downtime	Moderate
Ops staff                   None/dedicated absent
```

This is the territory where:
- Docker Compose,
- VM deployments,
- App Services,
- ECS/Fargate,
- Azure Container Apps,
- simple orchestration

are usually superior.

**Borderline zone**
```
Metric                       Typical Range
--------------------------   -------------
Team size                    10–30
Deployments                  Daily/multiple daily
Services                     5–20
RPS                          300–3000
Background workloads         Significant
Scaling asymmetry            Emerging
Multi-environment complexity Increasing
Availability needs           High
Release coordination pain    Significant
```

> [!NOTE]
> 📌 This is where Kubernetes may begin making sense.  
> But **organisational maturity matters more than traffic**.

**Strong Kubernetes candidate**
```
Metric                      Typical Range
-------------------------   -------------
Teams                       30+ engineers
Independent services        20–100+
Frequent releases           Continuous
Multiple scaling profiles   Strong
Multi-region                Yes
High availability           Critical
Downtime cost               Very high
Infrastructure automation   Mature
SRE/Platform team           Exists
```

> [!NOTE]
> ✔️ At this point: **orchestration complexity starts paying for itself**.


## The Most Important metrics indicate that Kubernetes may become useful

### Category 1 — Deployment Pain: the FIRST real indicator

**Indicators**:

- Release coordination failures: Teams block each other.
	Threshold:
	- deployments require synchronized downtime,
	- high merge conflicts,
	- rollback complexity increasing.

- Long deployment times.
	Threshold:
	- deployment > 20–30 minutes,
	- frequent rollback failures.

- Environment inconsistency
	Threshold:
	- “works on my machine” recurring,
	- configuration drift frequent.

**📌 Docker often solves this BEFORE Kubernetes is needed.**

### Category 2 — Scaling Asymmetry: Critical signal

	Example:
	- API uses 2 CPUs
	- Background image processing spikes to 64 CPUs

	**📌 This strongly favours orchestration.**

**Indicators**:
- CPU variability
	Threshold: 
	- Not average CPU.
	Instead:
	- unpredictable spikes,
	- uneven subsystem scaling.

- Memory fragmentation
	Threshold:
	- Some services need 512 MB, others 32 GB.
	- This benefits scheduling/orchestration.

### Category 3 — Operational Instability

**Indicators**:
- Manual recovery frequency
	Threshold:
	- humans restart services regularly.

- Recovery time
	Threshold:
	- MTTR too high,

- Cascading failures.
	Threshold:
	- Strong distributed orchestration signal.

### Category 4 — Team Topology

> Conway’s Law matters enormously.

**Indicators**:
- Multiple teams deploy independently
	Threshold:
	- coordination overhead dominates development.

- Service ownership boundaries emerge
	Threshold:
	- Strong orchestration indicator.

### Category 5 — Infrastructure Utilisation

_This is where people over-focus._

Raw CPU is less important than:
- volatility,
- fragmentation,
- scheduling inefficiency.

**CPU Thresholds**

Bad threshold:
- “CPU > 70%”

Better analysis:
- Are workloads bursty?
- Are workloads heterogeneous?
- Is infrastructure underutilised due to static allocation?

**Example**

Without orchestration:
- 10 VMs each reserved for peak load,
- average utilisation 15%.

> 📌 Kubernetes may consolidate workloads efficiently.
> **That is where cost savings emerge.**

### Database Metrics

> Databases rarely justify Kubernetes directly.

Usually they justify:
- replication,
- partitioning,
- caching,
- read models,
- async processing.

**Important DB indicators**
- Connection pool exhaustion - **repeatedly**.
- Queue build-up - **background jobs lagging**.
- Lock contention - distributed workload pressure - **frequent**.
- Read/write asymmetry - **significant**. May justify workload separation.

**Networking Indicators**

_Often overlooked._

**Indicators**
- Service-to-service traffic exploding
	Threshold: **growing internal mesh complexity**.
- Retry storms
	Threshold: **classic distributed-system warning sign**.
- Latency amplification
	Threshold: **distributed call chains growing**.

### The Real Alarm Thresholds

> [!NOTE]
> 📌 Here is the uncomfortable truth:
>	**There are no universal numeric thresholds.**
> 
> 👉 The true thresholds are organisational.

**Kubernetes starts making sense when:**
```
Organisational Signal               Meaning
--------------------------------	----------------------------
Platform team appears naturally     Infra complexity real
Developers blocked by deployments   Release orchestration needed
Services scale differently          Scheduler valuable
Downtime cost becomes severe        Automation justified
Infra utilization inefficient       Dynamic scheduling valuable
Teams require autonomy              Independent deployments matter
```


## Practical Decision Framework

### Stage 1 — Simple

Use:
- monolith,
- Docker,
- Compose,
- managed DB,
- CI/CD.

Typical scale:
- potentially up to millions of users, depending on architecture quality.

### Stage 2 — Growing

Add:
- background workers,
- caching,
- async processing,
- observability,
- horizontal scaling.

**👉 Still often no Kubernetes needed.**

### Stage 3 — Operational Complexity Emerges

Indicators:
- scaling asymmetry,
- deployment pain,
- operational coordination,
- workload fragmentation.

Now evaluate:
- ECS,
- Azure Container Apps,
- Nomad,
- Kubernetes.

### Stage 4 — Platform Scale

**👉 Now Kubernetes becomes rational.**

Not because CPU usage is high, but because **operational coordination complexity dominates**.


### Minimal Application Complexity

> [!IMPORTANT]
> 📌 Kubernetes helps when _infrastructure coordination complexity_ becomes larger than _application business complexity_.

### Example 1 — Simple Business System

**Architecture**
- Web API
- SQL database
- Payment provider
- Admin UI
- 5k users
- 20 RPS peak

👉 Kubernetes is almost certainly overkill. 
A VM/App Service can easily handle this.

### Example 2 — Growing SaaS

**Architecture**
- API gateway
- Authentication
- Payments
- Notifications
- Search
- Background jobs
- AI processing
- Multi-tenant workloads
- 100k users
- 1000+ RPS
- Continuous deployment

👉 Now:
- workload isolation,
- autoscaling,
- deployment orchestration,
- service discovery

may justify Kubernetes. ✔️



## Final Principle

_This point is worth repeating._

The best Kubernetes indicator is usually NOT:
- CPU,
- TPS,
- memory,
- user count.

> 📌 The best Kubernetes indicator is:  
> **“Are humans becoming the scalability bottleneck of the system?”**

> [!IMPORTANT]
> 👉 When operational coordination costs become larger than the cost of orchestration infrastructure, Kubernetes starts becoming economically justified.

_.. tbc_..


## See also:
- [Why containers, Docker and Kubernetes are a bad idea? - Part 1: The core problem of architecture patterns](./Containers_K8s_Part_1.md)
- [Why containers, Docker and Kubernetes are a bad idea? - Part 2: When Containers and Kubernetes Become Architectural Debt](./Containers_K8s_Part_2.md)
- [Why containers, Docker and Kubernetes are a bad idea? - Part 3: The strangest outcomes of modern infrastructure engineering](./Containers_K8s_Part_3.md)
- [Why containers, Docker and Kubernetes are a bad idea? - Part 4: A Practical Small-Team Architecture](./Containers_K8s_Part_4.md)
- [Why containers, Docker and Kubernetes are a bad idea? - Part 6: Kubernetes Costs and When Kubernetes Is Justified](./Containers_K8s_Part_6.md)

- [Is there a need to change the way software is developed today?](https://www.linkedin.com/pulse/need-change-way-software-developed-today-marek-kubis-dntie)
- [Is there a need to change the way software is developed today? - Continuation](https://www.linkedin.com/pulse/need-change-way-software-developed-today-continuation-marek-kubis-uytye)
- [Deterministic Developers in a Non-Deterministic World](https://www.linkedin.com/pulse/deterministic-developers-non-deterministic-world-marek-kubis-fstte)
- [Down the rabbit holes of AI-based software development process ](https://www.linkedin.com/pulse/down-rabbit-holes-ai-based-software-development-process-marek-kubis-fsyue)
- [This Isn’t Rebranding. It’s a Structural Shift in Software Development](https://www.linkedin.com/pulse/isnt-rebranding-its-structural-shift-software-marek-kubis-sanpe)

- [Mutation testing - Part 1: is it outdated?](https://www.linkedin.com/pulse/mutation-testing-part-1-why-works-all-marek-kubis-rkdde/)
- [Mutation testing - Part 2: Turn into a production-ready tool](https://www.linkedin.com/pulse/mutation-testing-part-2-turn-production-ready-tool-marek-kubis-qymbe/)
- [Mutation testing - Part 3: Mutation testing limits and how to go beyond it](https://www.linkedin.com/pulse/mutation-testing-part-3-limits-how-go-beyond-marek-kubis-taeue/)
- [Mutation testing - Part 4: mutation testing and LLM-written code](https://www.linkedin.com/pulse/mutation-testing-part-4-llm-written-code-marek-kubis-pjpne/)

- [Kafka & Service Bus — Part 1: Two Philosophies of Event-Driven Systems](https://lnkd.in/eiE5dcVp)
- [Kafka & Service Bus — Part 2: In Business Solutions: Real-world Architectures](https://lnkd.in/eAg_R5SZ)
- [Kafka & Service Bus — Part 3: Technical Comparison](https://lnkd.in/eBKcczQF)

- [Murphy’s law and more in AI time - one by one with examples](https://www.linkedin.com/pulse/murphys-law-more-ai-time-one-examples-marek-kubis-fkaze)
- [The Agile Vibe Coding and Conway's Law](https://www.linkedin.com/pulse/agile-vibe-coding-conways-law-marek-kubis-m0wpe)
- [Using a digital banking solution to prove Conway’s Law in AI-Driven engineering - example 1](https://www.linkedin.com/pulse/using-digital-banking-solution-prove-conways-law-ai-driven-kubis-xqlre/)
- [Using a .NET 10 migration project to prove Conway’s Law in AI-Driven engineering - example 2](https://www.linkedin.com/pulse/using-net-10-migration-project-prove-conways-law-ai-driven-kubis-abqae)

- [Where traditional Agile breaks in AI-driven systems](https://www.linkedin.com/pulse/where-traditional-agile-breaks-ai-driven-systems-marek-kubis-4wq6e/)
- [AI - It seems nobody has it fully figured out yet](https://www.linkedin.com/pulse/ai-nobody-has-figured-out-marek-kubis-bkyge)
- [Internal Development Platform and Agile Vibe Coding](https://www.linkedin.com/pulse/internal-development-platform-agile-vibe-coding-marek-kubis-kyhqe/?trackingId=5w3lWKp%2F0BLUpwNdrSmAcg%3D%3D&lipi=urn%3Ali%3Apage%3Ad_flagship3_pulse_read%3BqH%2FwqbkZRkmo%2Fagtxvqyrw%3D%3D)
- [Everyone will be vibe coders](https://www.linkedin.com/pulse/everyone-vibe-coders-marek-kubis-tlgze)
- [The Structural problems AI introduces into the SDLC](https://www.linkedin.com/pulse/structural-problems-ai-introduces-sdlc-marek-kubis-qyt6e)
- [Signals That Reveal the True Maturity of Organisations Claiming “AI-Driven Development”](https://www.linkedin.com/pulse/signals-reveal-true-maturity-organisations-claiming-ai-driven-kubis-urule)
- [AI - It seems nobody has it fully figured out yet](https://www.linkedin.com/pulse/ai-nobody-has-figured-out-marek-kubis-bkyge)

- [Agile Vibe Coding positioning and if this works, what changes?](https://www.linkedin.com/pulse/agile-vibe-coding-positioning-works-what-changes-marek-kubis-r4ate)
- [Agile Vibe Coding – Ceremony Modes](https://www.linkedin.com/pulse/agile-vibe-coding-ceremony-modes-marek-kubis-meq9e)
- [Agile Vibe Coding ceremonies approach compared to a simple one-prompt-per-task approach](https://www.linkedin.com/pulse/agile-vibe-coding-ceremonies-approach-compared-simple-marek-kubis-ecx5e)
- [Agile Vibe Coding Maturity Model](https://www.linkedin.com/pulse/agile-vibe-coding-maturity-model-marek-kubis-bbtqe)
- [The Agile Vibe Coding - the 4-level adaptive ceremony system](https://www.linkedin.com/pulse/agile-vibe-coding-4-level-adaptive-ceremony-system-marek-kubis-jizke)

- [Agile Vibe Coding Manifesto](https://agilevibecoding.org/)
- [Principles Behind the Agile Vibe Coding Manifesto - extended version](https://github.com/marekartur-dev/agilevibecoding/blob/main/Docs/Home/Principles.md)

- [Agile Vibe Coding](https://www.reddit.com/r/AgileVibeCoding/)
- [Marek Kubis - blog](https://github.com/marekartur-dev/agilevibecoding/tree/main)