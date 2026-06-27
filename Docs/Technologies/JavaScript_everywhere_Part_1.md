# JavaScript Everywhere – Part 1: Applications for Highly Regulated Industries (e.g., for Lawyers)

<img src="./../Images/Stressed_coder_facing_legal_threat.png" alt="Stressed coder facing legal threat" width="800"  style="float: right; margin-left: 10px">

_In highly regulated industries, strict requirements apply not only to companies but also to computer applications._
_However, business logic can now be created using a wide range of programming languages—including those traditionally associated with the front-end, such as JavaScript._
_Is it possible, then, to ensure application compliance with GDPR, SOC2, SRA, and PCI standards if a loosely typed language - such as JavaScript - is chosen as the primary programming language?_
_I am not sure whether developers bother to address such a question, given that the problem to be solved is often a massive challenge in itself._

## By way of introduction

The governments of many countries, including the United Kingdom, have set - and continue to strive to set - standards ensuring the proper and ethical use of software and artificial intelligence [1][2][3][4].
Commenting on these standards, _Charlie Bell, EVP Security, Microsoft_, has said that 
"Microsoft runs on trust, and trust must be earned and maintained. Our pledge to our customers and our community is to prioritise your cyber safety above all else."

In the world of software development, this means that developers must adhere to best practices, 
write clean and maintainable code, and ensure transparency that their applications are secure and reliable. 
By doing so, they build a reputation for quality and integrity, which in turn fosters trust with users and clients.
Do you agree with this premise?

### Limitations

This article focuses on enterprise backend systems operating in regulated environments. 
It does not imply that JavaScript or Node.js are unsuitable for all applications. 
They remain excellent choices for APIs, real-time communication, serverless functions, developer tooling, 
build systems, and many cloud-native workloads where regulatory complexity is lower.

## Is this possible using JavaScript as the primary language for business logic in highly regulated industries? 

> [!IMPORTANT]
> 📌 So the answer is **yes**, a **JavaScript application can satisfy GDPR, SOC 2, SRA, and PCI requirements**, 
> but compliance comes from the architecture and controls around the application.

> [!NOTE]
> 👉  **GDPR, SOC 2, SRA, and PCI compliance are not properties of JavaScript, C#, Java, or any programming language.**
> 
> They are properties of the entire system, including architecture, processes, infrastructure, operations, security controls, auditing, deployment practices, and software design.

**Let’s find out.**

A legal, banking, or healthcare system can be compliant when written in JavaScript, and a C# system can be completely non-compliant.

> [!WARNING]
> ❗️ Server-side JavaScript (typically in a Node.js environment) is an excellent choice in certain areas. 
> It is a technology that performs exceptionally well when matched to the specific nature of a given problem, 
> yet becomes cumbersome for applications that fall outside that scope.

This raises another question:

> [!IMPORTANT]
> 📌 How difficult is maintaining compliance when using JavaScript, compared to other platforms?

### Compliance vs Programming Language

Consider this simple example.

**Non-compliant JavaScript**:

```javascript
app.post('/customer', async (req, res) => {

    console.log(req.body);

    await db.insert(req.body);
});
```

Issues:
- Logs PII
- No consent tracking
- No audit trail
- No encryption
- No access control

**Compliant JavaScript**:

```javascript
app.post('/customer', async (req, res) => {

    authorize(req.user);

    validateConsent(req.body);

    await auditLog.record({
        user: req.user.id,
        action: "CreateCustomer"
    });

    await customerRepository.save(encrypt(req.body));
});
```

> [!IMPORTANT]
> 📌 The difference is architecture and controls, not language.

### General Data Protection Regulation (GDPR)

Under GDPR, regulators care about:
- lawful basis
- consent
- right to erasure
- data minimisation
- retention policies
- auditability
- security controls

They do not care whether the code is:
- JavaScript
- C#
- Java
- Go

For example, a _JavaScript_ system can implement (marked as PII):

```
    Data Classification
    Customer Name
    Customer Address
    Email
    Phone
```

**Encryption** 
- AES-256
- Key Vault
- HSM

**Audit Logging**
```
{
  "user":"lawyer123",
  "action":"ViewCase",
  "timestamp":"..."
}
```

