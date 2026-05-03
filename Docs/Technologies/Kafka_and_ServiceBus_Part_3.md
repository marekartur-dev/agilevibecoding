# Kafka & Service Bus — Part 3: An Integration

> In Part 1, we compared Kafka and Service Bus from a technical perspective.
> In Part 2, we looked at real-world business systems and how these technologies fit into their architectures.
> In Part 3, we’ll explore how to integrate these two platforms effectively in a hybrid architecture,
> leveraging the strengths of both while mitigating their weaknesses.
>
> The goal is to create a seamless data flow that allows us to use Service Bus for reliable command processing and Kafka for scalable event streaming and analytics.
> The goal is also to design an architecture that can be developed with AI support and evolve over time, starting with Service Bus and introducing Kafka as needed, 
> without the need for a major overhaul.

<img src="./../Images/Epic_battle_for_intelligence_supremacy.png" alt="Epic battle for event-driven & AI supremacy" width="600"  style="float: right; margin-left: 10px;">

## Integration patterns

### ✅ Pattern A — Dual-write (most common)

Producer writes to:
- Service Bus (commands)
- Kafka (events)

> [!TIP]
> 👉 Fast, simple, but needs idempotency

### ✅ Pattern B — Service Bus → Kafka bridge
- Commands enter Service Bus
- Forwarded into Kafka for streaming

> [!TIP]
> 👉 Best when we already rely on Service Bus

### ✅ Pattern C — Kafka → Service Bus bridge
- Kafka is source of truth
- Selected events trigger workflows via Service Bus

> [!TIP]
> 👉 Best for event-driven platforms

### Reliability patterns 
_non-optional_

- **Idempotency**
```
if (await _store.Exists(event.Id))
    return;
```

- **Outbox pattern** (CRITICAL for dual-write)

> Prevents message loss

```
DB Transaction:
  - Save Order
  - Save Event (Outbox table)

Background Worker:
  - Read Outbox → send to Kafka/SB
  - Outbox + Change Data Capture (CDC) → publish to Kafka without polling
```

- **Loop prevention (critical for infinite loops SB → Kafka → SB)**
```
- Add message headers (e.g., source=sb/kafka)
- Avoid reprocessing already bridged events
```

- **Auto commit vs manual commit**

> [!IMPORTANT]
> Kafka consumers should use controlled offset commits (manual or transactional) to avoid message loss or duplication.

### Retry strategy

- **Service Bus**: Built-in retries + Dead Letter Queue, DLQ
- **Kafka**: Retry topics: `orders → orders-retry → orders-dead` → Retry topics should include delay strategy (e.g., exponential backoff).

### Recommendation 

> [!TIP]
> Use Pattern B or C depending on system ownership. 
> Bi-directional integration should be used carefully to avoid feedback loops.


## Complete solution

### Install NuGet package
```bash
dotnet add package Confluent.Kafka
```

### Target Architecture

```
            ┌────────────────────────────┐
            │        Producers           │
            │  APIs / External Systems   │
            └────────────┬───────────────┘
                         ↓
              Azure Service Bus (commands)
                         ↓
          ┌──────────────┴──────────────┐
          ↓                             ↓
   SB → Kafka Bridge             Business Workers
          ↓
     Kafka Cluster (event stream)
          ↓
 ┌────────┴─────────┐
 ↓                  ↓
Analytics      Kafka → SB Bridge
/ ML                ↓
               Workflow triggers
```

> Implementation components: we need 3 services

### Component 1: Service Bus → Kafka Bridge example .NET Worker class
_Consumes SB → publishes to Kafka_

```csharp
using Azure.Messaging.ServiceBus;
using Confluent.Kafka;
using System.Text;

public class SbToKafkaBridge
{
    private readonly ServiceBusProcessor _processor;
    private readonly IProducer<string, string> _producer;

    public SbToKafkaBridge(ServiceBusClient sbClient)
    {
        _processor = sbClient.CreateProcessor("orders", new ServiceBusProcessorOptions
        {
            MaxConcurrentCalls = 10,
            AutoCompleteMessages = false
        });

        var config = new ProducerConfig
        {
            BootstrapServers = "kafka:9092"
        };

        _producer = new ProducerBuilder<string, string>(config).Build();
    }

    public async Task StartAsync()
    {
        _processor.ProcessMessageAsync += async args =>
        {
            var body = args.Message.Body.ToString();

            var result = await _producer.ProduceAsync("orders-events",
                new Message<string, string>
                {
                    Key = args.Message.MessageId,
                    Value = body
                });

            // Check delivery status (result.Status)

            await args.CompleteMessageAsync(args.Message);
        };

        await _processor.StartProcessingAsync();
    }
}
```

