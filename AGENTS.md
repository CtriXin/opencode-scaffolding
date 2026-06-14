# AGENTS.md - opencode-scaffolding

This repo is the source of truth for OpenCode-adjacent capability scaffolding.

## Product Boundary

- Do not fork or patch OpenCode core here.
- Prefer official OpenCode surfaces: plugins, custom tools, skills, agents, permissions, and documented/observed hooks.
- Keep contracts runner-neutral; OpenCode-specific assumptions belong under `adapters/opencode/` or `spikes/`.
- Keep MMF binding session-local. Do not write real `~/.config/mms/**` or global `~/.config/opencode/**` from this repo.
- `state-core` remains the canonical task-state and done-gate owner; adapters must not read or write canonical state files directly.

## Contribution Flow

- The initial seed may land on `main`.
- All later iterations should use a branch and PR.
- Follow `governance/committee-pr-contract.md` for committee-reviewed PRs.
- Committee review is for PR review and decision support, not for mutating files directly.
- A host may summarize committee votes, but the host must preserve dissent and evidence quality.

## Evidence Rules

- Spikes must record pass/partial/fail using durable evidence paths.
- Do not upgrade partial evidence to pass.
- Keep raw artifacts under ignored `spikes/**/artifacts/` paths unless explicitly curated.
- Do not store secrets, OAuth state, global account files, or real-home auth material.
