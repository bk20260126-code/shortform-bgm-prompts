# Prompt writing best practices

How to phrase a music prompt well, once you know what the video needs (see `scene-to-cue-system.md` for that part). Based on official generator documentation and cross-checked community practice, current as of 2026-08.

## Core structure

Most generators respond best to one natural-language paragraph in this order, not a comma-separated tag dump:

1. **Genre — name an era, not just a genre word.** "2020s electro-pop" outperforms "electro-pop." A bare genre word gets interpreted arbitrarily and is the single biggest cause of needing 3+ regenerations (community analysis of a large sample of prompt threads put this around 70% of first attempts).
2. **Mood — emotion plus a reference frame.** "Bright" alone gives the generator no direction. "Bright but composed, like a well-run system in a good mood" gives it something to aim at.
3. **Instruments — name 2-4, specifically.** Five or more produces a cluttered result; naming one or none lets the generator default to whatever it wants.
4. **Tempo — numeric BPM by default.** Numeric BPM (even an approximate range) changed output quality more than any other single variable in community testing. Pair with a natural-language phrase if the target generator's own docs favor that style (Google's Lyria examples do — e.g. "fast, energetic pace with a driving beat" — but a numeric BPM alongside it doesn't hurt).
5. **Vocals — state "instrumental" as one word, don't just negate.** This is the officially documented method for Google's Lyria models. Relying only on a "no vocals" negative is weaker.
6. **Start point — use timestamp prompting if the generator supports it.** `[00:00] begin immediately, no intro` forces the opening rather than hoping the instruction is followed.

## Negative prompting support varies by generator

| Generator | Negative prompt support | Method |
|---|---|---|
| Google Lyria (and products built on it) | Not documented as a separate field | Fold every constraint into the main prompt as a positive statement ("Instrumental." instead of "no vocals") |
| Udio | Documented, official | `[no: distortion, noise]` or `--no vocals` syntax |
| Suno | Community-documented, not official | Treat "what to exclude" as a distinct fourth section of the prompt structure |

Check this table before reusing a negative-prompt block across generators — a block written for Udio's `--no` syntax does nothing on a generator that doesn't parse it, and vice versa.

## Duration is a hint, not a guarantee

Official specs sometimes fix track length regardless of what the prompt asks for (e.g. one Lyria tier is documented as producing a fixed ~30-second track; a higher tier goes up to 3 minutes). A prompt asking for "20 seconds" may be silently ignored, with the actual short loop needing a separate trim/edit step after generation. Always measure the real output length before treating a duration request as satisfied — don't assume the prompt controlled it.

## Chat-session tools can silently turn "new candidate" into "edit"

Chat-style generation interfaces tend to interpret a follow-up message in the same session as an edit of the last track, not an independent new generation. This breaks the one-variable-at-a-time candidate testing method in `scene-to-cue-system.md`, because an "edited" result isn't an independent parallel candidate — it inherited the previous generation's underlying audio.

- Start a new session per candidate when independence matters.
- If a follow-up in the same session produced an edit instead, log it in the candidate table as an edit of the earlier candidate, not as its own independent A/B/C entry.
- Watch for the generator's own UI language ("Replace," "trimmed and arranged," etc.) confirming it treated the message as an edit — that's the signal to check, not an assumption to make in advance.

## Iteration discipline

If the first result disappoints, don't stack on more adjectives. Change exactly one variable that plausibly caused the failure (tempo, one instrument, or the mood reference frame) and regenerate. Changing several things at once destroys the ability to know what actually fixed — or broke — the result.

## Rights check before every real generation

Confirm commercial-use terms in the generator's own terms of service before submitting a real (non-draft) generation, and before uploading any reference audio. If a reference file's origin is unclear or unverified (an unlabeled download, an unknown source), ask the human to confirm they hold the rights to it before uploading — a generator's "I have the necessary rights" consent dialog is a factual claim only a human can make, not something to click through on their behalf.

## Sources

- Google Cloud — official Lyria prompting guide (structure, "instrumental" keyword convention, timestamp prompting, documented track-length tiers). Verify current specifics against the live docs before relying on exact numbers — model versions and limits change.
- Udio Help Center — official guidance-tags documentation (negative-prompt syntax).
- Community consensus drawn from AI-music-generation forums and prompt-sharing threads (instrument count, genre specificity, regeneration-rate observations). Treat as directional, not authoritative — it's aggregated secondhand, not a controlled study.
