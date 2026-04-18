# Kafka & Service Bus — Part 1: Two Philosophies of Event-Driven Systems
_Comparing Apache Kafka with Microsoft Service Bus_

> Last year, I discussed [using service APIs](https://www.linkedin.com/pulse/service-api-vs-message-bus-marek-kubis-nntle) 
> to deliver data to service-based applications in a cloud environment. 
> This architecture is used when data is received asynchronously as a result of event handling. 
> Such data typically comes from distributed sources. However, in this case, it is not the only solution used.

<img src="./../Images/Kafka_and ServiceBus.png" alt="Event-driven architectures" width="600"  style="float: right; margin-left: 10px;">

**The event-driven architecture** is also managed by the Apache Kafka server. 
Apache Kafka is a distributed event-streaming platform designed for high-throughput, fault-tolerant, and scalable data processing. 
It is widely used for real-time data pipelines and stream processing. 
However, the difference between Kafka and Service Bus goes beyond features. 
Both platforms are used by event-driven systems, but we cannot treat them interchangeably, 
as they were built according to different philosophies.

```
Kafka = database of events (append-only log)
Service Bus = delivery system for tasks

┌------------------------┬--------------------------------┐
| Kafka is built for:    | Service Bus is built for:      |
|- Streaming pipelines   | - Business workflows           |
|- Telemetry ingestion   | - Commands                     |
|- High-frequency events | - Integration between services |
└------------------------┴--------------------------------┘
```

## Core mindset

> - Think about Kafka as a **“source of truth for events”**.
> - Think about Service Bus as a **“task distribution / command handling”**.

```
┌--------------------------------------┬---------------------------------┐
| Kafka is an event streaming platform:| Service Bus is a message broker:|
|- Stores events as a durable log      | - Messages are processed and    |
|                                      |   removed                       |
|- Consumers pull and replay events    | - Consumers receive and complete|
|  anytime                             |   messages                      |
|- Designed for high-throughput,       | - Designed for reliable         |
|  real-time streams                   |   enterprise messaging          |
└--------------------------------------┴---------------------------------┘
```

### Event-driven implementation differences

> - Kafka: ```Producer → Topic (log) → Consumers (pull & replay)```
> - Service Bus: ```Producer → Queue/Topic → Consumer → Complete/Delete```

```
┌-----------------------------------┬------------------------------------┐
| Kafka model:                      | Service Bus model:                 |
|- Producer writes event → topic    |- Producer sends message →          |
|- Event stays in topic             |   queue/topic                      |
|  (retention: hours → forever)     |- Message is delivered → processed  |
|- Multiple consumers read          |   → completed (removed)            |
|  independently                    |- Competing consumers (queue) OR    |
|- Consumers track their own offset |   pub/sub (topic + subscriptions)  |
|-----------------------------------|------------------------------------|
| Key implications:                 | Key implications:                  |
|- Event replay = built-in          |- No natural replay                 |
|                                   |    (unless you build it)           |
|- Multiple consumers = easy        |- Strong delivery guarantees        |
|- Loose coupling = very high       |- More “workflow-oriented”          |
└-----------------------------------┴------------------------------------┘
```

### Heavy-load behaviour (this is where they diverge sharply)

> - **Kafka shines here.** 
> - The service bus handles workload differently than Kafka.

```
┌------------------------------------┬-----------------------------------┐
| Kafka under heavy load:            | Service Bus under heavy load:     |
|------------------------------------|-----------------------------------|
| Strengths:                         | Strengths:                        |
| - Sequential disk writes, very fast| - Reliable delivery, at-least-once|
| - Partitioning → horizontal scaling| - Built-in retries,               |
| - Consumers scale independently    |   dead-letter queues              |
|                                    | - Transaction support             |
|                                    |                                   |
| Real numbers (typical):            | Limits:                           |
| - Millions of messages/sec possible| - Throughput is lower than Kafka  |
| - Handles TBs of data easily       | - Scaling is more constrained     |
|                                    |   (tier-based)                    |
|                                    | - Latency increases under         |
|                                    |   very high load                  |
|                                    |                                   |
| Scaling model                      | Scaling model:                    |
| - Add partitions                   | - Premium tier,dedicated resources|
|    → increase parallelism          | - Messaging units, but ..         |
| - Add consumers → scale reads      | - not as horizontally elastic     |
|                                    |   as Kafka                        |
└------------------------------------┴-----------------------------------┘
```

### Ordering & partitioning

```
┌-----------------------------------------┬------------------------------┐
| Kafka:                                  | Service Bus:                 |
|- Ordering guaranteed within a partition | - Ordering via sessions      |
|- We control partitioning via keys       | - More complex to manage     |
|                                         |   at scale                   |
└-----------------------------------------┴------------------------------┘
```

> - Kafka: ```Key = "customer-123" // ensures order per customer```
> - **Kafka = simpler + more scalable ordering**

### Replay & event sourcing

> - **Kafka supports event replay natively** via its log-based storage model.
> - **Service Bus does not support replay natively**; messages are removed after processing.
  
```
┌-----------------------------------------┬------------------------------┐
| Kafka:                                  | Service Bus:                 |
| - Replay events anytime                 | - Messages are removed       |
|   (log retention)                       |   after completion           |
| - Consumers control offsets             | - Replay requires external   |
|                                         |   storage or duplication     |
|                                         |                              |
| Perfect for:                            | Replay requires:             |
|- Event sourcing                         | - Custom storage             |
|- Rebuilding state                       | - Dead-letter tricks         |
|- Debugging                              | - Or duplication             |
└-----------------------------------------┴------------------------------┘
```

> 👉 Huge architectural difference

### Delivery guarantees
```
Feature		Kafka		    Service Bus
-----------------------------------------------
At least once	✔️		    ✔️
Exactly once	✔️ (complex)	    ❌ (not native)
Transactions	✔️		    ✔️
Dead-lettering	✔️ (pattern-based)   ✔️ built-in
```
Warning:
- Kafka doesn’t have built-in DLQ like Service Bus, but DLQ is a well-established pattern (not “missing feature”) via retry/DLQ topics. 
- Exactly-once semantics in Kafka are possible but require careful configuration (idempotent producers, transactions) and are not trivial to achieve in practice.
- Exactly-once semantics in Service Bus are not natively supported; we must implement idempotency at the consumer level to achieve similar results (at-least-once with deduplication support).

### Latency vs throughput trade-off
```
Feature			Kafka		Service Bus
---------------------------------------------------------
Optimized for:		throughput	reliability & correctness
Under heavy load:	stays fast	stays safe
```

### Typical architecture usage

```
┌-------------------------------┬-----------------------------------┐
| Use Kafka for:                | Use Service Bus for:              |
| - event streaming             | - commands                        |
| - analytics                   | - workflows                       |
|-------------------------------|-----------------------------------|
| Use Kafka when:               | Use Service Bus when:             |
| - events are data             | - messages are commands           |
| - you need replay             | - you need guaranteed processing  |
| - you expect massive scale    | - you want simpler operations     |
└-------------------------------┴-----------------------------------┘
```

> - Kafka pattern:       ```Microservices → Kafka → Stream processing → Analytics / DB```
> - Service Bus pattern: ```API → Queue → Worker → Database```

```
┌-------------------------------┬-----------------------------------┐
| ❌ Don’t use Kafka if:       | ❌ Don’t use Service Bus if:      |
|-------------------------------|-----------------------------------|
| You just need simple queueing	| You need event streaming at scale |
| You don’t need replay	        | You need replay/event sourcing    |
| You want minimal ops overhead	| You expect very high throughput   |
└-------------------------------┴-----------------------------------┘
```

### Do we even need Kafka? - Reality Check

> Most systems don’t need Apache Kafka — until they really do.

```
┌-----------------------------------┬------------------------------------┐
| ✔️ We NEED Kafka if:              | ❌ Don’t DON’T need Kafka if:     |
|-----------------------------------|------------------------------------|
| We expect > 50k–100k messages/sec | We’re doing business               |
|  sustained                        |  workflows / commands              |
| We need event replay              | Load is moderate (< 20k msg/sec)   |
|  (event sourcing, audit rebuild)  |                                    |
| Multiple systems must consume     | We don’t need replay               |
|  the same events independently    |                                    |
| We’re building analytics          | We want simpler ops (huge factor)  |
|  / streaming pipelines            |                                    |
└-----------------------------------┴------------------------------------┘
```

> **👉 If you're unsure, start with Service Bus. Add Kafka later.**


## 1. Starting point - scaling WITHOUT Kafka 

### Key bottlenecks under heavy load

1) Azure Function
- Cold starts
- Concurrency limits

