# AGENTS.md

You maintain the Agent Setup Guide, a public website at agentsetupguide.com.

## What it is

A free, non-technical guide that walks a complete beginner from zero to their
own always-on AI assistant running on their Mac. The audience is Joey's
friends — photographers and creatives, not developers. Someone reading it has
never opened a terminal and should never need to. It also quietly credits
Joey's other apps and offers a paid 1:1 consultation.

## Where things are

- **Repo:** `github.com/joeywrightphoto/agent-setup-guide` — **public.** Assume
  every byte you commit is readable by strangers.
- Two pages, both self-contained static HTML with inline CSS and JS:
  - `/` → `index.html` — the five-step guide
  - `/compare` → `compare/index.html` — an older OpenClaw-vs-Hermes quiz
  - `graphics/` — shared images, referenced **absolutely** as `/graphics/…` so
    the subfolder page doesn't 404
- No `package.json`, no build step, no dependencies, nothing to compile. Open
  `index.html` in a browser and what you see is exactly what ships.

## First thing you do, every time

Read `README.md` before touching anything. It is the real operating manual —
the design system, the colour rules, the bento layout pattern, the icon
sprite, the nav bar, the app shelf, the quiz's JS hooks, and a list of
specific bugs that have already bitten once. This file is the orientation;
the README is the law. If the two ever disagree, **the README wins** — and say
so, so this file can be corrected.

## Deploying

Pushing to `main` is the deploy. The Vercel project `agent-setup-guide`
auto-publishes `main` to the live domain in seconds, with no build.

**Do not push to `main` on your own.** Make the change locally, show Joey what
it looks like, and push only when he says ship it. Branch pushes give preview
builds, but previews on this project sit behind Vercel deployment protection
and 302 to a login, so they're useless for showing him on a phone.

## The rules that make this site work

Breaking one of these turns it back into every other setup guide on the
internet. The README explains each in full; this is the short version.

1. **Nobody installs anything by hand.** No download lists, no version
   numbers, no "first install X." The OpenClaw installer brings its own Node,
   Homebrew and Git. A change that adds a manual install step is wrong.
2. **Subscription auth only.** Never steer a reader toward an API console or a
   billing account. A normal ChatGPT subscription is the whole story. This is
   the one mistake that costs a beginner real money.
3. **One AI provider — ChatGPT.** The guide deliberately offers no choice.
   Don't add a second provider card back in; the README explains why.
4. **The machine staying awake is a requirement,** not an optional extra.
5. **Optional add-ons are described by the problem they solve,** with no vendor
   name and no link — each is an accordion holding a copy-paste prompt, so the
   answer is always "ask your assistant to add it."
6. **Never tell a reader to disable System Integrity Protection.**
7. **Nothing personal on the page.** No file paths, no account handles, no
   references to whose setup this came from. It has to be sendable to a
   stranger. Joey's name, his apps and his Calendly link are the only
   deliberate exceptions.
8. **Verify any command you touch against docs.openclaw.ai** before changing
   it. The CLI moves. Don't quote a command from memory.

## Design

Strict Apple marketing style: system font, two backgrounds (`#fff` and
`#f5f5f7`) alternating by section, generous space, pill buttons, hairline
rules, minimal shadow, **no glass anywhere.** Colour is added deliberately and
sparingly — gradient on one payoff word per headline, tinted gradient cards, a
per-step accent, a soft wash behind the hero. The rule is that **colour marks
one thing per screen**; if two elements in view compete, one is wrong.

Icons are single-colour vector line art from one `<symbol>` sprite. Never an
emoji, never an icon font, and never a coloured chip behind an icon.

Sets of three use the bento pattern — one tall box beside two stacked — not
three equal columns.

Tokens live in `:root`. Change them there, not inline. The quiz carries a
second dark-mode token set plus a `prefers-color-scheme` block; edit all three
or dark mode drifts.

## How to work

- **Be skeptical of your own change.** Actually open the page and look at it at
  desktop, iPad and phone widths. Don't declare something fixed because the
  CSS looks right.
- **Measure instead of eyeballing** when something is "off-centre" or
  "overflowing." A past complaint about a box being shifted left turned out to
  be perfectly centred with left-aligned text inside it — the fix was the
  opposite of the obvious one.
- The guide contains **eight `<pre>` prompt blocks** that readers copy and
  paste. Before pushing, confirm they are byte-identical to the previous
  commit unless you meant to change one. Someone may be mid-paste.
- If you touch the quiz, **drive it end to end** afterwards — all six steps,
  both outcomes, dark mode, the detail drawer, reset — and check the console is
  clean. Its markup and JS are original; only the stylesheet was replaced.
- Run the full pre-push checklist at the bottom of the README.
- Write commit messages as a sentence describing the change, matching the
  existing log.

## When to stop and ask

- Anything that would remove or weaken one of the eight rules above.
- Changing the price or the booking link.
- Adding a dependency, a build step, or a framework. There aren't any, on
  purpose.
- Anything that puts a new personal detail on a public page.
