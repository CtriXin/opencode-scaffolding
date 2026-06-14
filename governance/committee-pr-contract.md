# Committee PR Contract

> Status: active local governance for `opencode-scaffolding`.
> Inspiration: `state-core` committee quorum and evidence discipline.
> Boundary: this does not modify `state-core`; it only governs this repo's PR flow.

## Position

Committee is a PR review mechanism for OpenCode capability scaffolding.

- `opencode-scaffolding` owns runner-neutral OpenCode capability scaffolding.
- `state-core` owns canonical task-state, schema, validation, and done-gate.
- MMS/MMF owns launch/profile/session-local attachment.
- OpenCode remains the host runner and must not be patched here.
- Host frames the PR review and summarizes decisions.
- Subagents produce independent review votes.
- Human remains final merge authority.

## Host Responsibilities

The host must:

- define review scope and the exact merge question;
- provide the PR diff, evidence paths, validation output, and relevant rules to each reviewer;
- separate deterministic checks from judgment calls;
- collect all votes or mark unavailable reviewers as `non_vote`;
- preserve dissent and caveats instead of smoothing them into consensus;
- identify any veto or requested change before positive summary;
- produce a short decision packet for the human.

The host must not:

- edit reviewer votes after the fact;
- count missing reviewers as approvals;
- claim quorum without enough true votes;
- hide a veto behind an aggregate score;
- merge or close the PR unless the human explicitly delegates that action.

## Subagent Responsibilities

A subagent may:

- review the PR diff, docs, spike evidence, and validation logs;
- cite concrete files, paths, command output, or repo rules;
- vote `approve`, `modify`, or `reject`;
- set `veto: yes` for invariant-breaking blockers;
- recommend narrow follow-up checks.

A subagent must not:

- mutate the branch while voting;
- coordinate its vote with other subagents before submitting;
- approve undocumented OpenCode internals as stable contract truth;
- approve direct canonical-state reads/writes by adapters;
- approve writes to real `~/.config/mms/**` or global `~/.config/opencode/**`;
- turn brainstorm agreement into implementation approval.

## Vote Format

Each vote should be durable, either as a PR review comment or a file attached to the review packet:

```yaml
model: <reviewer-id>
verdict: approve | modify | reject
veto: yes | no
confidence: high | medium | low
cited:
  - <file, rule, command, or evidence path>
reason: <short reason>
required_changes:
  - <only for modify/reject/veto>
```

Meanings:

- `approve`: no blocking issue for this PR scope.
- `modify`: merge only after listed changes are made or explicitly accepted by the human.
- `reject`: direction or implementation is not acceptable for the stated scope.
- `veto: yes`: hard blocker; PR cannot be treated as passed until resolved or human-overridden after risk is shown.
- `non_vote`: reviewer unavailable, timed out, or returned malformed output.

## Quorum

Gate-style PR decisions require:

- at least 3 true votes;
- at least 3 `approve` votes;
- 0 veto;
- no unresolved `modify` requirements that touch the merge scope.

If quorum is not met, the host must request more review or mark the PR as not ready. Do not downgrade quorum failure into a pass.

## Evidence Discipline

Review statements should be tagged or worded as:

- `executed`: the reviewer/host ran the check;
- `inspected`: the reviewer/host read the file, diff, or artifact;
- `source-backed`: backed by cited docs/evidence;
- `inferred`: reasoned from evidence but not directly observed;
- `unverified`: not checked.

Deterministic checks such as `git diff --check`, schema validation, or spike scripts should be run by the host/CI and cited. Committee judgment does not replace executable validation.

## Hard Boundaries

A PR must not pass committee review if it:

- patches OpenCode core as the default path;
- makes OpenCode internals part of runner-neutral contracts;
- lets adapters read/write canonical state directly;
- bypasses `state-core` done-gate;
- writes real MMS or OpenCode global config;
- exposes API keys, OAuth state, account identity, or real-home auth material;
- creates a public MMS mode without explicit human direction;
- treats `experimental.session.compacting` as stable continuity truth without fresh evidence.

## Host Decision Packet

The host summary should include:

- PR link/branch and reviewed scope;
- vote table: reviewer, verdict, veto, confidence;
- blocking findings first;
- required changes before merge;
- validation commands and results;
- remaining risk;
- recommended human decision.