2) Service Bus
- Throughput units / messaging units
- Partitioning

3) Consumers (Kubernetes)
- Processing speed
- Parallelism

### How to scale properly

1) Azure Function scaling
- Enable max concurrency
- Use batch processing

Example:
```csharp
[Function("ProcessEvents")]
public async Task Run([ServiceBusTrigger("orders", IsBatched = true)]
                      ServiceBusReceivedMessage[] messages)
{
    foreach (var msg in messages)
    {
        // process
    }
}
```

> **Batch = massive throughput improvement**

2) Service Bus scaling - _critical tuning_
- Premium tier
- Partitioned queues/topics
- Scale via: **Messaging Units (MU)**
- Rough capability: `~1,000–2,000 msg/sec per Messaging Unit (workload-dependent)`

3) Kubernetes consumer scaling

- Horizontal Pod Autoscaler (**HPA**)
- Increase replicas based on CPU/memory or custom metrics: `replicas: 10 → 100+`
- Optimize processing logic for parallelism
```
await Task.WhenAll(messages.Select(msg => Process(msg)));
```
> [!WARNING]
> - Instruction `Parallel.ForEach` does NOT support async properly.
> - Limit the number of concurrent operations if needed to avoid overwhelming downstream systems. 
>   [Example](https://www.linkedin.com/pulse/mechanical-sympathy-part-3-suggestions-avoiding-software-marek-kubis-vybbe/) or [SemaphoreSlim](https://docs.microsoft.com/en-us/dotnet/api/system.threading.semaphoreslim) can help.

### Advantages of the proposed solution

Service Bus helps here:
- Messages queue up safely
- Consumers scale independently

- 👉 Service Bus provides built-in buffering and backpressure via queues  
- 👉 Kafka relies on consumer lag and retention rather than explicit queueing semantics

### When this architecture starts breaking

We’ll hit limits when:
- We need real-time analytics
- We want multiple independent consumers
- We need replay
- Throughput approaches 100k+ msg/sec


## 2. Introduce Kafka (hybrid architecture)

Here’s the **correct way to add Kafka**, not replace everything.
``` 
    External Systems
          ↓
    Azure Gateway
          ↓
    Azure Function
          ↓
 ┌────────┴───────┐
 ↓                ↓
Service Bus      Kafka
(commands)       (event stream)
 ↓                ↓
K8s Workers      Analytics / Replay / Other Services
```

Let's separate responsibilities cleanly:
```
┌------------------------┬------------------------┐
| Kafka handles:         | Service Bus handles:   |
|------------------------|------------------------|
|- Event streaming       | - Commands             |
|- Replay                | - Business workflows   |
|- Analytics             | - Reliable processing  |
|- Fan-out at scale      |                        |
└------------------------┴------------------------┘
```

### Implementation

**Azure Function** 
```csharp
public async Task Run(...)
{
    // 1. Send command to Service Bus
    await _serviceBusSender.SendMessageAsync(message);

    // 2. Publish event to Kafka
    await _kafkaProducer.ProduceAsync("events", kafkaMessage);
}
```
> Same event → two systems, different purposes

### Heavy load comparison in the example configuration
```
Aspect	    Service Bus only	Kafka added
-----------------------------------------------
Simplicity          ✅	          ❌
Reliability	    ✅ strong	  ✅ strong
Replay	            ❌	          ✅
Throughput	    ⚠️ limited   ✅ massive
Ops complexity      ✅ low       ❌ high
Scaling consumers   ✅           ✅ better
```

### Real-world scaling numbers

**Service Bus** (Premium): 
- ~10k–20k msg/sec (well-tuned)
- Cost fully managed
- Minimal operational reality

**Kafka** cluster requirements:
- 100k → millions msg/sec
- Partition planning
- Broker scaling
- Monitoring
- Retention tuning

> 👉 Kafka = engineering commitment (even in Azure via Microsoft ecosystem or Confluent) 

### Recommended strategy

**1) Phase 1 (now)**

