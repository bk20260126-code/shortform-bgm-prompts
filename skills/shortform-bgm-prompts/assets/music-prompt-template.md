---
status: PROMPT_DRAFT
project: "[MISSING]"
version: 1
platform: "[MISSING]"
audience: "[MISSING]"
duration_seconds: "[MISSING]"
has_vo_or_tts: "[MISSING]"
research_refreshed_at: "[MISSING]"
target_generator: "[MISSING]"
generator_model: "[MISSING]"
generator_seed: "[MISSING]"
music_rights_status: "[VERIFY]"
prompt_sha256: "[GENERATE_AFTER_FINALIZATION]"
selected_audio: "[NOT_GENERATED]"
---

# Music Prompt — [Project]

## Input contract

- One message:
- Desired emotion:
- Undesired styles:
- Caption density:
- Reward/reveal moments:
- Loop requirement:
- Output audio path:

## Scene-to-cue map

Skip this table for a standalone/library track with no fixed video timeline — keep every other section.

| Time | Visual event | Viewer state | Caption load | Musical cue | Proof/loop role |
|---|---|---|---|---|---|
| 0.0– | | | | | |

## Music specification

- Genre:
- BPM / meter:
- Key / harmony:
- Core instruments:
- Rhythm:
- Hook motif:
- Mobile-speaker requirement:

## Final generation prompt

```text
[FINAL PROMPT]
```

## Negative prompt

Only fill this in if the target generator supports a separate negative-prompt field or syntax (see `references/prompt-best-practices.md`). Otherwise fold every exclusion into the final prompt above as a positive statement and leave this section marked N/A.

```text
[NEGATIVE PROMPT OR "N/A — folded into final prompt"]
```

## Evidence and claim boundaries

| Source | Verified date | Supported claim | Does not prove |
|---|---|---|---|
| | | | |

## Candidate log

| Candidate | Generator/model/seed | Changed variable | File | Result | Rejection/selection reason |
|---|---|---|---|---|---|
| A | | baseline | | `[NOT_GENERATED]` | |
| B | | one variable only | | `[NOT_GENERATED]` | |
| C | | one variable only | | `[NOT_GENERATED]` | |

## Listening QA

- [ ] Sound starts at 0.0s with a distinguishable event in the first second
- [ ] Intended change lands within ±0.25s of each scene boundary
- [ ] Music isn't too dense where captions are heavy
- [ ] Reward/proof moment reads clearly
- [ ] Fits platform and audience; no excluded style leaked in
- [ ] Passes a phone-speaker check
- [ ] Loop or ending behaves as intended
- [ ] Publish/commercial-use terms confirmed
- [ ] Duration, codec, sample rate confirmed
- [ ] Final in-context preview approved separately from the raw audio

## Feedback and mutation

- T+24:
- T+72:
- Repeated drop point:
- User edit:
- Failed condition:
- Prompt mutation candidate:
- Rollback condition:
