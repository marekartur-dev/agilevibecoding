# Principles Behind the Agile Vibe Coding Manifesto - Discussion

> A while ago, Nacho Coll and I met to discuss the Agile Vibe Coding Manifesto and principles. [Here's what we agreed on.](https://agilevibecoding.org/)

<img src="./../Images/Kermit_Piggy_Muppet.png" alt="Agile and Agile Vibe Coding in practise" width="600"  style="float: right; margin-left: 10px;">

We didn't argue because we were, and are, in agreement about what's important in software engineering, including AI-based engineering.

However, the topic that raised the most doubts for us, and which is perhaps the quintessence of any software development cycle, was the principles.

We agreed that the approach of the creators of the original Agile Manifesto, focusing on values ​​and not overloading them, was very good.

Ultimately, we retained the 12 Agile principles, focusing on adapting them to the new reality of AI-enhanced development.

At the same time, we agreed that over time, as a broader group of developers gains more experience with the practical application of AI, 
we would revisit the topic and consider updating the proposed principles.

To further clarify the discussion, I proposed that each principle should have an "extended version" that could encourage 
a broader group of IT professionals to share their experiences.

I encourage you to critically reflect on each of the Agile principles, the SDLC cycle, software engineering practices, 
software project management, and how all of these elements interact in the context of using artificial intelligence (AI).

Therefore, I am presenting a set of specific guidelines, because my experience with discussing abstract topics 
like "let's improve Agile" has shown that they always end up in generalities.

Below is some text that can be critically analysed and I hope it will inspire further, broader discussion.

In the meantime, it is important to gather current observations, which is why this document was created.

> AGILE VIBE CODING MANIFESTO PRINCIPLES - EXTENDED VERSION

# I. PURPOSE 

## 1. Customer Value Is the North Star

Applies to strategy, prioritisation, and investment decisions.  
All acceleration must translate into validated customer value. Speed without validation is waste.

## 2. Humans Own Outcomes

Applies to production responsibility and risk ownership.  
AI assists, but humans remain accountable for correctness, security, compliance, ethics, cost, and long-term maintainability. Accountability must be formalised through structured review and verification mechanisms.

## 3. Context Is a First-Class, Versioned Artifact

Applies to requirements, prompts, architecture, and domain language.  
Shared context must be externalised, versioned, and traceable. AI systems degrade when context is implicit.

## 4. Uncertainty Is Not a Flaw of the System — It Is the System

Applies to leadership mindset and delivery expectations.  
Complex work, evolving technology, shifting requirements, and human variability are constants.  
The objective is not to eliminate uncertainty, but to navigate it intelligently.

## 5. Learning Velocity Over Planning Certainty

Applies to roadmap governance and executive oversight.  
Measure how quickly the organisation discovers what is true and adapts.  
Responsiveness is a competitive advantage; predictive precision is not.

# II. EXECUTION

## 6. Small, Verifiable Increments

Applies to delivery structure and risk control.  
Work is divided into independently testable, reviewable, and deployable slices.  
Granularity enables learning and limits blast radius.

## 7. Architecture Constrains and Guides Generation

Applies to system design and AI-assisted coding.  
Clear architectural patterns (state machines, event-driven boundaries, layered design) reduce drift and prevent structural entropy.

## 8. Design for Controlled Regeneration

Applies to component boundaries and modularity.  
Systems must allow safe regeneration of components without destabilising the whole.  
Regeneration power requires explicit contracts, stable interfaces, and governance.

## 9. Simplicity Enables Regenerability

Applies to maintainability and AI collaboration.  
Modular, minimal designs reduce cognitive load, testing complexity, and regeneration risk.  
Complexity compounds AI errors.

## 10. Protect Responsiveness from Control Traps

Applies to process design and organisational behaviour.  
Governance must increase learning speed — not simulate certainty.  
When Agile is repurposed into a certainty machine, feedback slows, honesty declines, and adaptability collapses.

# III. VALIDATION

## 11. Continuous Validation Over Blind Trust

Applies to CI/CD, ML evaluation, and runtime systems.  
AI output is assumed fallible. Validation must be automated, adversarial, and continuous. Silent degradation is expected and must be monitored.

## 12. Observability by Design

Applies to metrics, telemetry, cost monitoring, and SLAs.  
Success criteria, failure thresholds, latency limits, and cost ceilings are defined before review.  
Decisions are made against measurable signals — not impressions.

## 13. Mandatory Review of All Generated Artifacts

Applies to code, models, prompts, and infrastructure.  
No artifact enters production without checklist-driven review, traceable origin, and verification of security, data integrity, and rollback readiness.

## 14. Blind Dataset and Cross-Domain Testing

Applies to statistical validation and ML systems.  
Evaluation must use held-out, adversarial, and cross-domain datasets.  
Training-like examples create false confidence.

## 15. Independent Re-Generation for High-Risk Systems

Applies to critical implementations and architectural decisions.  
Alternative implementations or analyses are generated independently.  
Discrepancies reveal hidden assumptions.

## 16. Inverted Prompting and Red-Team Simulation

Applies to resilience and adversarial robustness.  
Systems must be stress-tested through deliberate failure simulation, injection attempts, dependency disruption, and AI-assisted flaw discovery.

# IV. GOVERNANCE

## 17. Security and Governance by Default

Applies to infrastructure, data handling, and compliance.  
Constraints are encoded into prompts, pipelines, and policies. Governance must be auditable and enforceable — not advisory.

## 18. Guardrails for Critical Boundaries

Applies to core architecture, shared contracts, and security baselines.  
High-impact components require stricter review, limited regeneration rights, and elevated change controls.

## 19. Reproducibility Over Environmental Drift

Applies to infrastructure and deployment environments.  
All environments must be reproducible from versioned definitions. Parity is enforced, not assumed.

## 20. Architectural Drift Detection

Applies to schemas, contracts, and infrastructure definitions.  
Automated checks must detect unintended deviations before they accumulate into systemic fragility.

## 21. Economic Responsibility

Applies to cloud architecture, model usage, and scaling decisions.  
Innovation must be cost-aware. Compute, retraining cadence, and storage growth require governance.

## 22. Organizational Architecture Mirrors System Architecture

Team communication structures must align with architectural boundaries.
Platform teams provide shared capabilities; business teams own domain services.
AI generation must operate within these boundaries.

# V. HUMAN SYSTEM

# 23. Dual-Mode Review

Applies to peer review structure.  
Functional correctness and adversarial analysis must be separated.  
No author reviews their own critical work in isolation.

## 24. Sustainable Pace for Humans and Systems

Applies to cognitive load, iteration cadence, and operational scaling.  
Prevent burnout, prompt churn, compute excess, and architectural entropy.  
Sustainability is both human and technical.

## 25. Human–AI Retrospection

Applies to continuous improvement.  
Regularly assess collaboration quality, review discipline, and bias exposure (automation bias, plausibility bias, anchoring, authority bias).  
Confidence is not evidence.

## 26. Organisational Responsiveness Is the Ultimate Metric

Applies to leadership evaluation and cultural health.  
The system succeeds when it can notice change early and adapt without drama.  
Responsiveness — not certainty — is the enduring advantage.







## See also:
- [Agile Vibe Coding Manifesto](https://agilevibecoding.org/)
- [Principles Behind the Agile Vibe Coding Manifesto - extended version](https://github.com/marekartur-dev/agilevibecoding/blob/main/Docs/Home/Principles.md)