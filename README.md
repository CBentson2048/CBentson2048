# CHRISTOPHER BENTSON

**Senior AI & Full-Stack Engineer · Lead Software Engineer**

I build AI-enabled products across web, mobile, and backend services. I lead technical design and stay hands-on through implementation, testing, and release.

## Engineering experience

- **AI systems.** I’ve built workflows that coordinate multiple tools, moved model providers behind a common interface, and written evaluations for routing decisions and provider behavior.
- **Full-stack engineering.** My TypeScript work spans React web apps, React Native mobile clients, NestJS APIs, and PostgreSQL. I’ve unified design tokens across web and mobile, built typed API integrations, and written database migrations.
- **Technical leadership.** I turn open-ended requirements into a design, a sequence of reviewable changes, and rollout criteria. For an AI routing change, I added a shadow mode to evaluate the new router on real inputs before enabling it.

## Shared context for AI agents

Imagine fixing a duplicate webhook update with one AI tool. The next morning, a teammate opens another tool and asks why the handler now checks event IDs. Their agent has the diff and needs the reasoning from the original session.

I’d build that handoff with replayable sync and a retrieval service shared by the CLI and MCP server. The service would enforce session permissions before ranking results and link each answer to the message that supports it. I’d evaluate whether the agent finds the right decision and completes the task, then track time to resolution and cost per completed task.

<details>
<summary>Read an example handoff</summary>

**Why does the webhook handler deduplicate by event ID?**

The first delivery committed an update before the sender timed out. Its retry carried the same event ID and applied the update again. The team kept retries for transient failures and made repeat deliveries safe. Before changing the handler, reproduce a timeout after the first write and confirm that the retry leaves exactly one committed update. [Delivery trace](#delivery-trace) · [Decision note](#decision-note)

### Delivery trace

```text
09:12:10  evt_42  First delivery commits the update
09:12:12  evt_42  Sender times out waiting for a response
09:12:13  evt_42  Retry commits the same update again
```

### Decision note

> Keep retries for transient failures. Store the event ID and the update in one database transaction. Acknowledge repeat deliveries of an already processed event without applying the update again.

</details>

[Browse my GitHub repositories](https://github.com/CBentson2048?tab=repositories)