**Retention**
- Delete after 7 years

> [!NOTE]
> ✔  All perfectly achievable in Node.js.

### Systems and Organization Controls (SOC 2)

___SOC 2___ focuses on controls such as:
- Security
- Availability
- Confidentiality
- Processing Integrity
- Privacy

Auditors typically ask:
- Who changed production?
- How are secrets stored?
- How are deployments approved?
- How are incidents managed?

> [!NOTE]
> ✔  These are mostly operational concerns.

A _JavaScript_ application deployed on:
- Kubernetes
- Azure
- AWS

can absolutely satisfy ___SOC 2___.

> [!IMPORTANT]
> 📌 The language is largely irrelevant.

### Solicitors Regulation Authority (SRA)

Legal applications have additional requirements:
- Client confidentiality
- Access controls
- Auditability
- Conflict checking
- Data protection

> [!WARNING]
> ❗️ The biggest challenge is not JavaScript itself.

> [!IMPORTANT]
> 📌 The challenge is ensuring that ___Lawyer A cannot see Lawyer B's client data___ **and proving it!**

> [!NOTE]
> ✔  That is an authorization problem, not a JavaScript problem.

### Payment Card Industry Data Security Standard (PCI DSS)

> [!IMPORTANT]
> 📌 _PCI DSS_ is where architecture matters significantly.

_PCI DSS_ requires:
- Encryption
- Segmentation
- Logging
- Monitoring
- Vulnerability management
- Access controls

> [!NOTE]
> ✔  `JavaScript/Node.js` applications can absolutely be _PCI DSS_ compliant.

Many payment providers use Node.js.

Nevertheless, _PCI DSS_ environments often benefit from stricter governance because:
- _npm_ supply-chain risks
- dependency vulnerabilities
- frequent package updates

**must be managed carefully**.


## Where JavaScript is a good backend choice

### 1. I/O-heavy, real-time systems

JavaScript shines when the backend is mostly waiting on things:
- APIs calling other APIs
- database queries
- message queues
- file/network I/O

Typical examples:
- Real-time chat applications
- Collaboration tools (Google Docs-like features)
- Live dashboards
- Notification systems (WebSockets, SSE)
- API gateways / Backend-for-Frontend (BFF)

Why it works:
- `Node.js` uses an event-driven, non-blocking I/O model
- High concurrency with relatively low memory overhead per connection

### 2. API composition layers (BFF / aggregation services)

JavaScript is very strong when acting as a “glue layer”:
- Aggregating multiple microservices
- Transforming data for frontend clients
- Handling auth/session orchestration

This is one of the most mature and widely adopted `Node.js` use cases in enterprise systems.

### 3. Rapid product development / startups

JavaScript is often ideal when:
- Speed of delivery matters more than long-term optimization
- The team is small
- Frontend and backend share a language

Benefits:
- Shared types/models (especially with TypeScript)
- Faster iteration cycles
- Large ecosystem (npm)

### 4. Serverless and event-driven architectures

Works very well in:
- Cloud functions (AWS Lambda-style systems, Azure Functions)
- Event handlers
- Webhook processors

Because:
- Fast cold start (relatively)
- Stateless execution fits the model
- Simple deployment unit

### 5. Real-time communication systems

Especially strong for:
- WebSockets
- Push notifications
- Multiplayer game signalling (not game engine logic itself)

## Where JavaScript is NOT a good choice

### 1. CPU-intensive workloads

Avoid JavaScript when the backend does heavy computation:
- Image/video processing
- Scientific computing
- Large-scale encryption workloads
- Machine learning training
- Complex simulations

Why:
- Single-threaded event loop blocks easily
- No native parallel CPU model (worker threads exist but are not primary architecture)

Better choices:
- C#, Java, Go, Rust, Python (with native extensions)

> [!WARNING]
> ❗️  CPU-heavy workloads are possible, but they are not Node.js's primary strength.

> [!WARNING]
> ❗️ Complex simulations and large-scale encryption tasks are examples of challenges that applications designed for highly regulated industries must meet.

### 2. High-throughput low-latency systems (trading, telecom core)

If you need:
- Predictable latency under load
- Deterministic performance
- Very low _GC_ impact

