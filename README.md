# Description

"Clipper", a dark-theme code snippet manager with three screens (Home, Clip Detail, New Clip). You'll have a wire up of a design MCP (Pencil or Figma), use it to design each screen, then use the same MCP to generate React Native code in a pre-wired Expo app. Finally, you'll use /chrome to screenshot the running app and compare it against the design.

What you'll see:
* How to install and verify an MCP server with claude mcp add and /mcp
* How to use a design MCP to drive a real design-to-code pipeline
* How /chrome closes the visual validation loop
* The agentic engineering pattern: MCPs as sensors, hooks as guardrails, /chrome as feedback


# Design to Code with MCPs

Use MCPs to run the full design-to-code pipeline for **Clipper** — a dark-theme code snippet manager.

## Projects

| Directory | Purpose |
|-----------|---------|
| [`pencil-design/`](./pencil-design/) | Design phase — use Pencil or Figma MCP to create the three screens |
| [`clipper-expo/`](./clipper-expo/) | Code phase — implement the screens in a pre-wired Expo app |

## What You'll Do

1. **Read the spec** — `pencil-design/PRD.md` describes all three screens and design tokens
2. **Design with an MCP** — wire up Pencil or Figma MCP and design Home, Clip Detail, and New Clip
3. **Build the Expo app** — use the design MCP to generate React Native screens in `clipper-expo/`
4. **Validate with /chrome** — run `npx expo start --web` and screenshot the app against the spec

## Getting Started

```bash
cd pencil-design
claude
/assignment_4
```

The `/assignment_4` wizard walks you through all four parts.
