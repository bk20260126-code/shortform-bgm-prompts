# Getting started (no coding required)

This page is for anyone who wants background music for a short video and doesn't want to touch any code, install anything, or read the rest of this repo. It takes about 5 minutes.

You don't need Claude, this plugin installed, or any technical setup for this page — just a web browser and an AI music website.

## What this is, in one sentence

A fill-in-the-blanks recipe for describing the music you want, in words an AI music generator actually understands well — so you get a usable result on your first or second try instead of your tenth.

## Step 1 — Answer five questions about your video

Write down short answers, even rough ones:

1. **How long is the video (or how long should the music be)?** e.g. "about 20 seconds"
2. **Is anyone talking in the video, or is it just captions/text on screen?** e.g. "no talking, just captions" or "yes, someone is narrating"
3. **How should the music make people feel?** Pick 2–3 words. e.g. "happy, calm, a little exciting" or "mysterious, tense"
4. **What do you NOT want?** e.g. "no singing," "not childish," "not scary," "not too loud"
5. **Does it need to loop (repeat smoothly) or is one play-through enough?**

## Step 2 — Fill in this template

Copy the block below into a text editor or directly into the AI music website, and replace everything in `[brackets]` with your own answers from Step 1. Delete the bracket marks when you're done.

```text
[Instrumental — delete this word if you DO want singing/vocals.] A [2-3 mood words] [genre, e.g. "acoustic pop" or "electronic"] track, around [a number, e.g. 100] BPM. It should start right away with no long quiet intro, stay simple enough that on-screen text is still easy to read over it, and [end cleanly / loop back to the beginning smoothly — pick one]. Length: about [your number] seconds.
```

### A filled-in example, so you can see exactly what this looks like

Say you have a 20-second product demo video with captions, no talking, and you want it to feel upbeat and trustworthy but not childish, with no singing, looping smoothly:

```text
Instrumental. A warm, upbeat, confident acoustic pop track, around 108 BPM. It should start right away with no long quiet intro, stay simple enough that on-screen text is still easy to read over it, and loop back to the beginning smoothly. Length: about 20 seconds.
```

That's it — that whole paragraph is your prompt.

## Step 3 — Paste it into an AI music generator

Pick one (all have a free tier as of this writing — check current pricing on their sites, it changes):

- **Suno** — [suno.com](https://suno.com)
- **Udio** — [udio.com](https://udio.com)
- **Google Flow Music (Lyria)** — [labs.google/fx/tools/flow](https://labs.google)

Paste your filled-in paragraph into the text box where it asks what music you want, and click Create / Generate.

## Step 4 — Listen and decide

- **Good enough?** Download it and use it.
- **Close, but one thing is off?** Change only that one thing in your text (e.g. only the BPM number, or only the mood words) and generate again. Changing several things at once makes it hard to tell what helped.
- **The length is wrong?** Some AI music tools ignore the length you asked for. If your download is longer than you need, most of these websites have a trim/edit option after generation — use that rather than re-generating from scratch.

## A few things that trip people up

- **"It added singing even though I didn't want any."** Make sure the word "Instrumental." is the very first word of your prompt.
- **"It sounds nothing like what I described."** Your genre word was probably too vague. "Music" or "background music" gives the AI nothing to go on — try naming an actual style, like "lo-fi hip-hop," "cinematic orchestral," or "synth-pop."
- **"The download isn't the length I asked for."** This is common and not a mistake on your part — see Step 4 above.
- **"Can I use this commercially (in an ad, a business video, etc.)?"** Check the specific AI music website's terms before you publish anything — free plans on some of these tools restrict commercial use. This repo doesn't grant or affect those rights.

## Want more control?

The rest of this repository is a more thorough version of the same idea, meant for people using [Claude](https://claude.com) or Claude Code: it maps your video scene-by-scene instead of treating it as one block, tests three variations at once so you can compare, and runs a listening checklist before calling a track "done." See the main [README](README.md) if that's useful to you — otherwise, the steps above are the whole thing.
