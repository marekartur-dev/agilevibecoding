# Deterministic Developers in a Non-Deterministic World
___Some Dilemmas of Deterministic Software Developers___

Lately, many of my posts have been full of lists, bullet points, and structured arguments. This time, I’d like to try something different: a story.

Someone might be tempted to comment “papa, don’t preach” and suggest saving reflections like these for the grandchildren. Perhaps. But my goal is not to lecture. My goal is to place an important discussion into the right context.

Today, conversations about artificial intelligence and software engineering are increasingly emotional, and unfortunately, they are often accompanied by misconceptions — even among experienced practitioners. So instead of presenting another dry list of pros and cons, I would like to tell a story about a dilemma many developers are starting to face.

## The Problem

> The rapid growth of interest in AI has led to an explosion of opinions online. Among them are many statements that are not entirely correct — sometimes even from experienced software engineers.

One of the most common claims is that AI introduces chaos into software engineering, which historically has relied on deterministic systems. But this claim does not entirely reflect reality. Let’s look at a few examples.

### Example 1 — Is Microservices Architecture Overrated?

A widely discussed case involved Amazon's Prime Video monitoring system. The architecture was redesigned from a distributed approach to a monolithic one, reportedly reducing costs by around 90%. The story was described in the article ___“Scaling Prime Video Audio/Video Monitoring Service and Reducing Costs by 90%”___.

At first glance, the conclusion seemed obvious: **microservices were the problem**.

However, a deeper analysis from the engineering community revealed something important.

The system relied heavily on AWS Step Functions and orchestration logic. In practice, the architecture **did not truly resemble microservices**. The issues were largely caused by design decisions and orchestration overhead rather than the microservices paradigm itself. In other words, the term ___microservices___ was applied to something that wasn’t actually a microservices architecture.

This example highlights a broader problem: **mislabeling design mistakes as architectural failures**.

### Example 2 — What Is the Purpose of Programming?

Recently I encountered a statement from an experienced developer claiming that the purpose of programming is to:
- organize our thinking
- communicate our understanding to others.

There is some truth in that. Writing code does indeed structure thinking and communicate ideas. However, these outcomes are by-products, not the primary goals of software engineering. The fundamental objectives of software development are different.

Among the most important are:
1. Solving real-world problems
2. Correctness — producing correct results for given inputs
3. Efficiency — using computational resources effectively
4. Reliability — functioning consistently without failure
5. Security — protecting systems and data
6. Usability — enabling users to interact effectively with software

Historically, we also emphasized:
7. Maintainability
8. Scalability
9. Reusability
10. Portability

But the rise of AI-assisted development forces us to reconsider some of these assumptions.

### A New Perspective

Consider maintainability.

> Traditionally we say:
> Code should be easy to understand, modify, and extend.

But this view implicitly assumes something deeper: **that the system is deterministic**.

**Why?**

Because reproducibility depends on determinism. Yet AI systems are inherently non-deterministic. Given the same prompt, they may generate different outputs. And increasingly, AI systems are involved in generating code itself.

So does this mean reproducibility disappears?
**Not necessarily**.

> The key insight is this:
> Determinism should apply to the result, not necessarily to the process that produced it.

The system generating the solution may be non-deterministic. But the final response must still be reliable and verifiable. This shift has important consequences.

Questions like these begin to sound different:
- Will AI break scalability?
- Will AI prevent reuse?
- Will AI destroy portability?

In reality, these concerns may be misplaced. AI changes how software is produced, not necessarily what properties the resulting system must guarantee.

### Making Incremental Progress

A useful principle in deterministic systems is separating:
- code that decides
- code that acts.

AI introduces additional non-determinism into the decision space.

> So perhaps the real question is no longer:
> ___What exact internal path produced the answer?___

Instead, the more relevant questions become:
- **Is the result correct?**
- **Is it trustworthy?**

This leads to a shift in objectives:
- Our goal is to **guarantee deterministic results**.
- Our goal is to **build trust in responses produced by non-deterministic systems**.

## A Brief Look at Software Engineering History

Many developers believe deterministic behavior is a fundamental property of software. But historically, software engineers have always faced similar trust problems.

Why do we trust traditional software? Because:
- we understand the architecture
- we know the processes
- we can inspect the source code

Most importantly, we trust software because we test it:
- unit tests
- integration tests
- system tests
- penetration tests.

So what prevents us from testing non-deterministic systems? In practice, **nothing**.

The difference is that traditional tests alone are not enough. New methods are required:
- functional evaluation
- statistical testing
- behavioral validation
- probabilistic verification

This is not a failure of engineering. It is **a new engineering challenge**.

### The Oldest Question in AI

Interestingly, this challenge is not new. Early artificial intelligence research — particularly in medical expert systems in the 1960s and 1970s — faced the same issue.

> The most difficult question asked of those systems was simple:
> **Why?**

Doctors asked systems questions like:
- Why do you believe the infection is meningitis?
- Why do you conclude the patient has chronic hepatitis?
- What should be examined next and why?

These systems attempted to justify their reasoning using:
- production rules
- decision trees
- logical inference
- knowledge bases

Many of these ideas later evolved into modern software constructs:
- classes
- interfaces
- event systems
- dependency injection
- heuristic models
- neural networks.

### Trust Without Full Understanding

But let us consider another scenario.

Suppose an AI system knows hundreds of proofs of the Pythagorean theorem — mathematicians have documented more than 350 different proofs across history. If the system solves a geometry problem using one of those proofs, does it matter which proof it used? In most cases, it does not.

What matters is that the result is **correct**.

Even if the system uses different reasoning each time, repeated correct results across varied inputs can build confidence in the outcome.

## Summary

Artificial intelligence challenges some long-held assumptions in software engineering.

But it does not eliminate engineering discipline.

Instead, it shifts our focus.

> Determinism does not need to be a property of the entire system.
> **Determinism should be a property of the result**.

Our new objectives therefore become:
- guaranteeing reliable outputs
- validating correctness statistically and functionally
- building trust in systems whose internal processes may be non-deterministic

This is not the end of software engineering. It is simply the next stage of its evolution.


[Agile Vibe Coding Manifesto](https://agilevibecoding.org/)