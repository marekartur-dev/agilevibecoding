# Agile Vibe Coding versions with example - Scenario 2

<img src="./../Images/disciplined_engineering.jpg" alt="Agile flow with good suggestions" height="400"  style="float: right; margin-left: 10px;">

## The disciplined engineering

> “Developers propose generating, then verifying step by step.”

Agile Vibe Coding process is:
- iterative → aligned with Agile
- structured → not chaotic
- controlled → not blind generation

> The real problem is unbounded generation without enforced understanding not “vibe coding”. ❗
 
Call it whatever you want — that is the failure mode.

**Let’s reframe Agile models more precisely.**

3 practical ways to develop today:

### 1. ❌ Blind AI generation
- fast
- dangerous
- low understanding

### 2. 🟡 Traditional manual coding
- high control
- slower
- predictable

### 3. ✅ Controlled AI collaboration - 2nd generation vibe coding
- fast and controlled
- iterative
- understanding preserved

___👉 This is the optimal balance.___

### The uncomfortable truth

The process “constraint-driven AI coding” described as:
> generate → verify → refine
is **cognitively demanding**.

It requires:
- discipline
- patience
- engineering maturity


## A realistic C#, Azure-style change and walkthrough - Scenario 2

> Let’s consolidate everything into a realistic, end-to-end mini flow, 
> then compare the bad and good suggestions to see exactly where issues arise and how to prevent them.

### PART 1 — Clean mini architecture: Azure → Service Bus → Kubernetes

We model the system as follows:
- Azure Function → receives notification
- Service Bus → transports message
- Kubernetes app (.NET) → processes it

#### Architecture (clean boundaries)
```
[Azure Function]
    ↓ (HTTP trigger)
[Application Handler]
    ↓
[Service Bus Publisher]
    ↓
-------------------------
[Service Bus Queue]
-------------------------
    ↓
[Kubernetes Worker]
    ↓
[Application Handler]
```

🔹 1. Azure Function (thin entry point)

👉 No business logic here.
```
public class NotificationFunction
{
    private readonly INotificationHandler _handler;

    public NotificationFunction(INotificationHandler handler)
    {
        _handler = handler;
    }

    [Function("NotificationFunction")]
    public async Task Run(
        [HttpTrigger(AuthorizationLevel.Function, "post")] HttpRequest req)
    {
        var notification = await DeserializeAsync(req);
        await _handler.HandleWithRetryAsync(notification);
    }
}
```

🔹 2. Application Layer (pure logic)
```
public class NotificationHandler : INotificationHandler
{
    private readonly IMessagePublisher _publisher;

    public NotificationHandler(IMessagePublisher publisher)
    {
        _publisher = publisher;
    }

    public async Task HandleAsync(Notification notification)
    {
        Validate(notification);
        await _publisher.PublishAsync(notification);
    }

    public Task HandleWithRetryAsync(Notification notification)
    {
        return RetryPolicy.ExecuteAsync(() => HandleAsync(notification));
    }

    private void Validate(Notification notification)
    {
        if (notification == null)
            throw new ArgumentNullException(nameof(notification));

        if (string.IsNullOrEmpty(notification.Type))
            throw new ArgumentException("Type required");
    }
}
```

🔹 3. Retry Policy (isolated)
```
public static class RetryPolicy
{
    public static async Task ExecuteAsync(Func<Task> action)
    {
        const int attempts = 3;

        for (int i = 0; i < attempts; i++)
        {
            try
            {
                await action();
                return;
            }
            catch when (i < attempts - 1)
            {
                await Task.Delay(500);
            }
        }

        throw new Exception("Retry failed");
    }
}
```

🔹 4. Service Bus Publisher (infrastructure)
```
public class ServiceBusPublisher : IMessagePublisher
{
    private readonly ServiceBusSender _sender;

    public ServiceBusPublisher(ServiceBusClient client)
    {
        _sender = client.CreateSender("notifications");
    }

    public async Task PublishAsync(Notification notification)
    {
        var message = new ServiceBusMessage(JsonSerializer.Serialize(notification));
        await _sender.SendMessageAsync(message);
    }
}
```

🔹 5. Kubernetes Worker (consumer)
```
public class Worker : BackgroundService
{
    private readonly ServiceBusProcessor _processor;
    private readonly INotificationHandler _handler;

    public Worker(ServiceBusClient client, INotificationHandler handler)
    {
        _handler = handler;
        _processor = client.CreateProcessor("notifications");
    }

    protected override Task ExecuteAsync(CancellationToken stoppingToken)
    {
        _processor.ProcessMessageAsync += HandleMessage;
        _processor.ProcessErrorAsync += HandleError;

        return _processor.StartProcessingAsync(stoppingToken);
    }

    private async Task HandleMessage(ProcessMessageEventArgs args)
    {
        var notification = JsonSerializer.Deserialize<Notification>(args.Message.Body);
        await _handler.HandleAsync(notification);

        await args.CompleteMessageAsync(args.Message);
    }

    private Task HandleError(ProcessErrorEventArgs args)
    {
        // log only
        return Task.CompletedTask;
    }
}
```

