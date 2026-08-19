---
name: shortform-bgm-prompts
description: Writes AI music-generation prompts for short-form vertical video background music (Instagram Reels, YouTube Shorts, TikTok). Use when the user asks to write a BGM/music prompt for a reel or short, generate background music for a video with Suno/Udio/Lyria/other AI music tools, map a video's scene timing to musical cues, fix background music that doesn't match caption timing or has a bad intro/loop, or run a listening QA pass on generated candidate tracks.
---

# Short-form BGM prompt system

Turn a video's scene timing and the state the viewer should feel into a precise, generator-ready music prompt — instead of asking a generator for "upbeat background music" and re-rolling until something sticks.

## When this applies

- The video is silent/caption-driven, or has voiceover the music must not compete with.
- Background music carries rhythm, curiosity, or payoff instead of (or alongside) narration.
- A storyboard or shot list with rough timing already exists, or can be sketched in one message.

Generating music, inserting it into the video, re-rendering, and publishing are separate steps. Approving a prompt does not auto-approve the steps after it — always stop after producing the prompt and confirm before spending a generation credit or publishing anywhere.

## Workflow

### 1. Collect the mandatory intake

Do not guess any of these. Leave a field explicitly unset (`[MISSING]`) rather than inventing a plausible-sounding value — a wrong guess here produces a prompt that looks complete but is quietly wrong.

```yaml
project:
platform: # Reels / Shorts / TikTok / other
duration_seconds:
scene_boundaries: # timecodes, even rough ones
caption_density_by_scene: # none / light / heavy, per scene
reward_or_reveal_moments: # timecodes where something pays off
has_vo_or_tts: # yes/no — if yes, music must leave room for it
desired_emotion: # 3-6 words, not one
undesired_styles: # explicit exclusions, e.g. "not childish", "no dramatic strings"
target_generator: # Suno / Udio / Google Lyria / other
generator_duration_control: # does it have an explicit length UI/API control, or text-prompt only? see prompt-best-practices.md
music_rights_status: # confirm before real generation, see below
output_audio_path:
```

`target_generator` and `music_rights_status` can stay unset while drafting the prompt, but resolve both before the user actually submits a generation. Resolve `generator_duration_control` before writing the final prompt — it changes whether the prompt should target the real short duration directly or be structured for a trim/extract pass (see step 3).

### 2. Build the scene-to-cue map

For each scene, fix six fields in one row: time boundary, visual event, the state the viewer should be in, caption load, the proof/payoff moment, and how the ending reconnects to the loop. This table is what turns a vague mood request into a musical instruction a generator can act on. Full method and a default arc (hook / problem / mechanism / reward / resolution) in [`references/scene-to-cue-system.md`](references/scene-to-cue-system.md).

If there is no fixed video timeline — a general-purpose loop bed, not tied to one edit — skip the scene-to-cue table but keep every other intake field, especially the loop requirement and rights status.

### 3. Write the prompt

Read [`references/prompt-best-practices.md`](references/prompt-best-practices.md) before writing the actual sentences. It covers the structure most generators respond to, which generators support negative prompting and which don't, and a chat-session gotcha that silently turns a "new candidate" request into an edit of the last track.

**Check the target generator's actual duration mechanism first — most song generators (Suno, Udio, Lyria/Flow Music) default toward a full song, not a short clip, and several can't go below ~30 seconds at all.** If the generator exposes an explicit duration control (slider, dropdown, API field), plan to set it there, not just in prompt text. If it doesn't, or the account/session isn't confirmed to honor it, write the prompt for a longer structured piece (verse/hook, per `scene-to-cue-system.md`) with the actual target section — usually the opening hook — as the part meant to be trimmed out afterward, and say so in the artifact so the trim step isn't a surprise later.

Copy [`assets/music-prompt-template.md`](assets/music-prompt-template.md) to the project as `MUSIC-PROMPT.md` and fill it in — this is the artifact of record, not the chat transcript. Every prompt needs: exact duration and purpose, the generator's duration mechanism and whether a trim pass is planned, 3-6 named emotions plus explicit exclusions, genre/BPM/key, 2-4 named instruments, a first-frame start condition, per-scene timecodes (if applicable), a caption-density rule, an ending/loop condition, and production requirements. Keep candidate log, evidence, and QA sections in the same file — a prompt that only exists in chat gets lost and re-researched from scratch next time.

### 4. Generate candidates, one variable at a time

Generate at least three candidates from the same core prompt, changing exactly one variable per candidate (tempo, one instrument, or the hook motif — never more than one). Record generator, model, seed, duration, and file path for each in the candidate log. If a candidate fails, do not pile on more adjectives — change the one variable that plausibly caused the failure and try again.

Before submitting any real generation, confirm with the user if the generator's plan has a limited quota or the account isn't confirmed as commercial-use-cleared — spending someone's last credit on an untested prompt is a bad trade.

### 5. Run listening QA

Check every candidate against the checklist in [`references/scene-to-cue-system.md`](references/scene-to-cue-system.md) before calling it approved: hook (audible from 0.0s), timing (scene changes land within ±0.25s), caption space, reward clarity, tone fit, mobile-speaker translation, clean loop/ending, rights, and technical specs (duration/codec/sample rate match what was requested).

### 6. Close the loop

Record failed conditions and rejection reasons so the next prompt starts from what was already learned, not from zero. If a post-publish performance signal exists, log it against the specific musical variable it might explain — never promote a single post's performance into a permanent rule; wait for a repeated pattern (2+ occurrences) before treating it as a real signal.

## Evidence discipline

Prefer official generator documentation and platform documentation over aggregator blogs; treat community consensus (forums, social threads) as directional, not proof. Never promote a music-cognition finding (tempo, syncopation, groove) into a claim about platform reach or engagement — those are different domains and conflating them produces false confidence. Full sourcing method in [`references/prompt-best-practices.md`](references/prompt-best-practices.md).