### Component 2: Kafka → Service Bus Bridge example .NET Worker class
_Consumes Kafka → triggers workflows_

```csharp
using Confluent.Kafka;
using Azure.Messaging.ServiceBus;

public class KafkaToSbBridge
{
    public async Task Run()
    {
        var consumer = new ConsumerBuilder<string, string>(
            new ConsumerConfig
            {
                BootstrapServers = "kafka:9092",
                GroupId = "bridge",
                AutoOffsetReset = AutoOffsetReset.Earliest
            }).Build();

        var sender = new ServiceBusClient("conn")
            .CreateSender("workflow-queue");

        consumer.Subscribe("orders-events");

        while (true)
        {
            var result = consumer.Consume();

            if (result.Message.Value.Contains("TriggerWorkflow"))
            {
                await sender.SendMessageAsync(
                    new ServiceBusMessage(result.Message.Value));
            }
        }
    }
}
```

### Component 3: Shared Contract example (for both sides)
_VERY IMPORTANT - use JSON (text-based format using key-value pairs) or Avro (binary encoding) and versioned schema_

```csharp
public class OrderEvent
{
    public string Id { get; set; }
    public string Type { get; set; }
    public DateTime CreatedAt { get; set; }
}
```

### Repository structure
```
kafka-servicebus-integration/
│
├── src/
│   ├── Producer.Api/
│   ├── SbToKafkaBridge/
│   ├── KafkaToSbBridge/
│   ├── Worker.Service/
│   ├── Contracts/
│
├── infrastructure/
│   ├── docker/
│   │   ├── docker-compose.yml
│   ├── bicep/
│   │   ├── main.bicep
│
├── k8s/
│   ├── producer.yaml
│   ├── bridges.yaml
│   ├── worker.yaml
│
├── build/
│   ├── azure-pipelines.yml
│
└── README.md
```

### Local infrastructure (Docker)
> `infrastructure/docker/docker-compose.yml`

```yaml
version: '3.8'

services:
  kafka:
    image: apache/kafka:latest
    ports:
      - "9092:9092"
    environment:
      KAFKA_NODE_ID: 1
      KAFKA_PROCESS_ROLES: broker,controller
      KAFKA_LISTENERS: PLAINTEXT://:9092
      KAFKA_CONTROLLER_QUORUM_VOTERS: 1@kafka:9093

  kafka-ui:
    image: provectuslabs/kafka-ui
    ports:
      - "8080:8080"
    environment:
      KAFKA_CLUSTERS_0_BOOTSTRAPSERVERS: kafka:9092
```

Run:
```Bash
docker-compose up -d
```
or directly:
```bash
docker run -d -p 9092:9092 apache/kafka
```

> Docker Compose is recommended in real setups with Zookeeper or KRaft mode.

### Shared contracts
> `src/Contracts/OrderEvent.cs`

```csharp
namespace Contracts;

public class OrderEvent
{
    public string Id { get; set; }
    public string Type { get; set; }
    public DateTime CreatedAt { get; set; }
}
```

### Kafka message headers
```csharp
new Message<string, string>
{
    Key = order.Id,
    Value = json,
    Headers = new Headers
    {
        { "traceId", Encoding.UTF8.GetBytes(traceId) }
    }
};
```

### Producer API (entry point)
> `Producer.Api/Program.cs`

```csharp  
var builder = WebApplication.CreateBuilder(args);

builder.Services.AddSingleton(new ServiceBusClient("<connection>"));
builder.Services.AddSingleton<IProducer<string, string>>(
    new ProducerBuilder<string, string>(
        new ProducerConfig { BootstrapServers = "localhost:9092" }
    ).Build());

var app = builder.Build();

app.MapPost("/order", async (OrderEvent order,
    ServiceBusClient sbClient,
    IProducer<string, string> kafka) =>
{
    var sender = sbClient.CreateSender("orders");

    var json = JsonSerializer.Serialize(order);

    // ❗ WARNING: This is a simplified example bellow.
    // In production, use the Outbox pattern to avoid dual-write inconsistency.

    // Add message metadata
    var msg = new ServiceBusMessage(json);
    msg.ApplicationProperties["eventType"] = "OrderCreated";
    msg.ApplicationProperties["traceId"] = traceId;

    // Send to Service Bus
    await sender.SendMessageAsync(msg);

    // Send to Kafka
    await kafka.ProduceAsync("orders-events",
        new Message<string, string>
        {
            Key = order.Id,
            Value = json
        });

    return Results.Ok();
});

app.Run();
```

### Service Bus → Kafka Bridge as BackgroundService
> `SbToKafkaBridge/Worker.cs`

