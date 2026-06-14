# Governance

`opencode-scaffolding` uses a lightweight PR-first governance model inspired by `state-core` committee review, but the authority stays local to this repo.

## Default Flow

1. Work starts on a branch.
2. The branch opens a PR with scope, risk, evidence, and validation.
3. Committee reviews the PR as independent reviewers.
4. Host summarizes votes, blockers, and recommended decision.
5. Human decides merge/rework/close.

The committee reviews. It does not become the implementation runner.

## When Committee Review Is Required

Committee review is required before merging changes that affect:

- runner-neutral contract semantics under `contracts/`;
- OpenCode adapter assumptions under `adapters/opencode/`;
- MMF binding shape under `profiles/` or `examples/`;
- spike pass/fail interpretation that changes implementation direction;
- any rule that touches `state-core`, `mommy`, done-gate, canonical state, or session-local config boundaries.

Small typo fixes and evidence-file path corrections may use normal single-review PRs unless the host flags risk.

## State-Core-Inspired Rule

Use the `state-core` committee pattern as the shape, not as the owner:

- `approve|modify|reject` votes;
- explicit `veto: yes|no`;
- cited evidence and reason required;
- missing/crashed reviewers count as `non_vote`;
- Gate-style decisions need `>=3 approve` and `0 veto` unless the PR declares a stricter gate.

This repo must not write `state-core` governance records to approve its own changes. If a change requires a real `state-core` amendment, open that amendment in `state-core` separately.

## Files

- `committee-pr-contract.md` defines host/subagent/vote/quorum responsibilities.
- `../.github/pull_request_template.md` is the minimum PR packet.
