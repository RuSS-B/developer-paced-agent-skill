# Developer-Paced Implementation

A Codex skill for working in small, developer-approved batches without getting ahead of the developer's understanding or decisions.

## Principles

- Keep investigation, planning, implementation, and verification in small reviewable steps.
- Ask for approval between batches and stop after each completed batch.
- Keep questions, plans, and checkpoints concise.
- Preserve the developer's mental model and control over scope.

## Installation

Install with the `skills` CLI and select the target agent and scope when prompted:

```sh
npx skills add RuSS-B/developer-paced-agent-skill
```

Alternatively, from the repository root, copy the skill into the Codex skills directory:

```sh
mkdir -p "${CODEX_HOME:-$HOME/.codex}/skills"
cp -R skills/developer-paced-implementation "${CODEX_HOME:-$HOME/.codex}/skills/"
```