```csharp
public class Worker : BackgroundService
{
    private readonly ServiceBusProcessor _processor;
    private readonly IProducer<string, string> _producer;

    public Worker(ServiceBusClient client)
    {
        _processor = client.CreateProcessor("orders");

        _producer = new ProducerBuilder<string, string>(
            new ProducerConfig { BootstrapServers = "kafka:9092" }
        ).Build();
    }

    protected override async Task ExecuteAsync(CancellationToken stoppingToken)
    {
        _processor.ProcessMessageAsync += async args =>
        {
            var body = args.Message.Body.ToString();

            await _producer.ProduceAsync("orders-events",
                new Message<string, string>
                {
                    Key = args.Message.MessageId,
                    Value = body
                });

            await args.CompleteMessageAsync(args.Message);
        };

        await _processor.StartProcessingAsync();
    }
}
```

### Kafka → Service Bus Bridge as BackgroundService
> `KafkaToSbBridge/Worker.cs`

```csharp
public class Worker : BackgroundService
{
    protected override async Task ExecuteAsync(CancellationToken stoppingToken)
    {
        var consumer = new ConsumerBuilder<string, string>(
            new ConsumerConfig
            {
                BootstrapServers = "kafka:9092",
                GroupId = "bridge",
                AutoOffsetReset = AutoOffsetReset.Earliest
            }).Build();

        consumer.Subscribe("orders-events");

        var sender = new ServiceBusClient("<connection>")
            .CreateSender("workflow");

        while (!stoppingToken.IsCancellationRequested)
        {
            try
            {
                var msg = consumer.Consume(stoppingToken);

                // process
                if (msg.Message.Value.Contains("Trigger"))
                {
                    await sender.SendMessageAsync(
                        new ServiceBusMessage(msg.Message.Value));
                }
            }
            catch (OperationCanceledException)
            {
                break;
            }
            catch (Exception ex)
            {
                // log + retry strategy
            }
        }
    }
}
```

### Worker Service (business logic)

```csharp
public class Worker : BackgroundService
{
    protected override async Task ExecuteAsync(CancellationToken stoppingToken)
    {
        // Process business logic from Service Bus
        // Example: order fulfillment
    }
}
```

### CI/CD (simplified)
> `build/azure-pipelines.yml`

```yaml
trigger:
- main

pool:
  vmImage: ubuntu-latest

steps:
- script: dotnet build
- script: dotnet publish -c Release
- script: docker build -t yourrepo/app .
```

### Kubernetes deployment
> `k8s/bridges.yaml`

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: bridges
spec:
  replicas: 3
  selector:
    matchLabels:
      app: bridges
  template:
    metadata:
      labels:
        app: bridges
    spec:
      containers:
      - name: sb-to-kafka
        image: yourrepo/sb-to-kafka:latest
      - name: kafka-to-sb
        image: yourrepo/kafka-to-sb:latest
```

**Autoscaling** (important)
```yaml
apiVersion: autoscaling/v2
kind: HorizontalPodAutoscaler
metadata:
  name: bridges-hpa
spec:
  scaleTargetRef:
    apiVersion: apps/v1
    kind: Deployment
    name: bridges
  minReplicas: 2
  maxReplicas: 10
```

### Azure infrastructure (Bicep)
> `infrastructure/bicep/main.bicep`

```bicep
resource sb 'Microsoft.ServiceBus/namespaces@2022-10-01' = {
  name: 'sb-namespace'
  location: resourceGroup().location
  sku: {
    name: 'Premium'
    tier: 'Premium'
    capacity: 4
  }
}
```
Extend with:
- queues
- topics
- subscriptions

### End-to-end flow
```
Client → API
        ↓
Service Bus (commands)
        ↓
SB → Kafka Bridge
        ↓
Kafka (event stream)
        ↓
Analytics / Consumers
        ↓
Kafka → SB Bridge
        ↓
Workflow Queue
        ↓
Workers
```

### Production must-haves

- ✔ Idempotency → Store processed message IDs
- ✔ Outbox pattern → Prevent dual-write failure
- ✔ Observability → OpenTelemetry → Correlation IDs
- ✔ Retry strategy → SB DLQ → Kafka retry topics


### Finally, a reminder in the example - Producer (Sending a message to Kafka)
```csharp
using Confluent.Kafka;
using System;
using System.Threading.Tasks;

class OrderProducer
{
    public static async Task Main(string[] args)
    {
        var config = new ProducerConfig
        {
            BootstrapServers = "localhost:9092"
        };

        using var producer = new ProducerBuilder<Null, string>(config).Build();

        for (int i = 1; i <= 5; i++)
        {
            var message = $"OrderCreated: Id={i}, Time={DateTime.UtcNow}";

            var result = await producer.ProduceAsync(
                "orders-topic",
                new Message<Null, string> { Value = message });

            Console.WriteLine($"Sent: {result.Value}");
        }
    }
}
```

### Finally, a reminder in the example - Consumer (Read messages from Kafka)
```csharp
using Confluent.Kafka;
using System;

