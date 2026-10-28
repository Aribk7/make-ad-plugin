# make.ad for Claude Code

[make.ad](https://www.make.ad) turns a one-paragraph brief into a finished vertical video ad: the character, the
script, a voice or a song, every scene, the clips and the cut. It also makes one-off AI pictures, video clips
(including free LTX-2.5 clips), songs, UGC talking-to-camera ads and short films.

This plugin connects Claude Code to make.ad's MCP server (`https://www.make.ad/api/mcp`) and adds a skill that tells
Claude how to drive it well: quote before spending, idempotency keys on every create, and a stop at each review
(character, script, voice, stills) unless you ask for the ad to run on its own.

## Install

```
claude plugin install make-ad --marketplace Aribk7/make-ad-plugin
```

or, inside Claude Code:

```
/plugin marketplace add Aribk7/make-ad-plugin
/plugin install make-ad@make-ad
```

Then run `/mcp`, pick `make-ad` and Authenticate: your browser opens make.ad, you sign in and Approve. No key to copy.
A server or script with nobody to sign in can use a key instead: https://www.make.ad/mcp#key

## What you can ask for

- "Make a 30-second singing ad for my skincare brand"
- "A UGC ad: a woman in her thirties talks to her phone about our protein bar"
- "A whiteboard explainer of how our filter works"
- "Clone this winning ad for my brand"
- "A free AI video clip of a fox running through snow"

Every paid job is quoted in credits first. New to make.ad? Your first ad is free: https://www.make.ad/signup

## Other clients

Claude (web and desktop), ChatGPT, Cursor and Codex: one-step setup at https://www.make.ad/mcp

## License

MIT

make.ad is built by the team behind [Revana](https://revana.si).
