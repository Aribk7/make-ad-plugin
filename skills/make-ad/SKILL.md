---
name: make-ad
description: Make video ads, AI video clips, pictures, songs and short films with make.ad through its MCP server. Use when the person asks for a video ad, a UGC or talking-to-camera ad, a song or jingle ad, an AI video clip (including free LTX-2.5 clips), an AI picture, a product shot, or a short film, or mentions make.ad.
---

# make.ad

make.ad turns a brief into a finished video ad (script, voice or song, character, scenes, clips, cut) and also makes
one-off pictures, clips, tracks and movies. It is reached through its MCP server at `https://www.make.ad/api/mcp`.

## Connect first

If no `make-ad` (or `make.ad`) tools are available, connect the server and sign in through the browser:

- Claude Code: `claude mcp add --transport http make-ad https://www.make.ad/api/mcp`, then `/mcp`, pick `make-ad`, Authenticate.
- Codex: `codex mcp add make-ad --url https://www.make.ad/api/mcp` then `codex mcp login make-ad`.
- Anything else: https://www.make.ad/mcp has a one-step setup for each client.

Then call `whoami` (who you act as, what you may spend) and `get_options` (every model, voice, size and length).
Both are free.

## The rules that keep it cheap and correct

1. **Quote before anything paid.** Every paid tool has a `quote_*` twin. Tell the person the credits before starting.
2. **Make up an `idempotency_key` for every create** (a fresh one per new thing). Sending the same key again answers
   with the same thing and holds nothing more, so a retry after a network error is always safe.
3. **Follow the `next` line** in every answer. It says exactly what to do now.
4. **Wait inside the call, never in a loop.** Pass `wait_seconds` (up to 40) to `get_ad`, `get_render`, `get_film`.
   Several renders at once? One `get_render` with `render_ids: [...]` waits for all of them.
5. **Pictures go in by id, never as files or links**: a finished render (`from_render`, `from_renders`) or a cast member
   (`request_cast_upload` then `create_cast_member`, or `add_render_to_cast`).

## Free AI video: LTX-2.5

`generate_video` with `model: "pc-ltx-2-5"` is free: no credits, no limit, from a prompt or one picture, 2–15 s, up to
1080p, always with sound. It runs on make.ad's own GPU, one clip at a time for everyone, so it waits its turn: the answer
carries `queue: { ahead, about_seconds }`. Tell the person where they are in line, then `get_render` with
`wait_seconds`, again each time it answers `still_working`. Good for drafts, ideas and b-roll; use a paid model when the
person needs it fast.

## Making an ad

1. Pick the kind. Not sure, or asked for a format (whiteboard explainer, product showdown, podcast clip, talking
   product)? `list_workflows` has ready recipes: a `brief_template` to fill and the tool that makes it.
   - `create_song_ad`: a song sings the script (Suno). Catchy, memorable.
   - `create_narrated_ad`: a voice reads it (pick from `list_voices`, or `design_voice` for an original one).
   - `create_ugc_ad`: one person talking to camera.
   - `create_ad`: every setting.
2. Write the brief as a moment, not a product description: who, the problem in a scene, what changes, the line to end
   on. Include what the ad must never claim. A few words like "kidney support" is a label: the studio will develop the
   angle, but a real moment makes a better ad.
3. **Ask the person before creating**: approve each review here in the chat, or let it run to the finished video
   (`auto: true`)?
4. Not on auto, the ad stops at its reviews: character, script, voice, stills. Stay with it in the same turn
   (`get_ad` with `wait_seconds`), show what is ready (`view_pictures`, `get_ad` with `detail: "full"`), and:
   - yes: `approve_stage` (`then_auto: true` if they now want the rest to finish on its own);
   - change it: `revise_stage` with their note, or `redraw_stills` for chosen stills.
5. Finished: `get_ad` gives the video link. `set_captions` burns captions in (free).

A script the person wrote is used word for word and checked against the brand's never-claim list first. If it stops on
"Script check", show them the flagged lines and the suggested wording; they edit, then `retry_ad`.

## One-off renders and movies

- `generate_image` / `generate_video` / `generate_music` / `generate_shot`, then `get_render`.
- A picture to animate: `generate_video` with `from_render` set to the picture's render id.
- Movies and ad movies: `quote_film`, `create_film`, then `get_film` and `approve_film_stage` at each review.

## When something fails

Read the answer's `error` and `next`. A failed render gives its credits back. Content refusals (a safety filter, a
public figure, a brand name in a picture) come back the same way every time: change the prompt or picture instead of
retrying as is.
