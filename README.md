# Agent Setup Guide

**Live:** [agentsetupguide.com](https://agentsetupguide.com)
**Staging:** [openclaw-starter.vercel.app](https://openclaw-starter.vercel.app)

A public, non-technical guide that walks a friend from zero to their own
always-on AI assistant. Two static pages, no build step, no dependencies.

---

## Layout

| URL | File | What it is |
|---|---|---|
| `/` | `index.html` | **The main guide.** Five steps: get ChatGPT, keep the Mac awake, paste the install prompt, create the Telegram bot on your phone, then paste the interview prompt that connects it to your mail, calendar, messages and notes. |
| `/compare` | `compare/index.html` | **Side quiz.** Routes people between OpenClaw and Hermes based on host, OS, complexity, multi-agent needs and credential preference. Has its own dark/light theme toggle — independent of the main page by design. |
| — | `graphics/` | Shared images. Referenced **absolutely** (`/graphics/…`) so `/compare` doesn't 404 from its subfolder. |

Both pages are self-contained: CSS and JS are inline, there is no framework,
no `package.json`, and nothing to compile. Open `index.html` in a browser and
what you see is exactly what ships.

---

## Making changes

```bash
git clone https://github.com/joeywrightphoto/agent-setup-guide.git
cd agent-setup-guide
open index.html          # edit, refresh, repeat
```

**Deploy is a push.** Vercel project `agent-setup-guide` auto-deploys `main` to
the live domain. There is no build command and no install step, so a push is
live in seconds.

Push a branch instead of `main` to get a preview build. Previews on this
project sit behind Vercel deployment protection, so preview URLs 302 to a login
and are useless on a phone — that's what the `openclaw-starter` staging project
is for. Push there when someone needs to look at it before it goes live.

---

## Editing rules for the main guide

These are the constraints that make the page work. Breaking one turns it back
into every other setup guide on the internet.

**1. Nobody installs anything by hand.**
No download lists, no version numbers, no "first, install X." The OpenClaw
installer brings its own Node, Homebrew and Git. If a change adds a manual
install step, the change is wrong.

**2. Subscription auth only.**
Never steer a reader toward an API console or a billing account. A normal
ChatGPT subscription is the whole story. This is the single mistake that costs
a beginner real money.

**3. One provider — ChatGPT.**
The guide deliberately offers no choice of AI company. OpenClaw supports
Anthropic too, but its subscription route runs through `claude -p`, which the
OpenClaw docs flag as programmatic/Agent-SDK usage and steer toward an API key
for long-lived gateway hosts — i.e. straight into rule 2. OpenAI's docs state
plainly that subscription OAuth is supported in external tools like OpenClaw.
One officially-supported lane beats a fork in the road on step one. Don't add
a second provider card back in.

**4. Assume the machine stays on.**
An always-awake computer is presented as a requirement, not an optional extra.
Step 2 sorts the reader into desktop / laptop-open / laptop-clamshell, and the
step 3 prompt configures sleep for whichever they are. Don't soften this back
into "you can skip it."

Clamshell is solved with [Amphetamine](https://apps.apple.com/us/app/amphetamine/id937984704)
(free, Mac App Store), which lifts macOS's external-display-and-keyboard
requirement for closed-lid operation. Apple Silicon laptops also need its
**Power Protect** option, or a charger plug/unplug can drop the session. This
is the one third-party app the guide names, because the OS alternative is a
`sudo` command a beginner shouldn't be pasting.

**5. Optional things get described, not linked.**
Anything in "Later, when you want more" is named by the *problem it solves*,
with no vendor, no link and no install steps. Each one is a `<details>`
accordion that opens to a copy-paste prompt, so the answer stays "ask your
assistant to add it" rather than "go download this." Keep the vendor names out
of both the summary and the prompt — naming the problem lets the assistant pick
whatever is current.

**5b. Telegram is a step, not a footnote.**
Step 4 spells out the @BotFather flow because it is the one part the AI cannot
do for the reader — it happens on their phone. The step-3 prompt still handles
the config side (token, `dmPolicy: pairing`, `openclaw pairing approve`), so the
two halves have to stay in sync. Don't collapse step 4 back into the prompt.

**5c. Two prompts, and the split is deliberate.**
Step 3 installs and gets the reader to "I texted it and it answered." Step 5
connects it to their real accounts. Do not merge them. A permissions failure
during an Apple or Google hookup would otherwise block someone from ever
reaching a working assistant, and the payoff moment is what makes the rest of
the work feel worth doing.

Step 5 is an **interview**, not a checklist: it asks the reader about their
life one question at a time, then wires up only what they said yes to. That is
the whole point — a friend doesn't know which integrations exist, so the
assistant has to ask. Keep the questions about *their life* (what email do you
use) rather than about software. Its non-negotiable clauses: read-only first,
never send as the user without asking, credentials go to a password manager,
and stop for anything requiring a System Settings grant.

**5d. Never tell a reader to disable SIP.**
`imsg` basic mode (send/receive text and media) needs only Full Disk Access and
Automation. The advanced iMessage actions — reactions, edits, unsend, threaded
replies, effects, polls — require System Integrity Protection to be off. That is
a permanent security downgrade to a friend's personal Mac and is out of scope
for this guide. The step-5 prompt says so explicitly; leave that line in.

**5e. The Apple integrations here are the officially supported ones.**
Google mail/calendar/contacts via the Google Workspace skill (browser OAuth),
IMAP mail via the mail skill (an app-specific password from account.apple.com,
never the real Apple password), iMessage via `imsg`, plus the Apple Notes and
Apple Reminders skills. Apple Calendar and Apple Contacts have no first-party
OpenClaw skill — don't imply they're a one-liner. If someone lives in Apple's
calendar, the honest answer today is that it needs custom work.

**6. Nothing personal on the page.**
No names, no file paths, no account handles, no references to whose setup this
came from. It has to be sendable to a stranger.

**7. Verify commands before changing them.**
Every command quoted on the page is checked against
<https://docs.openclaw.ai/start/getting-started>. Re-check before editing one —
the CLI moves. Currently quoted: `curl -fsSL https://openclaw.ai/install.sh | bash`,
`openclaw gateway install`, `openclaw gateway status`, `openclaw triage`,
`openclaw pairing list telegram`, `openclaw pairing approve telegram <CODE>`,
`openclaw skills check`, `openclaw skills list`, `openclaw skills search`.

---

## Design

The main guide follows Apple's marketing style, which is mostly a set of
restrictions:

- System font stack (SF Pro on Apple devices), never a webfont.
- Two backgrounds only — `#fff` and `#f5f5f7` — alternating by section.
- Text `#1d1d1f`, secondary `#6e6e73`, one accent: `#0071e3` blue. No gradients.
- Headlines large and tight: `font-weight: 600`, negative letter-spacing.
- Generous vertical space. If a section feels cramped, add padding, not columns.
- Pill buttons, 20px card radius, hairline `#d2d2d7` rules.

Tokens live in `:root` at the top of `index.html`. Change them there, not inline.

---

## Checklist before pushing to `main`

- [ ] Opens correctly at a narrow width (the guide gets read on a phone).
- [ ] The **Copy prompt** button copies the full prompt.
- [ ] Any command you touched still matches the OpenClaw docs.
- [ ] `/compare` still loads and its logos still resolve.
- [ ] No personal information anywhere on the page.
