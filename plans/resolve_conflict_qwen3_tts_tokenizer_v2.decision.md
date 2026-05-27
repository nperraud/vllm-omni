# Decision

## Review verdict: LGTM

## Action
Adopt upstream change: remove `"inputs_embeds": inputs_embeds` from the initial `mask_kwargs` dict (line ~570).

Rationale: The signature-based inspect check below already handles both API variants dynamically. Retaining the line would potentially leave a stale `inputs_embeds` key when `create_causal_mask` expects `input_embeds` (without 's'). Matches original omni main and upstream.
