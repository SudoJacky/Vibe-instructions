# GPT-5.6 Sol Effort Level Guide

Choose the lowest level that can reliably handle the task.

<img width="629" height="339" alt="image" src="https://github.com/user-attachments/assets/af9ff397-366a-4f8d-b1a0-20bddee4e2c2" />

<img width="374" height="207" alt="image" src="https://github.com/user-attachments/assets/5c0ec5dc-9bb9-48b1-8d18-7d4c6557e9c9" />

| Level | Best for | Examples |
|---|---|---|
| **Light / Low** | Simple, fast tasks | Rewriting text, simple questions, small code edits |
| **Medium** | Most everyday work | Writing functions, explaining code, routine analysis |
| **High** | Complex tasks requiring careful reasoning | Debugging, multi-file changes, technical planning |
| **Extra High (`xhigh`)** | Difficult, long-running single-agent work | Architecture design, complex refactoring, deep investigation |
| **Max** | The hardest problems that need extensive reasoning and verification | Exploring multiple solutions, difficult root-cause analysis, repeated testing and revision |
| **Ultra** | Large tasks that can be divided into parallel workstreams | Repository-wide migrations, research from multiple angles, implementation plus independent review |

## Practical recommendations

- Start with **Medium** for most tasks.
- Use **High** when correctness matters or the task has several dependent steps.
- Use **Extra High** for difficult work that requires sustained reasoning.
- Use **Max** when a single agent should spend more time exploring, checking, and revising its approach.
- Use **Ultra** when the task can benefit from multiple agents working in parallel.

## Max vs. Ultra

- **Max** gives a single agent more time and compute than Extra High.
- **Ultra** coordinates multiple agents—four by default—to work on parallel subproblems and combine their results.

Use **Max** for problems that are difficult but tightly connected. Use **Ultra** for problems that can be cleanly divided into independent parts.

## Quick rule of thumb

```text
Simple task                         → Light
Normal daily work                  → Medium
Complex task                       → High
Very difficult single-agent task   → Extra High
Maximum single-agent effort        → Max
Parallel multi-agent task          → Ultra
```
