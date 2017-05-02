---
name: maximum-throughput
description: Split independent engineering work into concurrent lanes with clear contracts and mergeable handoffs. Use when a task can be explored, reviewed, tested, or implemented in parallel.
---

# Maximum Throughput

When work can run independently, split it early. The goal is not more agents; it is shorter feedback loops with clean synthesis.

## Decide What Can Split

Good split points:

- Different subsystems or directories.
- Independent hypotheses for a bug.
- Separate review lenses: correctness, security, performance, tests.
- One shared interface with independent implementations.
- Gathering once, then parallel reasoning over the same context.

Bad split points:

- Two tasks that constantly need each other's intermediate decisions.
- Multiple agents editing the same small file.
- A single causal chain where the next step depends on the previous result.
- Tiny tasks where coordination costs more than the work.

## Delegation Modes

Blank-slate:

- Use for independent exploration or scoped implementation.
- Include all required context in the prompt: paths, constraints, commands, expected output.
- Ask for concrete handoff fields: files read, findings, risks, tests, next steps.

Shared-context:

- Gather expensive context once.
- Write it to a local file or concise note.
- Send parallel lanes to reason over that same context from different angles.
- Merge findings by theme, not by which lane produced them.

## Contracts

Before parallel implementation, define the boundary:

- Data shape or interface.
- Ownership of files.
- Test command each lane should run.
- What counts as done.
- Handoff format.

If no contract exists, create one before dispatching work.

## Synthesis

- Deduplicate repeated findings.
- Treat independently confirmed findings as higher confidence.
- Call out conflicts directly.
- Check that independently produced changes compose into a valid build.
- Run the narrowest validation that covers the merged result.

## Handoff Format

Ask each lane to return:

```text
Summary:
Files touched/read:
Key findings:
Risks:
Validation:
Follow-ups:
```

For review tasks, include severity and exact file/symbol references. For implementation tasks, include test output and any skipped checks.

## Guardrails

- Do not parallelize destructive or stateful operations against the same resource.
- Do not hide uncertainty during synthesis.
- Do not merge code from parallel lanes without reading the diff.
- Prefer a smaller number of well-separated lanes over broad, redundant fan-out.
