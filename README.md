# Agent Setup Guide

**Live:** [agentsetupguide.com](https://agentsetupguide.com)
**Staging:** [openclaw-starter.vercel.app](https://openclaw-starter.vercel.app)

A public, non-technical guide that walks a friend from zero to their own
always-on AI assistant. Two static pages, no build step, no dependencies.

> **Working on this with an AI agent?** `AGENTS.md` is the orientation — what
> this site is, the eight rules that must not be broken, and how to deploy.
> This README is the detail, and it wins wherever the two disagree.

---

## Layout

| URL | File | What it is |
|---|---|---|
| `/` | `index.html` | **The main guide.** Five steps: get ChatGPT, keep the Mac awake, paste the install prompt, create the Telegram bot on your phone, then paste the interview prompt that connects it to your mail, calendar, messages and notes. |
| `/compare` | `compare/index.html` | **Side quiz.** Routes people between OpenClaw and Hermes based on host, OS, complexity, multi-agent needs and credential preference. It uses the same fixed light theme as the main guide. |
| — | `graphics/` | Shared images. Referenced **absolutely** (`/graphics/…`) so `/compare` doesn't 404 from its subfolder. |

`graphics/agent-setup-guide-social.png` is the 1200×630 Open Graph and X
share card for the main guide. Its absolute production URL appears in the
metadata at the top of `index.html`; keep the image and those tags together.

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

This means no *developer* tools or technical setup by hand. A reader still
installs the normal ChatGPT desktop app in step 1 and may need Telegram on
their phone in step 4, so never promise "zero things to install."

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

Describe the closed-lid option as a laptop the reader wants to close overnight
or while they are away. It stays plugged in with its lid closed; never frame it
as a laptop that has to stay open nonstop, or tell them to tuck it onto a shelf.

**5. Optional things get described, not linked.**
Anything in "Later, when you want more" is named by the *problem it solves*,
with no vendor, no link and no install steps. Each one is a `<details>`
accordion that opens to a copy-paste prompt, so the answer stays "ask your
assistant to add it" rather than "go download this." Keep the vendor names out
of both the summary and the prompt — naming the problem lets the assistant pick
whatever is current.

**Exception — Wispr Flow referral.** The card after the `#later` accordions is
the one deliberate named-vendor and external-link exception. It uses the
supplied `graphics/wispr.png` and must link to
`https://wisprflow.ai/r?JOEY175`. Its visible disclosure has to stay directly
beside the CTA: **Use Joey's link and you both get a free month of Flow Pro.**
Do not hide, soften, or move that line away from the link, and do not use this
exception as a reason to add other named optional add-ons.

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

**Both pages** follow Apple's marketing style, which is mostly a set of
restrictions:

- System font stack (SF Pro on Apple devices), never a webfont.
- Two backgrounds only — `#fff` and `#f5f5f7` — alternating by section.
- Text `#1d1d1f`, secondary `#6e6e73`, one accent: `#0071e3` blue.
- Headlines large and tight: `font-weight: 600`, negative letter-spacing.
- Generous vertical space. If a section feels cramped, add padding, not columns.
- Pill buttons, 20px card radius, hairline `#d2d2d7` rules.
- Minimal shadow. Depth comes from spacing and hairlines, not glass or blur.
  There is no glass anywhere now — the quiz's sticky blurred top bar was
  removed so both pages open the same way. Don't reintroduce one.
- **Icons are single-colour vector line art, never emoji.** They come from one
  `<symbol>` sprite at the top of `<body>` — 24px grid, `1.6` stroke,
  `fill="none"`, round caps and joins, drawn in `currentColor`. Used as
  `<svg class="ic"><use href="#i-name"></use></svg>`. There is deliberately
  **no chip or coloured square behind an icon** — the tinted card is already
  the container, and a second one muddies it. Need a new one? Draw it into the
  sprite in the same style. Don't paste an emoji, don't add an icon font.

Tokens live in `:root` at the top of each file. Change them there, not inline.
The quiz is light-only; do not add a dark token set, a `data-theme` override or
a `prefers-color-scheme` rule back in.

### Colour

The restrictions above would produce a page that looks like a homework
assignment, so colour is added deliberately and in four places only:

1. **A gradient on one word of a headline** (`.grad`, `#0071e3 → #5e5ce6 →
   #bf5af2`). Three per page, maximum. It marks the payoff word — *AI
   assistant*, *hands*, *alive*. Never on body text, never on a whole line.
2. **Saturated card backgrounds** — deep blue, violet, warm and mint gradients
   with white headings, softer white body copy and light line icons. They are
   intentionally strong; keep the text contrast intact when changing them.
   The three cards in **Not a chatbot** are the deliberate exception: clean
   white panels with one oversized coloured line icon and one matching payoff
   phrase in the heading. They have no visible border stroke.
3. **A per-step accent.** Each of the five steps sets its own `--accent`
   (blue → indigo → purple → pink → amber) which drives its step number and the
   left bar of any `.note.flag` inside it, so the page reads as a progression.
   The values are darkened versions of Apple's system colours because they get
   used on small uppercase text — don't swap in the bright ones.
4. **A soft radial wash behind the hero** (`.hero::before`), and the two product
   identities in the quiz: Hermes blue/violet, OpenClaw orange/pink.

The rule: colour marks *one* thing per screen. If two elements in view are
competing for attention with colour, one of them is wrong.

Card colours are **gradients, not flat fills** — each runs through deep shades
of one hue and carries a matching border, light copy and an `--icon` colour for
the line icon inside it. Keep those pieces in step. These cards used to be pale
tints and disappeared against the `#f5f5f7` sections; do not wash them out.
The `#what` exception is intentionally not a tinted card: its `--feature-pop`
token drives the icon and the `.card-pop` words, while the surrounding panel
stays white.

Immediately after `#what` is the quiet requirements section: a clean white
band before the five steps. Its heading uses the same scale as other section
headlines and simply says **You only need two things**, with *two things* as
the section's one gradient colour moment. It says there are exactly two
requirements: an always-awake Mac and a ChatGPT subscription. A reader starts
on the Mac they already have; a Mac mini is only an optional,
separate-machine upgrade — never a prerequisite. Apple currently positions it
as its most affordable Mac desktop, but do not hard-code a price there.
ChatGPT Plus is the starting plan; tell readers to upgrade only after their
real use requires it. Keep the section free of a lead-in sentence: the
headline and its two requirements are the entire message.

### Bento: three items, unequal boxes

Sets of three don't use three equal columns. `.bento` is Apple's pattern — one
tall box beside two stacked ones. Add `.feat` to whichever tile should be the
tall one, and `.flip` on the container to put it on the right instead of the
left. The featured tile centres its content vertically and scales its icon and
heading up, because a tall box with a paragraph stranded at the top reads as a
mistake rather than a choice.

Which item gets `.feat` is an editorial decision, not a default. Step 2 flips
so **Laptop, lid closed** is the tall one, because that's the section's actual
payoff — it's the reason nobody reading this needs to buy a second machine.

Step 2's three device choices use the supplied product cut-outs in
`graphics/devices/`. The desktop and lid-open art sits just beyond each card's
right edge, leaving an unobstructed text column on the left; align their
visible left edges, not the laptop's small lower lip. The closed-lid card is
the exception: full-width text in its upper row and `.device-art-closed` low
across a separate image row, with its visible left edge aligned to that text
inset and its right edge flush to the card. The cards are white against the
pale-gray setup section, with no neutral border stroke. They are product art,
not icons: do not replace
them with a second icon chip or a generic device glyph. The desktop, lid-open
and lid-closed images must stay matched to their respective choices.

`.bento` collapses to a stack at **700px**, on its own breakpoint rather than
the 860px one the 3-up grids use, so iPad portrait keeps the layout.

### Quiz-specific gotchas

- The stylesheet is a full replacement for the old teal glass theme, but the
  **markup and JS were left untouched.** Only two hooks cross that line:
  `--score-width` (set inline by JS on `.score-fill`) and `data-tone="amber"`
  on `.answer`. Don't rename either.
- `.check-list li` is built in JS as an inline `<svg>` tick plus a `<span>`.
  Style the SVG — don't add a `::before` marker or you get two ticks.
- The old brand mark SVG and its `.mark` / `.brand-copy` / `.brand-byline`
  styles are gone with the top bar. `.brand` is now a single link.
- `#resetButton` lives in `.page-actions` beneath the header, opposite the
  **Back to Setup Guide** link. Keep the ID — it resets the six-step quiz.

### The nav bar and the robot mark

Both pages open with the same sticky bar: translucent white, 20px blur, one
hairline underneath, 56px tall. Nothing else goes in it.

The header and browser-tab icon use Joey's silver 3D Robby mark at
`graphics/robby-head.png`; it is the small raster image beside the black guide
wordmark and the favicon for both pages. It has one blue-eye colour pop and
should stay compact enough to feel like a mark rather than a second hero.

The only in-page 3D character appearance is `graphics/robby-thumbs-up.png`,
to the right of the **Once it’s alive** prompts (stacked underneath on phones).
Do not repeat it through the guide.

**Bar layout rules, learned the hard way:**

- The guide's four nav links — **What it does**, **What you need**, **Setup**
  and **Add-ons** — hide below **820px**, and `.navcta` has to pick up
  `margin-left:auto` in that same query or the CTA slides left and sits next
  to the wordmark.
- On the quiz the header carries only the wordmark and consult CTA. Its
  **Back to Setup Guide** and **Reset comparison** controls sit directly below
  the header in `.page-actions`, so both remain visible without crowding it.
- `scroll-padding-top: 74px` on `html` keeps anchored sections clear of the
  bar. Any new `id` target inherits it automatically.

### The consult section

`#consult` sits between the add-ons and the quiz cross-link, and it is what
the nav CTA points at. The **Book a session** button deep-links to
`https://calendly.com/joeywrightphoto/ai` — a published, live event: 2 hours,
Zoom, **$500** taken through Stripe at booking, with a recording, written
summary and action plan sent afterwards. The page deliberately leaves the
price off the button label. Changing the underlying price or booking link still
requires Joey's approval.

Its three bento-card titles are intentionally larger than ordinary tile
headings: the session is the guide's human payoff, so the card titles carry the
composition rather than disappearing into the body copy.

Their gradients follow the supplied colour studies rather than merely
darkening: teal to a lighter teal, magenta through orange to yellow, and
purple to teal.

The quiz's own CTA points at `/#consult` rather than Calendly, so there is one
place to change.

### Credit and the also-by shelf

Joey is credited in four places, and they should stay in sync:

1. The nav wordmark — `Agent Setup Guide` next to the robot.
2. The guide's hero eyebrow — `Agent Setup Guide by Joey Wright`, with the
   guide name in blue and Joey's name linking to @JoeyBaggaBots.
3. The `#also` section above the guide's footer — a `.shelf` of four artwork
   cards for **SpeedGrid, HeySiggy, ShootFeed, ShootCast**. See below.
4. A one-line `Also by Joey:` row in the quiz footer. Text links only — the
   quiz doesn't get the card treatment.

If an app is added or renamed, update **both** the `#also` shelf and the quiz
footer row. Check the live taglines before writing new copy; don't invent them.

**The shelf** is a centred four-card row on wide screens. Below 1100px it
becomes a scroll-snap carousel so the artwork stays large: edge arrows, four
position dots, touch scrolling and five-second autoplay. Autoplay pauses during
interaction and is disabled when the reader prefers reduced motion.

- Art lives in `graphics/apps/*.jpg`, ~900px wide, under 60KB each. These are
  the apps' **own** OG/store graphics, resized — pulled from each project repo,
  not redrawn. If an app's branding changes, re-export from its repo rather
  than editing the JPEG here.
- Every card is `aspect-ratio: 1.905` with `object-fit: cover`. ShootCast's
  source is 2.048, so it crops slightly at the sides — check its logo and
  tagline survive if you replace it.
- `.art` needs `height: auto`. The `width`/`height` attributes on `<img>` are a
  presentational hint and will beat `aspect-ratio` without it, which renders
  every card absurdly tall. This has bitten once already.
- The shelf sits **outside** `.wrap`. At 1100px and above, all four cards share
  the available width and are horizontally centred. Below that, each card is
  up to 420px wide (or the viewport minus 56px) and centred with calculated
  side padding so it does not collapse into a thumbnail.
- **The art carries each app's name, so the card doesn't repeat it in type.**
  The name lives in `alt` and `aria-label`. Don't add an `<h4>` back.
- Tone: this is a thank-you, not a pitch. The line under the heading is about
  taking a look at what else Joey's made — **no money talk, no "this is how I
  make a living."** One plain sentence per app.

---

## Checklist before pushing to `main`

- [ ] Opens correctly at a narrow width (the guide gets read on a phone).
- [ ] The **Copy prompt** button copies the full prompt.
- [ ] Any command you touched still matches the OpenClaw docs.
- [ ] `/compare` still loads, its logos resolve, and the quiz still runs all
      six steps with no console errors.
- [ ] The nav bar fits with no horizontal overflow at 1440, 834, 430 and 360,
      and the CTA is still hard against the right edge at every one.
- [ ] The app shelf centres all four cards at 1440, switches to carousel mode
      at 1099 and below, and its touch scroll, arrows, dots and autoplay all
      select the same card at 390 / 768 / 834. Lazy images don't decode in an
      off-screen screenshot — scroll to the shelf and wait before judging a
      grey card as broken.
- [ ] No emoji used as an icon anywhere, and no chip behind a line icon.
- [ ] The eight prompt `<pre>` blocks hash identical to the previous commit
      unless you meant to change one. Someone may be mid-paste.
- [ ] No personal information anywhere on the page.
