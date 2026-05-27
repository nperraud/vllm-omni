# Plan Review — resolve_conflict_qwen3_tts_tokenizer_v2

- **Verdict**: lgtm
- **Confidence**: high
- **Fallback**: False

## Summary
The plan correctly resolves the merge conflict by adopting the upstream removal of the `"inputs_embeds": inputs_embeds` line, because the existing signature-based inspection below already handles both key variants dynamically. Keeping the line would cause a stale key if the expected parameter is `input_embeds` (without 's'). The change is minimal, aligns with upstream and original omni main branch, and verification steps are appropriate.

## Do Not Do
- Do not reintroduce the removed line or any other duplicate key assignments to `mask_kwargs` before the signature check.