# EdgeAI

A glass chat panel that floats over every other app, embedding your
**free** chat with Claude, ChatGPT, Google Gemini, or Perplexity AI — you
pick. Drag the tab into place. Tap **Home** to silently screenshot your
screen and paste it straight into the chat; hold **Home** to
screen-record. Separately, an optional mic button starts a **background**
voice session with your choice of ChatGPT Advanced Voice, Gemini Live,
Hume AI, Grok 3, or Sesame AI — no extra window pops up for it. An
animated orb at the top of the screen shows while it's listening or while
the AI is waiting on you, and a flowing border glow shows when it's
replied.

Nothing here calls a paid API — every chat is that provider's own free
website running inside the app.

## Setup

```
npm install
npm start
```

On first run, the tab appears as a small glass shape on the right edge of
your screen. Click it to open the chat panel, log into your chosen AI
provider inside it (one-time), and you're set.

## Controls

| Action | Result |
|---|---|
| Click the tab | Slide the chat panel open/closed |
| Tap `Home` (from anywhere, any app) | Panel auto-opens, screenshots the whole screen, auto-pastes into the chat box |
| **Triple-tap** `Home` (3 quick taps) | Opens **Settings** |
| Hold `Home` | Start recording; release to stop and save the clip |
| Click the mic button (if enabled) | Starts/stops a background voice session — no window opens |

## Repositioning the tab or mic button

This is a deliberate, two-step action rather than something that can
happen by accident while clicking:

1. In Settings, click **"Move tab…"** or **"Move mic button…"**
2. Drag it wherever you want — you can adjust as many times as you like
3. Click the green tick that follows it to confirm and lock it back in
   place

"Snap tab to edge" (Left/Right/Top/Bottom) is a separate, instant action —
it snaps to wherever you're currently positioned along that edge, not a
fixed spot.

## Voice chat (background, no window)

Off by default — turn it on in Settings → Voice chat, then a mic button
appears (top-left by default). Clicking it starts a session with your
selected **Voice AI** (a separate choice from the text-chat AI agent,
picked in its own Settings section) entirely in the background.

**Read this before relying on it**: since there's no visible window, the
app can't show you that provider's own call screen — instead, it tries to
find and click a plausible "start call" / "end call" button on that page
for you, automatically. This is inherently best-effort:

- Every provider's page is built differently, so this may work on some and
  not others, and can break if a site changes its layout.
- **Grok 3's voice mode is paid-tier only** (Premium+/SuperGrok) as of this
  writing — free accounts won't have a voice button for the app to find.
- **Hume AI and Sesame AI are research/demo products**, not mainstream
  consumer chat apps — their public demo pages may require their own
  sign-up flow, time limits, or could change URL entirely without notice.
- If it can't find a control, the page still loads in the background and
  holds microphone permission — nothing else in the app depends on this
  working, and the mic/orb will still reflect on/off state either way.

Please actually test this and tell me what you see (works / silent / a
console warning) — I can't click through a live GUI from here, so this
needs a real run to know what's actually happening.

## The orb & border

A small animated orb (flowing wave-line style) appears at the **top
centre** of the screen only when voice chat is listening, or the AI is
waiting on you for a question — invisible the rest of the time. A
separate flowing "ribbon" border animates around the full edge of your
screen for a finished reply, a pending question, or while recording — each
in its own colour, set in Settings → Notifications.

## AI agent vs Voice AI — two separate choices

Settings has two independent pickers:
- **AI agent** — Claude, ChatGPT, Google Gemini, Perplexity AI. This is
  what loads in the main chat panel.
- **Voice AI** — ChatGPT Advanced Voice, Gemini Live, Hume AI, Grok 3,
  Sesame AI. This is what the mic button connects to, in the background.

They can be different — e.g. text-chat with Claude, voice with Gemini
Live.

