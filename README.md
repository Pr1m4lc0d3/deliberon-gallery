# Deliberon Community Gallery

This repository is the backing store for the **Community Gallery** in [Deliberon](https://deliberon.com) — a place where users share the **agent portrait avatars** and personas they create and discover ones made by others.

## Layout

Each shared agent is its own folder — a real, copy-installable plugin:

```
agents/{name}-{id}/
  plugin-icon.png          the avatar
  wildcard-settings.json   the full persona (voice, vocation, posture, coordinates…)
  manifest.json            the plugin manifest
  README.md                a human-readable description of that agent
index.json                 the browse index the app reads
```

Nothing is shared or overwritten between agents — every upload is a self-contained folder.

## How it works

You don't edit this repo by hand. It's written to by a small, free **Cloudflare Worker** (the gallery server) that the Deliberon app talks to:

- Sharing an agent (with a required author handle) checks the image, blocks duplicate portraits, and commits the agent's folder + an `index.json` entry — all in **one atomic commit**.
- **Stars** work like GitHub's: one per person. Live counts live in the Worker; a scheduled job folds them into `index.json` every few minutes, so the numbers here stay real.
- The app reads `index.json` and the per-agent files directly from this repo to show the gallery and install agents.

The GitHub credentials live only inside the Worker, never in the app.

## Contributing & moderation

Agents are contributed from inside Deliberon (build a custom agent → **Share to Gallery**), not by pull request; an author handle is required. Every gallery image must be a square avatar portrait. Anything reported enough times is automatically hidden pending review, and the maintainer can remove entries or block abusive contributors.

## A note on sharing

This is a **community share** gallery — agents posted here are meant to be browsed, downloaded, and remixed. The full persona of each agent is public by design (it has to be, so the app can install it). If you post an agent, you're sharing it with the community.