_JavaScript_ becomes risky because:
- Garbage collection pauses
- Event loop jitter under load
- Less control over memory layout

### 3. Strongly domain-driven enterprise systems with heavy complexity

You can build these in _JavaScript_, but risks increase when:
- Domain model is large and complex
- Many teams contribute over years
- Strict architectural boundaries are required

Without discipline, _JavaScript_ ecosystems tend to drift into:
- inconsistent patterns
- dynamic typing (if not using TypeScript)
- architecture erosion

> [!WARNING]
> ❗️ An extensive and complex domain model, a high degree of complexity, and the like are also typical challenges for applications in highly regulated industries.

### 4. Systems requiring strict multi-threaded control

Examples:
- High-performance message brokers
- Parallel computation engines
- Systems requiring fine-grained thread scheduling

## Evaluating JavaScript backend properly

### 1. Scalability

**Strengths**
- Excellent horizontal scalability
- Handles large numbers of concurrent connections efficiently
- Stateless services scale well in containers/serverless

**Weaknesses**
- CPU scaling is poor within a single process
- Requires clustering or multiple instances for CPU-heavy workloads

> [!NOTE]
> 👉 Key insight:
> JavaScript scales out very well, but not up very efficiently for CPU-bound tasks.

### 2. Performance

**Strengths**
- Very fast for I/O-bound workloads
- Good throughput under concurrency

**Weaknesses**
- Single-threaded event loop bottlenecks
- GC pauses can introduce latency spikes
- Not ideal for sustained heavy computation

### 3. Maintainability

**Strong when**:
- We use TypeScript
- We enforce architecture patterns (clean architecture, modular monolith, etc.)
- We control dependency sprawl

**Weak when**:
- Pure _JavaScript_ in large codebases
- Over-reliance on _npm_ packages without governance
- Lack of strict boundaries between layers

> [!NOTE]
> 👉 Reality:
> **Maintainability** is not a language issue, it **is an ecosystem discipline issue in _JavaScript_**.

### 4. Team expertise

_JavaScript_ is strongest when:
- Team already does frontend work
- Full-stack engineers are needed
- Rapid onboarding is important

But risks appear when:
- Backend complexity grows beyond frontend-style thinking
- Developers treat backend like frontend (no architecture discipline)

### 5. Operational requirements

**Strong points**:
- Excellent cloud-native support
- Easy containerisation (_Docker/Kubernetes_)
- Works well in serverless environments
- Huge observability ecosystem (logging, tracing, metrics)

**Weak points**:
- Memory tuning is less predictable than Go/Java
- GC tuning is limited
- Requires careful monitoring under high load

### Practical decision framework

Use JavaScript backend if MOST of these are true:
- Workload is I/O-bound
- We need real-time features
- We want fast delivery and iteration
- Our team is already JavaScript/TypeScript-heavy
- We are building APIs, gateways, or Backend for Frontend (BFF) layers

Avoid JavaScript backend if MOST of these are true:
- CPU-heavy processing dominates
- Ultra-low latency is critical
- You need strict deterministic performance
- System is mission-critical or deeply enterprise-core
- Long-lived, highly complex domain logic dominates system

> [!IMPORTANT]
> ❌  Are applications in highly regulated industries critical to an organization's operations? 👉 Definitely.

## Where JavaScript fits in strict legal systems

> [!WARNING]
> ❗️ In regulated legal applications, Node.js is best used as a boundary and orchestration layer, **not as the system of record**.

**Good roles for JavaScript**

### 1. API Gateway / BFF layer

- Request validation
- Authentication (OIDC, SSO)
- Routing to internal services
- Response shaping for UI

**Why it fits**:
- Stateless
- Easier to audit externally
- Limited domain responsibility

### 2. Workflow orchestration layer