A couple of these sites actively try to detect and block "unusual"
embedded browsers (Google's sign-in does this on purpose, for security
reasons on their end) — this app sets a standard desktop-Chrome identity
string to get past the most common check, but it isn't a guarantee for
every provider. If a provider shows a blank page or a "browser not
supported" message, tell me exactly what you see.

## Accounts & logins

**There's no such thing as one "EdgeAI account" that logs you into
Claude, ChatGPT, Gemini, and the rest** — those are separate companies
with their own sign-up systems, and there's no legitimate way to bridge
that (attempting to automate account creation across unrelated services
would violate each of their terms of service, so this isn't something I
built). What EdgeAI does do, honestly: once you log into a provider
inside the app, that login is remembered permanently, so you only ever
sign into each one once — not every time you open the app. This is also
explained inside the Settings window itself.

## Chat panel position & size

- **Default side** (Left/Right) controls which edge the full-height chat
  drawer attaches to.
- **"Draw new area…"** lets you click-drag anywhere on your screen to set
  an exact custom position and size instead — the panel then fades in/out
  there instead of sliding from an edge. **"Reset to default"** undoes
  that.

## Settings

Triple-tap Home to open it. Beyond the above:
- **Style** — Liquid glass or Solid (flat colour, no blur)
- **Button colour** and **shadow colour**, with a shadow-strength slider
- **Tab shape** — Pill, Circle, Square, or Bar
- **Tab size** — a 50%–250% slider
- **Notification colours** — separate colours for listening, a response, a
  pending question, recording, and the mic button

All settings save to disk automatically (in your Windows user profile,
`AppData\Roaming\claude-edge-panel`) and persist across restarts.

## Building a Windows .exe

```
npm install
npm run dist
```

Produces `dist\Claude Edge Panel Setup x.x.x.exe` (installer) and a
portable version. No manual steps needed — the previously-required
`CSC_IDENTITY_AUTO_DISCOVERY` workaround is now baked into the `dist`
script itself.

- **Build it on Windows, for Windows** — cross-compiling from Linux/macOS
  needs Wine and is fragile.
- **No icon set yet** — uses Electron's default. Send me an image if you
  want a custom one.
- **Unsigned builds may trigger a SmartScreen warning** on first launch —
  "More info" → "Run anyway" is normal for a self-built app.

## Starting automatically with Windows

Once installed via the Setup `.exe` (or run as the portable `.exe`), the
app registers itself to launch quietly at login. Turn it off via Windows
Settings → Apps → Startup.

## If something doesn't work

`npm start` (not the built `.exe`) auto-opens DevTools consoles for the
tab and Settings windows. Check those for red error text and send it to
me — that's the fastest way to find the actual bug instead of guessing.
Lines starting with `devtools://` (e.g. "Autofill.enable failed") are
Chrome DevTools' own internal noise, not an app bug — ignore those and
look for anything referencing this project's own files instead.

Settings (including the tab/mic's saved position) live in
`%APPDATA%\claude-edge-panel\` and persist across every folder or zip you
download — they don't reset just because you got a fresh copy of the
project. If a layout change ever makes an old saved position land
somewhere wrong, the app now detects that automatically (via an internal
version number) and resets just the position/size fields back to
default, keeping all your colour/style preferences intact — you shouldn't
need to manually delete anything.

## Things worth knowing

- **This only captures your primary display** for screenshots/recording.
- **The tab, mic button, orb, and drawn chat-box area each have a small
  transparent margin** around their visible shape (so drop-shadows aren't
  clipped into a hard square edge) — clicks right in that margin can pass
  through instead of hitting the shape.
- **A drawn chat-box area is remembered until you draw a new one or hit
  Reset** — if your display resolution changes, a custom area might end up
  partially off-screen; Reset fixes that instantly.
- **The "glass blur" is CSS**, not true blur-behind-the-desktop — Windows
  doesn't give apps a supported way to blur what's genuinely behind them.
