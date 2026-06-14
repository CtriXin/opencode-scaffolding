# MMF Data-Layer Alignment

Date: 2026-06-14
Status: design constraint for future PRs

## Why This Exists

Recent MMF OpenCode work makes one boundary explicit:

`opencode-scaffolding` should not hardcode model context, output, thinking, effort, or multimodal capability facts.

Those facts belong to the MMS/MMF data layer and should be consumed by generated OpenCode config or session-local adapter material.

## Source Order

When generating OpenCode-facing material, prefer this source order:

1. latest-approved capability bundle;
2. model-policy / WebUI-approved model capability facts;
3. provider-profiles;
4. per-runtime explicit launch override;
5. conservative fallback constants only when no approved source exists.

Fallback constants are not product truth. They are guardrails for missing data.

## Context And Output Limits

Do not add model-specific context/output hardcoding in this repo.

Examples of forbidden patterns:

- GLM/Kimi/MiniMax context constants in adapter code;
- `[1m]` suffix parsing as official model identity;
- model-name lists that imply output limits;
- OpenCode-specific defaults that override approved capability facts.

Expected pattern:

- approved model capabilities expose `context_window_tokens` and output limits;
- provider profiles describe protocol-specific parameter names and quirks;
- runtime launch overrides may set explicit `max_output_tokens` for one launch;
- OpenCode generation consumes those values without redefining model truth.

## Thinking And Effort

OpenCode-facing `options` / `variants` should be generated from runtime/provider/profile data.

Rules:

- TUI/WebUI launch choices such as `thinking_mode` and `reasoning_effort` may flow into OpenCode config when the provider profile supports them.
- Env-only controls for other CLIs must not leak into OpenCode `options` / `variants`.
- Effort names are compatibility data, not agent roster facts.
- If a model or provider lacks an approved effort mapping, leave it inherited/disabled rather than inventing one.

## Launch Defaults Shape

The preferred future UI shape is a unified `launch.defaults` surface, not scattered per-CLI fields.

Recommended fields:

- `thinking_mode`: `enable | disable | inherit`
- `reasoning_effort`: `low | medium | high | xhigh | inherit`
- `bypass`: `true | false | inherit`
- `bypass_thinking_mode`: `enable | disable | inherit`
- `bypass_reasoning_effort`: `low | medium | high | xhigh | inherit`
- `caveman_mode`: `enable | disable | inherit`
- `caveman_level`: `light | standard | full | inherit`
- `nsr_mode`: `enable | disable | inherit`
- `agent_pack`: `none | ecc | omc | inherit`
- `claude_1m_mode`: `auto | enable | disable | inherit`
- `disabled_session_surfaces`: `skills | mcp | hooks`
- `opencode.default_profile`: `agent | review | committee | omo | raw`

## Bypass Defaults

Bypass should not imply one hardcoded thinking behavior.

Use two layers:

1. normal launch defaults;
2. bypass-specific overrides.

If bypass-specific fields are unset, inherit normal launch defaults.

This supports both patterns:

- normal: thinking enabled, effort high; bypass: thinking enabled, effort xhigh;
- normal: project defaults; bypass: inherit model/provider behavior.

## WebUI And TUI Roles

WebUI should own persistent defaults:

- preferences;
- preview DB;
- model-policy;
- provider profiles;
- approved model capabilities.

TUI should own launch-time confirmation:

- read the default values;
- allow one-shot override for this launch;
- write back only when the user explicitly chooses to remember the value.

## Impact On This Repo

Future `opencode-scaffolding` PRs should treat MMS/MMF data-layer values as inputs and should not create a parallel model registry.

Adapter work should record which revisions it consumed when possible:

- route revision;
- policy revision;
- profile revision;
- capability revision;
- explicit launch overrides.

## Acceptance Criteria For Future Adapter PRs

A PR that generates OpenCode config should show:

- where context/output limits came from;
- where thinking/effort options came from;
- whether any fallback constant was used;
- that no model-specific hardcoding was added to contracts or adapter code;
- that session-local generated config does not write global OpenCode or MMS config.