- Case: `opened → notify → assign → log → trigger document generation`
- Calling downstream services (C#, Java, Python services)

> [!IMPORTANT]
> 👉  _Node.js_ should not own the business truth, only coordinate it.

### 3. Real-time / UI-facing services

- Notifications (case updates)
- WebSockets for dashboards
- Audit event streaming to UI

### 4. Integration layer

- Connecting legacy systems (courts, document systems, CRM, email, DMS)
- Translating formats (XML ↔ JSON, etc.)

### 5. Commonly Used In

- Banking:
  - Open Banking APIs
  - Mobile backend gateways
  - Authentication services
- Insurance:
  - Customer portals
  - Claims submission APIs
- Legal:
  - Case management portals
  - Document workflows
  - Client communication systems
- Healthcare:
  - Patient-facing portals
  - Scheduling systems

> [!WARNING]
> ❗️ At the same time, though, the core risk analysis engines are often implemented in C#, Java, or Go, while JavaScript and Node.js serve as the orchestration and integration layer.

## Where JavaScript Creates Additional Compliance Challenges

### 1. Dynamic Typing

Example:

```javascript
    amount = "100";
    amount = null;
    amount = {};
```

> [!WARNING]
> ❗️ The compiler allows it, but in regulated systems, data consistency matters.

Mitigation:
- Use TypeScript: `amount: number;`

### 2. Package Ecosystem Risk

A typical Node.js project might have 1500+ dependencies through transitive packages.

Auditors increasingly scrutinise:
- supply chain attacks
- vulnerable dependencies
- abandoned packages

Mitigation:

```
- Software Bill of Materials (SBOM)
- dependency scanning
- dependency pinning
- committed lock files
- approved package repositories
- Renovate or Dependabot[5] for controlled updates
- artifact signing (e.g. Sigstore)
- build provenance verification
- approved package lists
```

### 3. Managing the Software Supply Chain

> [!IMPORTANT]
> 📌 In highly regulated industries, third-party dependencies must be governed with the same level of discipline as internally developed code.

Modern Node.js applications frequently depend, directly and indirectly, on thousands of open-source packages.

Every dependency introduces additional operational, security, and compliance risks.

A mature engineering organisation therefore treats dependency management as part of its secure software development lifecycle rather than a routine package update.

**Dependency Pinning**

> [!IMPORTANT]
> 📌 Applications should always use deterministic dependency versions.

Avoid:

```
{
    "express": "^5.0.0"
}
```

Prefer:

```
{
    "express": "5.0.0"
}
```

Pinning dependency versions ensures that every build uses the exact versions that have been tested and approved.

> [!NOTE]
> ✔ Predictable builds are a fundamental requirement for auditability and long-term reproducibility.

**Lock Files**

A lock file records the complete dependency graph used during a successful build.

Examples include:
- package-lock.json
- pnpm-lock.yaml
- yarn.lock

> [!NOTE]
> ✔  These files should always be committed to source control.

Without a lock file, identical source code may produce different applications on different days.

> [!WARNING]
> ❗️ In regulated environments, reproducible builds are often a compliance requirement rather than simply a best practice.

**Automated Dependency Updates**

Keeping dependencies current is important, but updates should never be applied blindly.

Tools such as:
- Renovate
- Dependabot

can automatically:
- detect outdated packages
- identify known vulnerabilities
- create pull requests
- propose version upgrades

However, every proposed update should still undergo:
- automated testing
- security scanning
- architectural review
- controlled deployment

> [!WARNING]
> ❗️ Automation accelerates maintenance - it does not replace engineering judgement.

**Signed Packages**

Modern package ecosystems increasingly support cryptographic signatures.

Signed packages allow consumers to verify that software:
- originated from the expected publisher
- has not been modified
- has not been replaced during distribution

Package signing significantly reduces supply-chain attack risks.

**Sigstore**

One of the most important recent developments is ___Sigstore___.

Sigstore provides an open framework for:
- package signing
- artifact verification
- certificate transparency
- identity verification

Rather than relying solely on long-lived signing keys, Sigstore issues short-lived certificates linked to verified identities.

This substantially improves trust throughout the software supply chain.

**Build Provenance**

Knowing what was built is only part of the story.

Equally important is proving:
- who built it
- when it was built
- from which commit
- using which pipeline
- with which dependencies

This information is known as ___build provenance___.

> [!WARNING]
> ❗️ A mature CI/CD pipeline should be able to demonstrate that every production artifact originated from an approved and auditable build process.

**Enterprise Supply Chain Policy**

A typical enterprise policy might require:
- approved package repositories
- dependency pinning
- committed lock files
- automated vulnerability scanning
- SBOM generation
- signed build artifacts
- provenance verification
- mandatory pull request reviews
- automated regression testing before upgrades

> [!IMPORTANT]
> 📌 **Compliance** is not achieved by trusting open-source packages. 
> It **is achieved by continuously verifying the integrity, provenance, and security of every dependency incorporated into the application**.

**CI/CD Example**

A mature pipeline for regulated software might include:

```
Source Code
      │
      ▼
Dependency Restore
      │
      ▼
Lock File Verification
      │
      ▼
Static Analysis
      │
      ▼
Dependency Scan
      │
      ▼
SBOM Generation
      │
      ▼
Unit Tests
      │
      ▼
Artifact Signing
      │
      ▼
Provenance Generation
      │
      ▼
Deployment Approval
      │
      ▼
Production
```

### 4. Architectural Discipline

JavaScript does not force:
- layering
- DDD
- clean architecture

**A team must enforce it**.

Without discipline:

```
    Controller
      ↓
    Database
```

becomes common.

But, compliance systems need:

```
    Controller
      ↓
    Application Service
      ↓
    Domain Service
      ↓
    Repository
```

### 5. Runtime Validation

> [!WARNING]
> ❗️ TypeScript types disappear at runtime!

> [!NOTE]
> 👉 This surprises many teams.

But, we still need:

```typescript
    vzod
    joi 
    class-validator
```

for input validation.

> [!IMPORTANT]
> 📌  Especially important for GDPR and PCI.

### 6. Other risks in legal systems

> [!IMPORTANT]
> ❌ Avoid using `JavaScript/Node.js` for 
> ___Core legal domain logic___ (“system of record”)

Examples:
- Case lifecycle rules
- Legal status determination
- Contract validation and verification logic
- Evidence integrity rules

Why:
- Harder to formally verify
- Easier to introduce logic drift over time
- Dynamic typing risk (unless strictly TypeScript + validation layers)

> [!IMPORTANT]
> ❌ Audit-critical computation

If the result must be:
- Legally defensible
- Reproducible years later
- Identical across environments

`JavaScript/Node.js` is weaker than:
- .NET (C#)
- Java (strong enterprise auditing ecosystem)
- Go (deterministic runtime characteristics)

> [!IMPORTANT]
> ❌ Long-running transactional workflows

> [!WARNING]
> ❗️ Node.js is not ideal for
> - Multi-day case workflows with state persistence inside process memory

> [!NOTE]
> ✔ Instead use - persistent workflow engines (e.g., durable orchestration patterns)



## JavaScript vs TypeScript in regulated software

> [!IMPORTANT]
> 📌 Modern enterprise Node.js applications are rarely written in plain JavaScript. 
> They are typically developed using TypeScript, together with strict compiler settings, runtime validation, verification, and rigorous development standards.

While JavaScript and TypeScript ultimately execute on the same runtime (Node.js), they provide very different development experiences.

The difference becomes particularly significant in highly regulated industries such as legal, healthcare, finance, insurance, and government, 
where software must remain maintainable, auditable, and reliable over many years.

### JavaScript

JavaScript provides maximum flexibility.

```javascript 
let amount = 100;

amount = "100";
amount = null;
amount = {};
```

Although this flexibility can accelerate rapid prototyping, it also increases the likelihood of introducing defects that remain undetected until runtime.

> [!WARNING]
> ❗️ In large enterprise systems, such defects can become difficult to trace and expensive to correct.

### TypeScript

TypeScript adds a static type system on top of JavaScript.

> [!NOTE]
> ✔  TypeScript introduces additional build complexity, configuration overhead, and occasionally more verbose type definitions. 
> These costs are generally outweighed by improved maintainability and defect prevention in large, long-lived systems.

```typescript
let amount: number = 100;

// Compile-time errors
amount = "100";
amount = null;
amount = {};
```

The compiler detects many classes of defects before the application is deployed.

This significantly improves:
- maintainability
- refactoring safety
- IDE assistance
- code navigation
- long-term reliability

> [!NOTE]
> ✔ Strong compile-time validation does not eliminate the need for runtime validation.

### Strict Compiler Settings (Non-Negotiable)

> [!IMPORTANT]
> 📌 In regulated software, TypeScript should always be configured in strict mode.

Recommended settings include:

```json
{
    "compilerOptions": {
        "strict": true,
        "noImplicitAny": true,
        "strictNullChecks": true,
        "noUncheckedIndexedAccess": true,
        "exactOptionalPropertyTypes": true,
        "noImplicitOverride": true
    }
}
```

These options eliminate many common sources of programming errors before deployment.

> [!IMPORTANT]
> 📌 Without strict compiler settings, much of TypeScript's safety advantage is lost.

### ESLint

> [!NOTE]
> ✔  Static analysis should extend beyond the compiler.

ESLint enforces coding standards that improve consistency and reduce implementation defects.

Typical enterprise rules include:
- no unused variables
- no floating promises
- no implicit any
- mandatory error handling
- consistent import ordering
- restricted use of any
- prevention of unsafe type assertions

> [!NOTE]
> ✔ A mature project should treat linting failures as build failures within the CI/CD pipeline.

### Runtime Validation & Verification

> [!WARNING]
> ❗️  One of the most common misconceptions is that TypeScript guarantees runtime correctness.

**It does not!**

> [!WARNING]
> ❗️ Type information is removed during compilation.

```
interface CustomerDto {
    age: number;
}
```

The following JSON is still accepted by the application:

```
{
    "age": "twenty"
}
```

unless explicit conversion is performed.

> [!WARNING]
> ❗️ Compile-time validation protects developers.
>
> ✔  Runtime validation protects production systems.
>
> 👉  Both are required.

### Zod

One increasingly popular solution is ___Zod___.

```typescript
const CustomerSchema = z.object({
    age: z.number().min(18)
});
```

Benefits include:
- runtime validation
- automatic type inference
- readable schemas
- excellent developer experience
- straightforward integration with REST APIs

> [!NOTE]
> 👉  Because the schema becomes the single source of truth, both runtime validation and TypeScript types remain synchronized.

### io-ts

Another mature alternative is ___io-ts___.

```typescript
const Customer = t.type({
    age: t.number
});
```

Compared to Zod:
- stronger functional-programming approach
- excellent integration with fp-ts
- highly expressive type composition
- steeper learning curve

> [!NOTE]
> 👉  Large enterprise systems that already embrace functional programming frequently prefer io-ts.

### Branded Types

Primitive types are often insufficient for modelling business domains.

For example:

```typescript
type CustomerId = string;
type OrderId = string;
```

These types are interchangeable, allowing defects such as:

```
getCustomer(orderId);
```

Branded types prevent this class of errors.

```
type CustomerId = string & {
    readonly __brand: "CustomerId";
};

type OrderId = string & {
    readonly __brand: "OrderId";
};
```

The compiler now rejects accidental misuse of domain identifiers.

> [!NOTE]
> ✔ Branded types provide a lightweight alternative to value objects for many enterprise applications.

### Data Transfer Object (DTO) Validation

> [!IMPORTANT]
> 📌 Every external request should be validated before reaching business logic.

```
Internet
    ↓
API Gateway
    ↓
DTO Validation
    ↓
Application Service
    ↓
Domain Service
```

A validated Data Transfer Object (DTO) should guarantee:
- required fields exist
- correct data types
- valid ranges
- valid formats
- permitted values
- business-independent structural correctness

Business rules should only execute after DTO validation succeeds.

> [!IMPORTANT]
> 📌 Never trust data received from browsers, mobile applications, partner APIs, or internal services.

**Every boundary is a trust boundary.**

### Recommended Enterprise Validation Pipeline

A mature Node.js backend typically validates data at multiple layers.

```
Incoming JSON
        │
        ▼
Runtime Validation
(Zod / io-ts)
        │
        ▼
DTO
(TypeScript)
        │
        ▼
Application Service
        │
        ▼
Domain Model
(Branded Types / Value Objects)
        │
        ▼
Business Rules
```

> [!IMPORTANT]
> 📌 Each layer eliminates a different category of defects.

> [!NOTE]
> ✔ No single mechanism is sufficient on its own.

### Compliance Perspective

> [!IMPORTANT]
> 📌 Neither JavaScript nor TypeScript makes an application compliant with GDPR, SOC 2, SRA, or PCI DSS.

However, TypeScript significantly improves an application's ability to satisfy compliance requirements by making the codebase easier to:
- understand
- review
- audit
- test
- refactor
- maintain over many years

Compliance depends on a combination of:
- architecture,
- development processes,
- operational controls,
- governance,
- testing,
- documentation,
- traceability,
- auditing.

> [!NOTE]
> ✔  Programming language choice is one contributing factor rather than the determining factor.

When combined with strict compiler settings, runtime validation, immutable audit logging, secure CI/CD pipelines, comprehensive automated testing, 
and disciplined architectural practices, **TypeScript becomes a strong foundation** for enterprise-grade Node.js applications.

```
| Concern              | Compile Time | Runtime |
| -------------------- | ------------ | ------- |
| Type checking        | ✓            | ✗       |
| Null safety          | ✓            | ✗       |
| JSON validation      | ✗            | ✓       |
| Authorization        | ✗            | ✓       |
| Business rules       | ✗            | ✓       |
| Database constraints | ✗            | ✓       |
```

The practices I recommend align with widely recognised standards, like:
- OWASP ASVS
- OWASP SAMM
- NIST Secure Software Development Framework (SSDF)
- SLSA
- ISO/IEC 27001
- ISO/IEC 27034
- CIS Software Supply Chain Security

## Final Recommendation

> [!IMPORTANT]
> 📌 Enterprise Node.js applications supporting regulated legal systems should adopt ___TypeScript___ with strict compiler settings as the minimum acceptable baseline.

> [!NOTE]
> ✔ In modern enterprise Node.js development, dependency governance is no longer optional.

Dependency pinning, lock files, automated update management, artifact signing, provenance, and continuous vulnerability monitoring 
should be regarded as baseline engineering practices for applications operating in highly regulated environments.

..tbc..

## See also:
1. [Open Standards for Government](https://www.gov.uk/government/publications/open-standards-for-government)
2. [Open standards for government data and technology](https://www.gov.uk/government/collections/open-standards-for-government-data-and-technology)
3. [A guide to good practice for digital and data-driven health technologies](https://www.gov.uk/government/publications/code-of-conduct-for-data-driven-health-and-care-technology/initial-code-of-conduct-for-data-driven-health-and-care-technology)
4. [UK Government Publishes Guidelines for Artificial Intelligence Procurement](https://www.bevanbrittan.com/insights/articles/2020/uk-government-publishes-guidelines-for-artificial-intelligence-procurement/)
5. [Dependabot quickstart guide](https://docs.github.com/en/code-security/tutorials/secure-your-dependencies/dependabot-quickstart)

## See:
- [JavaScript Everywhere – Part 2: Applications for Highly Regulated Industries - Compliance and Governance](https://www.linkedin.com/pulse/javascript-everywhere-part-2-applications-highly-regulated-kubis-wc0je/)
- [JavaScript Everywhere – Part 3: Applications for Highly Regulated Industries - Auditability, Testing, CI/CD, Observability](https://www.linkedin.com/pulse/javascript-everywhere-part-3-applications-highly-regulated-kubis-ruwne/)

- [Availability vs Identity in Distributed C#/.NET Applications - Part 1: The Role of Availability and Identity](https://www.linkedin.com/pulse/availability-vs-identity-distributed-cnet-part-1-role-marek-kubis-xvpze/)
- [Availability vs Identity in Distributed C#/.NET Applications - Part 2: Lock-in on Use Cases and on Cloud](https://www.linkedin.com/pulse/availability-vs-identity-distributed-cnet-part-2-lock-in-kubis-zhmee/)

- [What is managed identities for Azure resources?](https://learn.microsoft.com/en-us/azure/active-directory/managed-identities-azure-resources/overview)
- [IAM Roles](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_roles.html)
- [Authenticate to Google Cloud APIs from GKE workloads](https://cloud.google.com/kubernetes-engine/docs/how-to/workload-identity)
- [What is Azure role-based access control (Azure RBAC)?](https://learn.microsoft.com/en-us/azure/role-based-access-control/overview)

- [Once and Only Once with Examples - Part 1: Is It Obvious?](https://www.linkedin.com/pulse/once-only-examples-part-1-obvious-marek-kubis-nyebe/)
- [Once and Only Once with Examples - Part 2: And AI-generated Code](https://www.linkedin.com/pulse/once-only-examples-part-2-ai-generated-code-marek-kubis-kn9ie/)
- [Once and Only Once with Examples - Part 3: Where Duplication Is Simultaneously Necessary](https://www.linkedin.com/pulse/once-only-examples-part-3-where-duplication-necessary-marek-kubis-vpxce/)

- [Mutation testing - Part 1: is it outdated?](https://lnkd.in/eDbVukCf)
- [Mutation testing - Part 2: Turn into a production-ready tool](https://lnkd.in/eSx9b6pB)
- [Mutation testing - Part 3: Mutation testing limits and how to go beyond it](https://lnkd.in/e3qsTXBy)
- [Mutation testing - Part 4: mutation testing and LLM-written code](https://lnkd.in/eKfvJfbp)

- [Underestimated and Annoying, or the "Dirty Dozen" of Programmers - Part 1: The Problem Space](https://www.linkedin.com/pulse/underestimated-annoying-dirty-dozen-programmers-marek-kubis-mcfxe)
- [Underestimated and Annoying, that is "The Dirty Dozen" of Programmers - Part 2: AI-Generated Software](https://www.linkedin.com/pulse/underestimated-annoying-dirty-dozen-programmers-part-2-marek-kubis-tqkme/)
- [Underestimated and Annoying, that is "The Dirty Dozen" of Programmers - Part 3: I. Organizational Problems](https://www.linkedin.com/pulse/underestimated-annoying-dirty-dozen-programmers-part-marek-kubis-h9y3e/)
- [Underestimated and Annoying, that is "The Dirty Dozen" of Programmers - Part 4: II. Human Problems](https://www.linkedin.com/pulse/underestimated-annoying-dirty-dozen-programmers-part-marek-kubis-mn5ve/)
- [Underestimated and Annoying, that is "The Dirty Dozen" of Programmers - Part 5: III. Process Problems](https://www.linkedin.com/pulse/underestimated-annoying-dirty-dozen-vibe-coding-part-marek-kubis-83jre/)
- [Underestimated and Annoying, that is "The Dirty Dozen" of Programmers - Part 6: IV. Architecture Problems](https://www.linkedin.com/pulse/underestimated-annoying-dirty-dozen-programmers-part-marek-kubis-remze/)
- [Underestimated and Annoying, that is "The Dirty Dozen" of Programmers - Part 7: V. Validation Problems](https://www.linkedin.com/pulse/underestimated-annoying-dirty-dozen-programmers-part-marek-kubis-dqk2e/)
- [Underestimated and Annoying, that is "The Dirty Dozen" of Programmers - Part 8: VI. Economic Problems](https://www.linkedin.com/pulse/underestimated-annoying-dirty-dozen-programmers-part-marek-kubis-7bb6e/)

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

- [Agile Vibe Coding positioning and if this works, what changes?](https://www.linkedin.com/pulse/agile-vibe-coding-positioning-works-what-changes-marek-kubis-r4ate)
- [Agile Vibe Coding – Ceremony Modes](https://www.linkedin.com/pulse/agile-vibe-coding-ceremony-modes-marek-kubis-meq9e)
- [Agile Vibe Coding ceremonies approach compared to a simple one-prompt-per-task approach](https://www.linkedin.com/pulse/agile-vibe-coding-ceremonies-approach-compared-simple-marek-kubis-ecx5e)
- [Agile Vibe Coding Maturity Model](https://www.linkedin.com/pulse/agile-vibe-coding-maturity-model-marek-kubis-bbtqe)
- [The Agile Vibe Coding - the 4-level adaptive ceremony system](https://www.linkedin.com/pulse/agile-vibe-coding-4-level-adaptive-ceremony-system-marek-kubis-jizke)

- [Agile Vibe Coding Manifesto](https://agilevibecoding.org/)
- [Principles Behind the Agile Vibe Coding Manifesto - extended version](https://github.com/marekartur-dev/agilevibecoding/blob/main/Docs/Home/Principles.md)

- [Agile Vibe Coding](https://www.reddit.com/r/AgileVibeCoding/)
- [Marek Kubis - blog](https://github.com/marekartur-dev/agilevibecoding/tree/main)