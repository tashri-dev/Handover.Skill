# Diagram conventions (Mermaid)

Use fenced ```mermaid blocks (render natively in Obsidian). Derive every diagram from code you actually read; never invent participants, methods or edges. If unsure, mark the element `(unverified)` and list it under Open questions.

## Which diagram for what
| Need | Type |
|---|---|
| Request/event flow across services, auth, retries | `sequenceDiagram` |
| Domain model, key types, relationships, inheritance | `classDiagram` |
| Business/process logic, branching, deploy or failure paths | `flowchart TD` |
| Service topology, dependencies, data stores | `flowchart LR` (add subgraphs per boundary) |

## Rules
- One idea per diagram; split when over ~12 nodes/participants.
- Name nodes with real identifiers from the code (service, class, endpoint). Add a caption line above each diagram saying what it shows and the source files (`path:line`).
- Sequence: show sync vs async (`->>` vs `-->>`), include the failure/timeout path for critical calls, use `autonumber`.
- Class: only the types that matter to the explanation; show key fields/methods, not everything.
- Flowchart: label decision edges; use distinct shapes for start/end/decision/store.
- Avoid characters that break Mermaid in labels (quote labels containing parentheses, brackets, colons).
- Minimum set per system/service group: 1 topology flowchart, 1 sequence for the main happy path, 1 sequence or flowchart for the main failure path, 1 class diagram for the core domain.