#### ✅ Why this is correct
- Azure Function = entry only
- Handler = pure logic
- Retry = separate concern
- Service Bus = infrastructure only
- Kubernetes = consumer, not mixed with transport logic

👉 Each component is understandable in isolation.

### PART 2 — Bad vs Good Prompts (Side-by-Side)

Now the critical comparison.

#### ❌ BAD PROMPT
Create an Azure Function that receives notifications, validates them,
retries on failure, logs everything, and sends to Service Bus.
Also include a consumer service.

#### ❌ BAD OUTPUT (typical AI)
```
public async Task Run(HttpRequest req)
{
    try
    {
        var body = await new StreamReader(req.Body).ReadToEndAsync();
        var notification = JsonSerializer.Deserialize<Notification>(body);

        if (notification == null || string.IsNullOrEmpty(notification.Type))
        {
            _logger.LogError("Invalid notification");
            return;
        }

        int retries = 3;

        while (retries > 0)
        {
            try
            {
                var message = new ServiceBusMessage(body);
                await _client.SendMessageAsync(message);

                _logger.LogInformation("Sent");
                return;
            }
            catch (Exception ex)
            {
                retries--;
                _logger.LogWarning(ex, "Retrying...");
                await Task.Delay(1000);
            }
        }
    }
    catch (Exception ex)
    {
        _logger.LogError(ex, "Failed");
    }
}
```

What is wrong:
- All responsibilities combined in one place
- No separation of concerns
- Hard to test
- Implicit retry policy
- Azure + business logic tightly coupled

👉 This is the long-term maintenance risk.

#### ✅ GOOD PROMPT (structured)
Context:
Azure Function receives HTTP notifications and forwards them to Service Bus.

Goal:
Implement ONLY the Azure Function entry point.

Constraints:
- No business logic
- No retry logic
- No Service Bus code
- Only call an injected handler

Output:
Single class, max 30 lines

#### ✅ GOOD OUTPUT
```
public class NotificationFunction
{
    private readonly INotificationHandler _handler;

    public NotificationFunction(INotificationHandler handler)
    {
        _handler = handler;
    }

    [Function("NotificationFunction")]
    public async Task Run(HttpRequest req)
    {
        var notification = await DeserializeAsync(req);
        await _handler.HandleWithRetryAsync(notification);
    }
}
```

#### Next Prompt (layering)
Implement the NotificationHandler.

Constraints:
- No Azure SDK
- No logging
- No retry logic
- Only validation + publishing via interface

✅ Output
```
public class NotificationHandler
{
    private readonly IMessagePublisher _publisher;

    public NotificationHandler(IMessagePublisher publisher)
    {
        _publisher = publisher;
    }

    public async Task HandleAsync(Notification notification)
    {
        if (notification == null)
            throw new ArgumentNullException(nameof(notification));

        await _publisher.PublishAsync(notification);
    }
}
```

#### Side-by-side insight
<table>
<tr>
    <th>Aspect</th>
    <th>❌ Bad Prompt</th>
    <th>✅ Good Prompt</th>
</tr>
<tr>
    <td>Scope</td>
    <td>Everything at once</td>
    <td>One responsibility</td>
</tr>
<tr>
    <td>Output</td>
    <td>Large, tangled</td>
    <td>Small, clean</td>
</tr>
<tr>
    <td>Debugging</td>
    <td>Hard</td>
    <td>Easy</td>
</tr>
<tr>
    <td>Ownership</td>
    <td>AI</td>
    <td>Developer</td>
</tr>
<tr>
    <td>Architecture</td>
    <td>Implicit</td>
    <td>Explicit</td>
</tr>
</table>

#### The deeper insight

The difference is NOT:
- AI quality
- model intelligence

> 👉 It is prompt structure controlling complexity

### What elite developers are starting to do 

They build personal prompt playbooks, like:
- “core logic prompt”
- “refactor prompt”
- “test-first prompt”
- “anti-bloat prompt”

> 👉 Reusable, consistent, predictable


## Final mental model

Think of AI like a very fast junior developer who:
- writes quickly
- overcomplicates things
- needs strict guidance

> 💬 _The one sentence that matters_: **“If a prompt contains multiple responsibilities, the resulting code will conceal multiple problems”.**.

## See also:
- [Agile Vibe Coding versions with example - Scenario 1](https://www.linkedin.com/pulse/agile-vibe-coding-versions-example-scenario-1-marek-kubis-9lq5e/)
 
- [Is there a need to change the way software is developed today?](https://www.linkedin.com/pulse/need-change-way-software-developed-today-marek-kubis-dntie)
- [This Isn’t Rebranding. It’s a Structural Shift in Software Development](https://www.linkedin.com/pulse/isnt-rebranding-its-structural-shift-software-marek-kubis-sanpe)
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