class OrderConsumer
{
    public static void Main(string[] args)
    {
        var config = new ConsumerConfig
        {
            BootstrapServers = "localhost:9092",
            GroupId = "order-consumers",
            AutoOffsetReset = AutoOffsetReset.Earliest
        };

        using var consumer = new ConsumerBuilder<Ignore, string>(config).Build();
        consumer.Subscribe("orders-topic");

        Console.WriteLine("Listening for messages...");

        while (true)
        {
            var result = consumer.Consume();
            Console.WriteLine($"Received: {result.Message.Value}");
        }
    }
}
```

### Final architecture mindset

- **Service Bus** = command backbone → reliable command processing
- **Kafka** = event backbone (high-throughput, replayable data layer) → scalable event streaming
- **Bridges** = glue between worlds → controlled integration layer

- 👉 Don’t merge them
- 👉 Or replace one with the other
- 👉 Integrate them deliberately

### Final CD mindset

1) Run Docker (Kafka locally)
2) Deploy Service Bus in Azure
3) Run producer API
4) Run both bridges
5) Send test events (/order)
6) Watch flow in Kafka UI


## Scaling design - Kubernetes deployment suggestions

### Bridges
```replicas: 2–10```

### Kafka
- Partitions: 12–50
- Brokers: 3–6

### Service Bus
- Premium: 4–8 MU

### Autoscaling signals
- Kafka lag
- Service Bus queue length

## Schema
- Use schema evolution strategy (backward/forward compatibility).

## Observability 
_VERY IMPORTANT - don’t skip this_

Use:
- OpenTelemetry
- Track end-to-end latency across `SB → Kafka → consumers`
- Correlation ID across systems
```
message.ApplicationProperties["traceId"] = traceId;
```

Monitor:
- Kafka consumer lag
- Service Bus queue depth
- Dead Letter Queue, DLQ size

## Summary

### Common mistakes (avoid these)

- ❌ **Direct sync calls between systems** → Always async via messaging
- ❌ **No idempotency** → We WILL duplicate messages
- ❌ **Using Kafka like a queue** → It’s a log, not a task processor
- ❌ **Ignoring schema evolution** → Breaks consumers in production

**Avoid integration if:**
- System is small/simple
- No analytics/replay requirement
- Operational complexity is not justified
 
### Ordering considerations
- Kafka → ordered per partition
- Service Bus → use sessions if needed

👉 Use strongly typed messages

👉 Use same Kafka keys for partitioning:
```
Key = orderId
SessionId = orderId
```
👉 Add error handling

👉 In real apps:
- **Producer** → inside API
- **Consumer** → hosted service

This integration is ideal when:
- You need reliable workflows (Service Bus)
- AND high-scale event streaming (Kafka)
- AND multiple independent consumers

👉 This is exactly:
- Large retail
- Airports
- Smart infrastructure
- Industrial IoT

> [!IMPORTANT]
> **Service Bus ensures work is done.**
> 
> **Kafka ensures data is never lost.**

## See also:
- [Kafka & Service Bus — Part 1: Two Philosophies of Event-Driven Systems](./Kafka_and_ServiceBus_Part_1.md)
- [Kafka vs Service Bus Part 2: Real-world Architectures](./Kafka_and_ServiceBus_Part_2.md)
- [Azure Service Bus documentation](https://learn.microsoft.com/en-us/azure/service-bus/)
- [Apache Kafka documentation](https://kafka.apache.org/documentation/)
- [OpenTelemetry documentation](https://opentelemetry.io/docs/)
- [Azure Service Bus patterns](https://learn.microsoft.com/en-us/azure/architecture/patterns/category/messaging)
- [Outbox pattern reference](https://microservices.io/patterns/data/transactional-outbox.html)

- [Queues for Apache Kafka® Is Here: Your Guide to Getting Started in Confluent](https://www.confluent.io/blog/kafka-queue-semantics-share-consumer-ga/)
- [Confluent Blog](https://www.confluent.io)

- [Agile Vibe Coding Manifesto](https://agilevibecoding.org/)
- [Principles Behind the Agile Vibe Coding Manifesto - extended version](https://github.com/marekartur-dev/agilevibecoding/blob/main/Docs/Home/Principles.md)

- [Agile Vibe Coding](https://www.reddit.com/r/AgileVibeCoding/)
- [Marek Kubis - blog](https://github.com/marekartur-dev/agilevibecoding/tree/main)