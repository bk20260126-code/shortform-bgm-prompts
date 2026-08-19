# shortform-bgm-prompts

A Claude Code / Claude plugin that turns a short-form vertical video's scene timing into a precise, generator-ready AI music prompt — instead of asking for "upbeat background music" and re-rolling until something sticks.

Built for Instagram Reels, YouTube Shorts, and TikTok. Works with any AI music generator (Suno, Udio, Google Lyria / Flow Music, or others) — it produces the prompt and the QA discipline; you paste the prompt into whatever generator you use.

> **Not a developer, or don't want to install anything?** → **[GETTING-STARTED.md](GETTING-STARTED.md)** is a 5-minute, no-coding guide: fill in a short template, paste it into an AI music website, done. Everything below this point is the fuller version for Claude / Claude Code users.

## What it does

- Maps your video's scene timing and the state the viewer should feel, scene by scene, into musical cues (tempo, arrangement changes, accent placement) — not just an overall mood.
- Writes the actual prompt sentence using the structure and phrasing generators respond to best, including generator-specific handling of negative prompting, instrumental/vocal control, and start-timing control.
- Runs a candidate-testing discipline (3 candidates, one variable changed at a time) so you know what actually caused a good or bad result.
- Gives you a listening QA checklist before you call any candidate "approved" — hook, timing, caption space, mobile-speaker translation, loop, rights, technical specs.
- Keeps the whole thing in one project artifact (`MUSIC-PROMPT.md`) instead of losing the reasoning in chat history.

## Install

Copy this plugin into your Claude Code plugins directory, or install the packaged `.plugin` file through your client's plugin installer.

## Use

Ask Claude to write a BGM prompt for a reel/short, or describe the video and say what kind of music it needs. The skill activates automatically; it will ask for the handful of details it needs (duration, scene timing, whether there's voiceover, target generator) rather than guessing them.

```
"Write a background music prompt for this 22-second product demo Reel.
Scenes: 0-3s hook, 3-10s problem, 10-18s solution, 18-22s CTA.
No voiceover, captions throughout. Target generator: Suno."
```

## What's inside

```
skills/shortform-bgm-prompts/
├── SKILL.md                          # the workflow: intake → scene-to-cue → prompt → candidates → QA → feedback
├── references/
│   ├── scene-to-cue-system.md        # failure-path audit, scene-to-cue method, candidate gate, QA checklist, feedback loop
│   └── prompt-best-practices.md      # prompt structure, generator-specific negative prompting, duration caveats, session-editing gotcha
└── assets/
    └── music-prompt-template.md      # the MUSIC-PROMPT.md template to copy per project
```

No MCP servers, agents, or hooks — this is a pure knowledge/workflow skill. It doesn't call out to any external service; you still do the actual generation in your music tool of choice.

## Notes

- The generator landscape (Suno/Udio/Lyria specifics, negative-prompt syntax, duration limits) changes; re-verify against each generator's current official documentation before relying on exact numbers.
- Don't promote a single post's performance into a permanent rule about what BGM works — the skill's feedback-loop guidance asks for a repeated pattern (2+ occurrences) before treating a signal as real.
- This plugin doesn't handle music licensing for you. Confirm commercial-use rights for both any reference audio you upload and the generator's own output terms before publishing.

## License

MIT
