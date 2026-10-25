# System Architecture

A screenshot of the **Software Dev Team** flow on the Flowise Agentflow canvas.

![Flowise canvas for the Software Dev Team flow](Screenshot%202026-10-23%20203102.png)

## Nodes on the canvas

| Node | Type | Notes |
|---|---|---|
| Start | Start | Receives the user's feature request |
| Product Manager | Agent | Model: `qwen2.5-coder:1.5b` |
| Architect | Agent | Model: `qwen2.5-coder:1.5b` |
| Developer | Agent | Model: `qwen2.5-coder:1.5b` |
| Tester | Agent | Model: `qwen2.5-coder:1.5b` |
| Round Counter | Custom Function | Adds 1 to `round` |
| Condition 0 | Condition | Hard stop on the round limit |
| Supervisor | Condition Agent | Model: `qwen2.5-coder:1.5b`, three outputs (0, 1, 2) |
| Direct Reply 0 | Direct Reply | Sends the final package to the chat |
| Loop 0 | Loop | Sends control back for another pass |
| Loop 1 | Loop | Sends control back for another pass |

## How control flows

- The main line runs Product Manager, Architect, Developer, Tester, then Round Counter.
- **Condition 0** checks the round count. One branch goes straight to Direct Reply; the other goes to the Supervisor.
- The **Supervisor** has three outputs: output 0 goes to Direct Reply, output 1 goes to Loop 0, and output 2 goes to Loop 1. The prompt's scenarios map to these as finish, fix the code, and redo the design.
- The two Loop nodes are what turn the straight line into a feedback cycle.

For the same flow drawn as a text diagram, and the list of Flow State variables, see the [main README](../README.md#architecture).
