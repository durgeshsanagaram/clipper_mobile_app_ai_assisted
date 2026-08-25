## Architecture

**Clipper** is a dark-themed code snippet manager for mobile, built to demonstrate a full design-to-code pipeline driven by a design MCP. A PRD defines three screens; Pencil (or Figma) MCP produces the designs; Claude Code generates the Expo/React Native implementation; and `/chrome` closes the loop by screenshotting the running app and comparing it back to the spec.

```mermaid
flowchart TD
    subgraph Spec["1 · Specify"]
        PRD["📄 pencil-design/PRD.md<br/>screen descriptions + design tokens"]
    end

    subgraph Design["2 · Design with MCP"]
        PencilMCP["🎨 Pencil MCP<br/>or Figma MCP<br/>claude mcp add · verify with /mcp"]
        Screens["Three screens<br/>Home · Clip Detail · New Clip"]
        PencilMCP --> Screens
    end

    subgraph Build["3 · Build"]
        CLI["Claude Code CLI<br/>agent loop + hooks as guardrails"]
        Expo["📱 clipper-expo/<br/>React Native + Expo screens"]
        CLI --> Expo
    end

    subgraph Validate["4 · Validate visually"]
        Web["Expo web build<br/>running app"]
        Chrome["🔍 /chrome + Playwright<br/>screenshot the running app"]
        Diff["Compare against PRD spec"]
        Web --> Chrome --> Diff
    end

    PRD --> PencilMCP
    Screens -->|"design tokens, layout, components"| CLI
    PRD -.->|"acceptance criteria"| Diff
    Expo --> Web
    Diff -->|"mismatch → refine design or code"| CLI
    Diff -->|"spec drift → update the design"| PencilMCP
    Diff -->|"✅ match"| Done["Shipped Clipper app"]

    classDef spec fill:#fef9c3,stroke:#ca8a04,color:#3b2f04
    classDef design fill:#f3e8ff,stroke:#8b5cf6,color:#1e1b4b
    classDef build fill:#dcfce7,stroke:#16a34a,color:#052e16
    classDef check fill:#e0f2fe,stroke:#0284c7,color:#0c243b
    classDef out fill:#ffe4e6,stroke:#e11d48,color:#4c0519

    class PRD spec
    class PencilMCP,Screens design
    class CLI,Expo build
    class Web,Chrome,Diff check
    class Done out
```

### The agentic engineering pattern

```mermaid
flowchart LR
    Sensors["🛰️ MCPs as sensors<br/>Pencil / Figma read the design"]
    Agent["🤖 Claude Code<br/>plans and writes code"]
    Guards["🛡️ Hooks as guardrails<br/>enforce checks on tool calls"]
    Feedback["🔁 /chrome as feedback<br/>screenshots close the loop"]

    Sensors --> Agent
    Guards --> Agent
    Agent --> Feedback
    Feedback -->|"what the app actually looks like"| Agent

    classDef s fill:#f3e8ff,stroke:#8b5cf6,color:#1e1b4b
    classDef a fill:#dcfce7,stroke:#16a34a,color:#052e16
    classDef g fill:#fef9c3,stroke:#ca8a04,color:#3b2f04
    classDef f fill:#e0f2fe,stroke:#0284c7,color:#0c243b
    class Sensors s
    class Agent a
    class Guards g
    class Feedback f
```

### Pipeline at a glance

| Phase | Input | Tool | Output |
| --- | --- | --- | --- |
| 1 · Specify | Product intent | `pencil-design/PRD.md` | Screen specs + design tokens |
| 2 · Design | PRD | Pencil MCP / Figma MCP | Home, Clip Detail, New Clip |
| 3 · Build | Design data via MCP | Claude Code CLI | `clipper-expo/` React Native screens |
| 4 · Validate | Running web build | `/chrome` + Playwright | Screenshot diff vs. spec → iterate |

### Repository layout

```
pencil-design/     Design phase — PRD.md and the Pencil/Figma MCP work
clipper-expo/      Code phase — pre-wired Expo app
```

Run `/repo` directory  from `pencil-design/` to launch the interactive wizard.
