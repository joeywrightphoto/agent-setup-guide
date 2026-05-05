# Agent Setup Guide

A small static website for helping friends choose between a streamlined Hermes setup and a more customizable OpenClaw setup.

## Use it

Open `index.html` in a browser, or host this folder with any static host.

## What it does

- Starts with one simple question.
- Routes toward Hermes or OpenClaw with a live recommendation preview.
- Asks about install target, operating system, complexity, multi-agent needs, maintenance tolerance, and credential preference.
- Recommends a secondary computer by default, with VPS and main-machine guidance when appropriate.
- Notes that Hermes works on macOS, Linux, and Windows through WSL2, but not native Windows via the official installer.
- Gives credential guidance for subscription/account-based setup versus provider API tokens.
- Steers subscription users toward OpenAI ChatGPT plans for predictable OpenAI-backed usage, and treats Anthropic/Claude as API-token-only.
- Explains cost shape without scaring people: subscriptions are predictable; API tokens need spend caps and billing alerts.
- Links to the canonical install guides for [Hermes](https://hermes-agent.nousresearch.com/docs/getting-started/installation) and [OpenClaw](https://docs.openclaw.ai/install).
- Includes a security baseline for Tailscale/private access, SSH keys, limited OS users, updates, and token hygiene.
- Shows a short setup checklist for the recommended route.

## Default safety posture

The guide intentionally recommends a secondary computer first, a locked-down VPS second, and the main personal computer only for short limited trials.
