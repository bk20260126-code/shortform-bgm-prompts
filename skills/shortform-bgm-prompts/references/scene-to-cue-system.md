# Scene-to-cue system

The method for turning a video's scene timing and the state the viewer should feel into musical variables a generator can act on, plus the checklist that catches the ways this goes wrong.

## Failure-path audit

Each row is a failure this method exists to prevent. When a generated track disappoints, check which row it matches before writing a new prompt — most failures trace back to one of these causes, not to needing a fancier adjective.

| Failure | Causal path | Guard |
|---|---|---|
| Generic "upbeat BGM" | No scene timing in the prompt → one undifferentiated block of music | Every scene boundary and cue goes into the prompt as an explicit timecode |
| First second is empty | Generator defaults to a long intro/fade-in | State "start immediately," "no intro," "no fade-in" — in the prompt body if the generator has no separate negative-prompt field, or in both prompt and negative prompt if it does |
| Captions are tiring to read | Melody/high-end/rhythm density competes with on-screen text | Lower melodic density in caption-dense scenes; ask for a clean midrange and steady loudness |
| Payoff moment falls flat | No exact reveal timecode → the musical accent lands at a random point | Give each reward/reveal an accent type and an exact time range |
| Overly childish result | "Gamified" got translated as generic 8-bit/arcade | Name the actual audience and say "adult," "not childish," "restrained retro accent" explicitly |
| Music-cognition research inflated into a reach claim | Tempo/syncopation/groove research → assumed Instagram/TikTok reach increase | Keep a claim-boundary table per source; anything not directly demonstrated is `[INFERENCE]`, not fact |
| Lesson lost between projects | Prompt only ever existed in chat | Keep the project's `MUSIC-PROMPT.md` (from `assets/music-prompt-template.md`) plus a shared mutation log updated together |
| Rights status unknown at generation time | Generator/plan/region terms never checked | Confirm generator, plan, and commercial-use status before the actual generation call — not after |
| A 20-second track request comes back 2-3 minutes long | Duration stated only in prompt text, not set via the generator's actual duration control; most song generators default toward a full song | Check `generator_duration_control` before writing the final prompt; set the real control if one exists, otherwise plan a trim/extract pass — see `prompt-best-practices.md` |

## Scene-to-cue conversion

For every scene, fix these six fields in one row:

| Field | Question | Musical variable |
|---|---|---|
| Time | When does the scene start/end? | Cue boundary |
| Visual event | What changes on screen? | Accent or arrangement change |
| Viewer state | What should the viewer feel? | Harmony / timbre / energy |
| Caption load | How much text is there to read? | Melodic/rhythmic density |
| Proof | What moment should stick in memory? | Hit / chime / dropout |
| Loop role | How does this reconnect to the opening? | Pickup / motif reprise / clean loop |

A default arc, useful as a starting point and not a rule to force onto every video:

```text
0-2s hook: motif starts immediately, no intro
problem: keep the pulse, thin arrangement
mechanism: add groove and a regular accent
reward: harmonic lift and a short chime
insight/CTA: lower melodic density, resolve cleanly, loop pickup
```

## Prompt contract

A finished prompt needs all of the following before it goes to a generator:

1. Exact duration and purpose
2. 3-6 named emotions, plus what to avoid
3. Genre, BPM, meter, harmony
4. Core instruments and a phone-speaker requirement
5. First-frame start condition
6. Per-scene timecodes and musical events (skip if there is no fixed timeline — see below)
7. A caption-density spacing rule
8. Ending/loop condition
9. Production requirements
10. A negative prompt, if the target generator supports one (see `prompt-best-practices.md`)

**No fixed timeline (standalone/library track):** drop the per-scene timecode table, but keep provenance, rights status, candidate feedback, and approval boundaries. A loop bed that gets reused across many unrelated clips still needs a clean loop condition and an explicit rights check — it just doesn't need scene timing.

## Candidate generation gate

Generate at least 3 candidates from one core prompt, changing exactly one variable per candidate:

```text
A: baseline
B: change only tempo or rhythmic density
C: change only timbre or the hook motif
```

Record model/version/seed/prompt hash/length/file path for each. If a result is bad, don't add more adjectives — change the one condition that plausibly failed.

## Listening QA

| Gate | Pass condition |
|---|---|
| Hook | Sound from 0.0s; a distinguishable motif/event in the first second |
| Timing | Intended change lands within ±0.25s of the scene boundary |
| Caption space | Lead/high-end/fills aren't too dense where captions are heavy |
| Reward | Payoff/proof moment reads clearly, without overselling |
| Tone | Fits the platform and audience; no excluded style leaked in |
| Mobile | Kick/bass survive phone speakers; nothing piercing in the highs |
| Loop | Ending doesn't cut abruptly; reconnects to the opening motif |
| Rights | Publish/commercial-use terms for this generator, plan, and region are confirmed |
| Technical | File exists; duration/codec/sample rate match what was requested |

If there's voiceover/TTS, check intelligibility in preview, not just on a meter. Don't promote a fixed dB number into a universal rule — mix targets vary by platform and content.

## Feedback loop

Only record T+24 / T+72 signals once a track has actually been published, using whatever the platform's real metrics are (watch time, retention, saves, shares, comments) — and record the rejected candidates' specific rejection reasons alongside them.

Don't decide a BGM's causal effect from a single post's performance. Promote a repeated drop-point or candidate-selection pattern to a mutation candidate at 2+ occurrences, and only change a default after 3+ occurrences or a clear A/B result.

## Evidence policy

Priority order when sourcing claims for a prompt or a mutation decision:

1. Platform's own documentation (feature/measurement claims)
2. Peer-reviewed music-cognition research (tempo, harmony, syncopation, groove)
3. The target generator's official documentation (prompt syntax, length control, commercial-use terms)
4. Creator/marketing writing — ideas only, never promoted to the source of a numeric claim

Re-check platform documentation if it's more than ~90 days old or the feature might have changed. Never use a music-cognition trend as direct proof of increased platform reach — cite it only for what it actually measured.
