# CBentson2048

**The next agent should know how we got here.**

Imagine a developer and an AI agent debugging a webhook that updates a record twice. They reproduce the failure and ship a fix. The next morning, a teammate opens a different AI tool and asks why the handler now checks event IDs. The new agent has to infer the reason from the diff.

I want to build the infrastructure for that handoff. When work moves between people and tools, the next session should surface what the team tried, why it made a decision, and where to verify it.

## Where I’d start

- **Make sync recoverable.** Save progress as sessions are imported from each tool. Key records by source and session ID so a retry updates the same record.
- **Give every answer a source.** Apply sharing permissions before ranking results. Use the same retrieval service for the CLI and MCP server, and point each answer to the relevant message in the original session.
- **Measure whether the handoff worked.** Use tasks with known decisions to test whether agents recover the right evidence and complete the task. Track time to resolution and cost per completed task.

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
