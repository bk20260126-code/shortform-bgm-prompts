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

## Duration: most generators default to a full song, not a short clip

Song generators (Suno, Udio, Google Lyria/Flow Music) are built around producing a complete track with a verse/chorus structure. That shapes two separate problems, and they need separate fixes:

1. **Some generators have a hard floor above your target.** Udio's creation-time options are only 32 seconds or 2:10 — nothing in between, and nothing shorter, without a separate "Extend" step afterward. Lyria 3.5's documented range starts at 30 seconds.
2. **Duration is usually a UI/API parameter, not something the prompt text controls.** Writing "length: 20 seconds" in the prompt paragraph does not reliably set the output length — the generator needs the actual duration control (a slider, a dropdown, an API field) set explicitly. Text-only duration requests routinely get ignored, and the generator falls back to its own default, which tends to be much longer (in practice, a 2:49–2:59 track came back from a Flow Music session that only stated a duration in prose, never used a length control).

| Generator | Duration mechanism | Practical floor for a direct short generation |
|---|---|---|
| Google Lyria / Flow Music | UI/API duration control (Lyria 3.5: 30s-3min range) | ~30s if the control is actually set; unset defaults to a full song length |
| Udio | Fixed creation-time presets: 32s or 2:10 | 32s, and only exactly 32s — nothing shorter, nothing in between |
| Suno (v5.5+) | Duration slider, 10s-6min in 5s steps | 10s, but only if the slider is used — pre-slider or slider-unset behavior defaults toward a full song and frequently undershoots or overshoots an unstated target |
| ElevenLabs Music API | `music_length_ms` parameter, 3s-10min, or `Auto` | 3s technically, but the API's own docs recommend generating in ~30s sections and building up rather than requesting one long or very short span directly |
| Mubert, Soundraw | Purpose-built for loop/background music; explicit length selection is the normal path | Reliable at whatever short length you need — this is their actual design target, unlike the song generators above |

**Default posture: plan for "generate a full-length track, then extract or trim your section" rather than assuming a song generator will hand you a tight loop.** Two ways to act on this:

- If the target generator exposes an explicit duration control (slider, dropdown, API field), set it there — don't rely on prompt-text duration language to do that job. Prompt text can still state the target as a secondary hint, but the control is what actually works.
- If it doesn't, or you're not sure it will be respected, write the prompt assuming a longer, structured output (e.g. verse/hook, per `scene-to-cue-system.md`'s scene-to-cue map or the default arc) and plan a trim/extract pass afterward to pull your actual short segment out — usually the opening hook, since that's the section already written to "start immediately." Say so explicitly in the project's `MUSIC-PROMPT.md` so the trim step isn't a surprise later in the workflow.

Either way: always measure the real output length before treating a duration request as satisfied. Don't assume the prompt or even the UI control produced exactly what was asked — generators commonly land close but not exact.

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
- Google DeepMind / Google blog — Lyria 3.5 launch coverage confirming the 30-second-to-3-minute duration range in Flow Music (2026-07).
- Udio Help Center — "Create Your First Song" (official; confirms the 32s/2:10 creation-time presets and that other lengths require the Extend feature) and guidance-tags documentation (negative-prompt syntax).
- Suno release notes / Duration Slider documentation — confirms the 2026-07-20 introduction of explicit 10s-6min duration control, and that pre-slider behavior did not reliably hit an unstated target length.
- ElevenLabs — official Eleven Music documentation (`music_length_ms` parameter range and the platform's own recommendation to build tracks from ~30s sections rather than requesting one long span directly).
- This plugin's own prior session logs — direct observation that a Flow Music generation with a duration stated only in prompt prose (not a UI/API control) returned a 2:49-2:59 length track instead of the ~20s requested.
- Community consensus drawn from AI-music-generation forums and prompt-sharing threads (instrument count, genre specificity, regeneration-rate observations, Mubert/Soundraw duration-control behavior). Treat as directional, not authoritative — it's aggregated secondhand, not a controlled study.
