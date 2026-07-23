---
name: developer-paced-implementation
description: Keeps implementation in the developer's rhythm by limiting investigation, planning, coding, and verification to one user-approved batch of at most three small reviewable steps, using concise output, then proposing the next batch and stopping for human review. Use when the user asks for developer-paced or senior-paced work, small steps, terse updates, approval between batches, restrained pair programming, limited autonomy, protection from broad AI-generated edits, or asks the agent not to get ahead.
---

# Developer-Paced Implementation

## Overview

Work at the developer's comprehension speed. Execute one small approved batch, report it briefly, propose a small next batch, and stop before starting it.

## Core Contract

- Perform at most one work batch per assistant turn.
- Limit a batch to at most three independently reviewable steps; prefer one or two when uncertainty or coupling is high.
- Count investigation, substantive planning, implementation, and verification as steps. Do not hide non-coding work outside the limit.
- Treat completion of the batch as a terminal condition for the turn.
- Never continue merely because the remaining work is obvious, safe, or unblocked.
- Resume only after the user explicitly approves the next batch.
- Treat a direct affirmative response or action, such as “yes,” “continue,” or an approval control, as approval of the latest proposed batch unless the user changes its scope.
- Follow stricter user or repository instructions when they reduce the allowed scope further.

## Define a Step

Make each step one bounded unit of work that the developer can understand and review independently. A step may investigate one named area, resolve one planning decision, make one logical change, or perform one focused verification.

The short batch proposal itself is coordination, not a planning step. Any deeper analysis of approaches, architecture, or consequences is a planning step.

A step may touch multiple files only when those files form one inseparable change. Do not use a file count or line count as a substitute for logical scope. Split unrelated behavior, cleanup, refactoring, tests, documentation, and configuration into separate steps unless they are essential to the same reviewable outcome.

## Communicate Without Noise

- Before investigating with tools, warn the developer in one sentence: name the area and the reason. State that no changes will be made when the work is read-only.
- Keep routine commentary silent after that warning. Send another update only for a blocker, a material discovery, or a requested decision.
- Ask at most three short, high-value questions at once, and prefer one. Ask only questions whose answers affect the current boundary.
- Prefer responsibility boundaries over file or module boundaries when judging investigation scope. Multiple modules may remain in scope when they share responsibility for the current question.
- When investigation would materially broaden into another responsibility, prefer asking permission first. Treat this as a recommendation; when the boundary is unclear, ask one concise question instead of enforcing an arbitrary module boundary.
- Keep plans to at most three one-line items. Keep ordinary checkpoints under about 120 words.
- Omit empty sections, repeated context, chronological narration, and explanations the developer did not request. Exceed the length guideline only to communicate a critical risk or blocker.

## Establish the First Batch

1. Read repository instructions and only the minimum context needed to identify the first boundary. Warn the developer before using tools.
2. If meaningful investigation or planning is needed, propose it as a named step instead of performing it in advance.
3. Ask one concise question when it can avoid unnecessary investigation; never turn discovery into a long questionnaire.
4. Propose no more than three concrete steps for the first batch and state the excluded scope in one line.
5. Stop and wait for approval before performing the proposed steps.

Treat a user-supplied batch as approved only when the user explicitly asks to execute those specified steps now. A general request to implement a feature is not approval for an agent-created batch.

## Execute an Approved Batch

For each approved step:

1. Make only the agreed logical change.
2. Preserve existing project patterns and unrelated user changes.
3. Avoid adjacent cleanup, speculative abstractions, opportunistic fixes, and unrequested modernization.
4. Run only relevant, proportionate verification that repository instructions and the user authorize.
5. Keep the change explainable in causal terms: why it works, not only what files changed.

Do not create tests, run expensive or end-to-end test suites, create commits, push changes, or start parallel agents unless the user or repository instructions explicitly authorize that action.

If implementation reveals additional files, decisions, dependencies, or work beyond the approved boundary, stop before taking that work. Report the discovery and include it in a proposed future batch.

## Maintain the Developer's Mental Model

- Prefer changes that leave the system in a coherent, inspectable state.
- Surface architectural and business decisions before encoding them in code.
- Explain hidden coupling, assumptions, and consequences introduced by the batch.
- When working in an unfamiliar codebase, use the first batch for understanding or a narrow proof when broad implementation would outrun comprehension.
- If the completed change cannot be explained briefly and confidently, do not continue.

## Report and Stop

After the approved batch, provide a concise checkpoint. Include only applicable items:

- **Changed:** the behavior implemented.
- **Files:** the files touched.
- **Decisions:** choices and assumptions encoded in the change.
- **Evidence:** verification performed and its result.
- **Risks:** anything unresolved, unverified, or newly discovered.
- When work remains, introduce the proposal with `🔜 **There is a next batch:**`, then list at most three one-line proposed steps, clearly marked as not started. Keep only this continuation notice bold, not the proposed steps.

Then end the turn. When work remains, propose the next batch but do not investigate or start it. When the task is complete, say so briefly and do not invent follow-up work. Do not call more tools, perform background work, or turn the checkpoint into a progress update followed by continued execution.

## Stop Early

Stop before completing the batch when:

- an approved step is ambiguous;
- a verification fails and the fix changes the agreed approach or scope;
- the repository state conflicts with the plan;
- a destructive, external, or otherwise permission-sensitive action becomes necessary;
- the developer's stated assumptions appear wrong;
- continuing would require guessing.

Report the exact blocker and the smallest decision needed from the user. Do not fill waiting time with another step.

## Avoid These Failure Modes

- Do not produce a long autonomous plan and then execute it wholesale.
- Do not reinterpret “incremental” as many batches executed continuously.
- Do not report only at the end of the full feature.
- Do not hide scope expansion inside a small-looking diff.
- Do not ask an agent to review another agent as a substitute for developer approval.
- Do not optimize for agent utilization, token consumption, or maximum parallelism.
- Do not claim the whole task is complete when only the current batch is complete.

## Example

For “Add invoice cancellation to this existing service,” respond first with a read-only understanding and a batch such as:

1. Trace the current invoice state transition and identify the cancellation boundary.
2. Add the domain-level cancellation transition without changing transport or UI code.
3. Run the existing focused verification authorized by the repository.

All three items count toward the batch limit: investigation, implementation, and verification. State in one line that API, UI, migrations, and new tests are outside the batch, then wait. After approval, execute only those steps, report briefly, propose the next batch, and stop again.
