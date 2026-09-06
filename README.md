# Agent Setup Guide

agentsetupguide.com — two static pages, no build step.

| Path | File | What it is |
|---|---|---|
| `/` | `index.html` | **The main guide.** The easiest path to your own always-on AI assistant: pick a subscription, stop the machine sleeping, paste one prompt and let Claude Code / Codex do the whole install. |
| `/compare` | `compare/index.html` | **Side quiz.** Routes people between OpenClaw and Hermes based on host, OS, complexity, multi-agent needs and credential preference. |

Shared assets live in `graphics/` and are referenced absolutely (`/graphics/...`) so both pages can use them.

## Editing rules for the main guide

The guide's whole promise is **nobody installs anything by hand.**

- No download lists. The OpenClaw installer brings its own Node/Homebrew/Git.
- No terminal apps, no code editors, no VPS, no second computer.
- Subscription auth only. Never steer readers toward an API console — that's how people get a surprise bill.
- Anything optional is described by the *problem it solves*, with no link and no install steps: you ask your assistant to add it later.

Commands quoted on the page are checked against https://docs.openclaw.ai/start/getting-started — re-verify before changing them.

## Use it

Open `index.html` in a browser, or host this folder with any static host.
