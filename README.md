# Superpowers for Cowork

Structured workflows for knowledge work. The superpowers methodology — stop before acting, plan before executing, verify before declaring done — adapted from software development to everything you do in Cowork.

## Installation

### Cowork

Install from the Cowork plugin UI:

1. Open **Customize** in the sidebar, then **Plugins**.
2. Install from a marketplace, or upload this plugin as a file package.
3. Open the installed plugin and review its skills.

If this plugin is published in a Git repository marketplace, add that repository as a marketplace first, then install `superpowers-cowork` from the plugin browser.

### Claude Code / local development

Load this checkout directly from disk:

```bash
claude --plugin-dir /path/to/superpowers-cowork
```

Install-by-name only works after `superpowers-cowork` is available in one of your configured Claude plugin marketplaces.

## Commands

| Command | What it does |
|---------|-------------|
| `/superpowers` | Full pipeline — clarify → plan → execute → verify → deliver |
| `/plan` | Break a complex task into structured steps with checkpoints |
| `/research` | Define questions, search, evaluate sources, synthesize findings |
| `/review` | Audit an artifact against its goals before sharing |
| `/deliver` | Pick format, verify completeness, send/save/schedule |
| `/brief` | Pull context from email + calendar + docs into a coherent picture |
| `/draft` | Managed drafting with iteration and feedback loops |

## How it works

Every command follows the same methodology:

1. **Clarify** — What are we doing, for whom, and what does "done" look like?
2. **Plan** — Break it into steps with tools, checkpoints, and success criteria
3. **Execute** — Do the work, using subagents in parallel where possible
4. **Verify** — Check the output against the success criteria defined in step 1
5. **Deliver** — Pick the right format and destination, confirm sensitivity, ship it

Commands enter the pipeline at different points. `/superpowers` runs the full thing. `/review` enters at Verify. `/deliver` enters at Deliver.

## Philosophy

- **Verification-driven work** — Define what "done" looks like before you start. Check against it when you finish. The TDD principle, generalized.
- **Checkpoint with the human** — At every phase transition, stop and confirm. The human can redirect, skip ahead, or abort.
- **Tool-aware execution** — The plugin knows which MCP connectors you have and chooses the right tool for each step.
- **Stays out of the way** — Nothing auto-triggers. If you don't invoke a command, you don't get the methodology.

## Connectors

The plugin works standalone, but gets better with connected tools. See [CONNECTORS.md](CONNECTORS.md) for details.
