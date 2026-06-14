# Status And Roadmap

Date: 2026-06-14
Status: seeded, governance-ready, not implementation-ready

## Current Result

`opencode-scaffolding` is now the standalone home for OpenCode-adjacent capability scaffolding.

What is done:

- the old MMS-local scaffold has been moved into this repo;
- the repo has product boundaries in `AGENTS.md`;
- committee PR governance is defined under `governance/`;
- the first hook-fit spike evidence is preserved under `spikes/opencode-hook-fit/`;
- Phase 1 result is recorded as `partial_blocked`;
- the repo is pushed to GitHub on `main`.

What this means:

- this repo is ready for PR-based iteration;
- it is not ready for adapter MVP implementation yet;
- later work should be reviewed by committee through PRs.

## Main Direction

The desired shape is:

```text
MMF profile
  -> session-local capability pack
  -> role slots
  -> generated OpenCode agents/plugins/tools/skills
  -> adapter-owned bridge calls
  -> state-core remains canonical task-state and done-gate owner
```

This keeps the design resilient when OpenCode changes internally.

## What To Do Next

### 1. Narrow continuity strategy

Reason:

- Phase 1 did not prove durable restore-packet survival through `experimental.session.compacting`.
- OpenCode `1.15.13` returned 503 for `/api/session/{sessionID}/compact`.

Next decision:

- required path: external session-close / pickup artifact controlled by this repo's adapter layer or MMF launch layer;
- optional path: OpenCode compaction hook restore injection, enabled only after future proof.

Expected result:

- continuity no longer blocks on experimental compaction;
- compaction injection becomes an optimization, not contract truth.

### 2. Re-run boundary kill-switch checks

Scope:

- full-state subagent reject;
- custom tool cannot directly touch canonical state.

Expected result:

- a `pass|partial|fail` record for bounded dispatch;
- a `pass|partial|fail` record for custom-tool boundary;
- explicit decision on whether adapter MVP may start.

### 3. Align with MMF data-layer launch defaults

Scope:

- consume context/output/thinking/effort from latest-approved capabilities, model-policy, provider-profiles, and explicit launch overrides;
- avoid model-specific hardcoding in this repo;
- preserve WebUI as persistent config owner and TUI as launch-time override surface.

Expected result:

- OpenCode generated config follows MMS/MMF data truth;
- this repo does not create a parallel model registry;
- max output / 1M context / effort behavior is explainable from data sources.

See `docs/mmf-data-layer-alignment.md`.

### 4. Draft runner-neutral contracts

Scope:

- capability pack manifest shape;
- context projection contract;
- closeout/pickup artifact contract;
- adapter bridge contract.

Expected result:

- contracts survive OpenCode internal changes;
- MMF/OpenCode-specific details stay outside neutral contracts.

### 5. Draft OpenCode reference adapter MVP

Only start after the boundary checks are acceptable.

Scope:

- session-local plugin/tool/skill/agent generation;
- no global `~/.config/opencode/` writes;
- adapter-only state bridge;
- evidence-emitting launch/run artifacts.

Expected result:

- a minimal OpenCode session can hydrate bounded context, expose approved tools, dispatch bounded agents, and close through `state-core` done-gate without patching OpenCode core.

## What Not To Do Yet

- Do not patch OpenCode core.
- Do not create a new public MMS mode.
- Do not write real `~/.config/mms/**`.
- Do not write global `~/.config/opencode/**`.
- Do not let custom tools read or write canonical state directly.
- Do not treat `experimental.session.compacting` as stable continuity truth.
- Do not start adapter MVP before the boundary checks and MMF data-layer alignment are recorded.

## PR And Committee Rule

After the initial seed:

- every substantive iteration should use a branch and PR;
- committee reviews the PR diff and evidence;
- host summarizes votes and blockers;
- human decides merge/rework/close.

Gate-style committee review uses:

- at least 3 true votes;
- at least 3 `approve`;
- 0 veto;
- missing/crashed reviewers count as `non_vote`.

See `governance/committee-pr-contract.md`.

## Current Evidence

- Spike outline: `spikes/opencode-hook-fit/README.md`
- Evidence template: `spikes/opencode-hook-fit/EVIDENCE_TEMPLATE.md`
- Phase 1 evidence: `spikes/opencode-hook-fit/PHASE1_EVIDENCE.md`
- Phase 1 result: `spikes/opencode-hook-fit/RESULT.md`

Current Phase 1 verdict:

- kill-switch A, compaction restore packet: `partial`
- kill-switch B, boundary negative tests: `not_run`
- overall: `partial_blocked`

## Done Definition For This Repo Seed

The seed is complete when:

- repo exists and is pushed;
- old MMS-local scaffold is no longer the source of truth;
- governance says future iterations use PR + committee review;
- spike evidence records why adapter MVP should not start yet;
- next work is explicit and bounded.

This seed meets those conditions.