Stick with:
- Azure Function
- Service Bus
- Kubernetes

> 👉 Optimize scaling (we likely haven’t hit limits yet)

**2) Phase 2 (when needed)**

Introduce Kafka ONLY for:
- Event streaming
- Analytics
- Replay

**3) Phase 3 (advanced)**

Move toward:
- Event-driven platform
- Possibly event sourcing

### Hybrid architecture summary 
- **Service Bus = backbone of business processing**.
- **Kafka = backbone of event streaming**.
- Don’t replace one with the other.
- Use both only when justified by scale or requirements.


## Summary 

Hybrid approach - solid enterprise pattern:
- Service Bus triggers business process → updates state → produces event to Kafka 
- Kafka streams all events for analytics + replay

```
External Systems
      ↓
Azure Gateway
      ↓
Azure Function
      ↓
Service Bus (Queue/Topic)
      ↓
Kubernetes (.NET consumers)
```

_In the next article from the series, I’ll present realistic numbers based on production experience to understand when Kafka becomes necessary and when Service Bus is sufficient._

## See also:
- [Kafka vs Service Bus Part 2: Real-world Architectures](./Kafka_and_ServiceBus_Part_2.md)
- [Kafka vs Service Bus Part 3: Technical Comparison](./Kafka_and_ServiceBus_Part_3.md)

- [Agile Vibe Coding Manifesto](https://agilevibecoding.org/)
- [Principles Behind the Agile Vibe Coding Manifesto - extended version](https://github.com/marekartur-dev/agilevibecoding/blob/main/Docs/Home/Principles.md)

- [Agile Vibe Coding](https://www.reddit.com/r/AgileVibeCoding/)
- [Marek Kubis - blog](https://github.com/marekartur-dev/agilevibecoding/tree/main)
