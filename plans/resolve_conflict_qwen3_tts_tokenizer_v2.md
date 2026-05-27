# Plan: Resolve merge conflict in modeling_qwen3_tts_tokenizer_v2.py

## Conflict File
`vllm_omni/model_executor/models/qwen3_tts/tokenizer_12hz/modeling_qwen3_tts_tokenizer_v2.py`

## Conflict Location
Lines 569-572 — inside the `mask_kwargs` initialization dict:

```
<<<<<<< HEAD
                "inputs_embeds": inputs_embeds,
=======
>>>>>>> 753508fe025c3f897a729cd7cef3817e12f06a5f
```

## Analysis
- **HEAD** added `"inputs_embeds": inputs_embeds` to the initial `mask_kwargs` dict.
- **Upstream** removed it.
- **Below the conflict** (lines 578-583), there's already a robust signature-based check:
  ```python
  sig = inspect.signature(create_causal_mask)
  if "input_embeds" in sig.parameters:
      mask_kwargs["input_embeds"] = inputs_embeds
      mask_kwargs["cache_position"] = cache_position
  else:
      mask_kwargs["inputs_embeds"] = inputs_embeds
  ```
- Having `"inputs_embeds"` in the initial dict AND then potentially adding `"input_embeds"` (different key) would leave **both** keys in the dict if `create_causal_mask` expects `input_embeds`.
- The upstream approach is correct: let the signature check decide which key to add.
- The original omni main branch also does NOT have this line.

## Resolution
Adopt the upstream change — **remove** the `"inputs_embeds": inputs_embeds` line from the initial `mask_kwargs` dict.

## Verification
1. No remaining `<<<<<<<`, `=======`, `>>>>>>>` markers.
2. Valid Python syntax.
3. Imports unchanged.
4. Run `git add` on the resolved file.
