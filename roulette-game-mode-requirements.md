# 🎰 CLAUDE CODE PROMPT — CLASSROOM GAME CONSOLE v2
### Adding "Lucky Spin" Roulette Mode + Unified Game Launcher

> **Context:** This document extends the existing **"Complete the Phrase"** classroom game (`index.html`).
> The task is to integrate both games into a single unified application with a shared launcher screen,
> shared player registration system, and a brand-new roulette game mode called **"Lucky Spin"**.
> Both games must coexist in the same `index.html` file, sharing the same visual language and tech stack.

---

## PROMPT (copy everything below this line)

---

You are extending an existing classroom game application into a **multi-game console** — a single `index.html` file
that hosts two distinct game modes:

1. **"Complete the Phrase"** — the existing Wheel of Fortune-style letter-guessing game (already built; keep it intact)
2. **"Lucky Spin"** — a brand-new roulette-style classroom game described in full below

Both games share the same visual system, tech stack, player registration module, and top-level launcher screen.

---

## TECH STACK (unchanged from v1 — apply to all new code)

- **Vanilla HTML + CSS + JavaScript** — single `index.html`, no build step, no framework, no backend
- **GSAP `@3.14`** loaded from jsDelivr CDN for all animations — specify the version explicitly:
  `<script src="https://cdn.jsdelivr.net/npm/gsap@3.14/dist/gsap.min.js"></script>`
- **Web Audio API** for all synthesized sound effects — initialize `AudioContext` only inside the first user gesture (button click), never on page load, to comply with browser autoplay policy
- **Google Fonts** via CDN for typography — always define a styled fallback font stack alongside the import
- Zero external dependencies beyond CDN links — must run by double-clicking `index.html`
- Target display: **1280px+ widescreen** (projector/monitor). No mobile responsiveness needed.

---

## PART 1 — THE UNIFIED GAME CONSOLE LAUNCHER

### What changes at the top level

The application now opens to a **Game Console Home Screen** instead of going directly into "Complete the Phrase."
This screen is the command center for the entire experience.

### HOME SCREEN design requirements

- Full-screen landing with the console brand name: **"CLASS ARCADE"** — displayed as an electrifying neon marquee title with a scanline texture overlay to evoke a classic arcade cabinet feel. Animate it with a flickering neon-light effect on load (the letters pulse irregularly, like real neon tubing).
- Below the title, two large **game selection cards** side by side, each representing one game mode:

  **Card 1 — "Complete the Phrase"**
  - Icon: a glowing letter tile (e.g. a stylized **"?"** tile)
  - Subtitle: *"Guess the hidden phrase, one letter at a time"*
  - A **"PLAY"** button that launches this game's existing setup flow

  **Card 2 — "Lucky Spin"**
  - Icon: an animated mini roulette wheel (a small CSS-animated spinning wheel icon that slows to a stop when the card is hovered)
  - Subtitle: *"Spin the wheels. Race to the answer."*
  - A **"PLAY"** button that launches the Lucky Spin setup flow

- Both cards should have a hover state with a dramatic lift effect (scale up, glow intensifies, card rises with a drop shadow)
- A subtle animated background: slow-drifting geometric shapes or particle field — something that feels alive but doesn't distract
- A persistent **"← HOME"** navigation button available on all game screens to return to this launcher (with a confirmation modal if a game is currently in progress: *"Return to Home? Current game progress will be lost."*)

---

## PART 2 — SHARED PLAYER REGISTRATION MODULE

Both games use **identical player registration**. This module must be built once and reused by both games.

### Player Registration Screen

- Displayed after a game is selected from the Home Screen, before entering that game's specific setup
- Register **2 to 4 players**
- For each player slot:
  - Text input for **player name**
  - **Avatar selector**: a grid of at least 10 illustrated emoji-style avatars rendered as styled `<div>` elements with colorful gradient backgrounds — not raw emoji text. Make them large (60×60px minimum), rounded, and visually rich. Examples: 🧑‍🚀 👩‍🎤 🧙 🦸 🐯 🤖 👾 🎩 🦊 🐸
  - Each player slot has a distinct accent color (Player 1: electric blue, Player 2: crimson red, Player 3: emerald green, Player 4: golden amber)
- **"+ Add Player"** button (up to 4), and a **"✕"** remove button per slot (minimum 2 players always required)
- A **"CONTINUE →"** button — disabled until at least 2 players have names entered
- This screen has a **back button** to return to the Home Screen

---

## PART 3 — "LUCKY SPIN" GAME MODE (NEW — BUILD IN FULL)

### Concept

Lucky Spin is a **tension-building roulette game** for the classroom. Three spinning wheels reveal a combined
challenge — a topic, a category, a constraint, a bonus modifier, or anything the moderator defines.
Once all three wheels stop, players race to complete a physical activity (writing, drawing, searching a textbook, solving a problem on paper, etc.). The **first player to finish** is awarded the round points by the moderator with a single click.

The magic of this game is entirely in **the wheel animation sequence** — the suspense of watching each wheel
slow down and lock in, one by one, is what makes the crowd lean forward.

---

### SCREEN A — LUCKY SPIN GAME CONFIGURATION

Shown after player registration, before the first round. This is where the moderator configures the wheels.

**Wheel Configuration Panel:**

For each of the **3 wheels**, the moderator enters the options that will appear on that wheel:

- Each wheel has a **label/title field** (e.g. "Topic", "Time Limit", "Bonus Rule", "Subject", "Format") — this label appears above the wheel on the game screen
- Each wheel has an **options list**: a dynamic list where the moderator types options one by one
  - An input field + **"+ Add Option"** button
  - Each added option appears as a styled chip/tag with a **"✕"** remove button
  - Minimum **3 options** per wheel required (validation enforced)
  - Maximum **12 options** per wheel
  - Options should be short (enforce a 30-character max per option with a character counter)
- A **"Shuffle options"** button per wheel — randomizes the visual starting position of options on the wheel

**Round Configuration:**
- Number of rounds input (default: 5, min: 1, max: 15)
- Points per round input (default: 100, min: 10, max: 1000, step: 10) — this is the points up for grabs each round, displayed prominently on the game screen during the activity phase

**Validation:**
- The **"START LUCKY SPIN"** button is disabled until all 3 wheels have at least 3 options and names are valid
- If validation fails, show inline error messages per wheel (e.g. *"Wheel 2 needs at least 3 options"*) with a gentle shake animation on the offending wheel config section

---

### SCREEN B — LUCKY SPIN GAME BOARD (THE MAIN STAGE)

This is the most visually spectacular screen in the entire application. It must feel like a real casino floor.

#### Overall Layout

```
┌─────────────────────────────────────────────────────────────────┐
│  TOP BAR: Round X of Y  │  [Round Points Badge]  │  [HOME btn]  │
├─────────────────────────────────────────────────────────────────┤
│                                                                 │
│   ┌──────────────────────────────────────────────────────────┐  │
│   │              THE THREE SPINNING WHEELS                   │  │
│   │   [  WHEEL 1  ]    [  WHEEL 2  ]    [  WHEEL 3  ]       │  │
│   │    (label)          (label)          (label)             │  │
│   └──────────────────────────────────────────────────────────┘  │
│                                                                 │
│   ┌─────────────────────────────┐   ┌────────────────────────┐  │
│   │   RESULT DISPLAY PANEL      │   │   LIVE SCOREBOARD      │  │
│   │  (shown after all 3 stop)   │   │   (player cards)       │  │
│   └─────────────────────────────┘   └────────────────────────┘  │
│                                                                 │
├─────────────────────────────────────────────────────────────────┤
│  MODERATOR CONTROL BAR (bottom)                                 │
└─────────────────────────────────────────────────────────────────┘
```

---

#### THE WHEELS — Critical Animation Specification

Each wheel is a **vertical slot-machine style reel** (not a flat circle — a tall, cylindrical drum that spins vertically). The center position is the "selected" slot, highlighted with a glowing selector frame.

**Wheel Visual Design:**
- Each wheel is rendered as a tall viewport window showing 3–5 options at a time (the center one is the "active" selection)
- The options above and below the center are visible but progressively faded/blurred (3D perspective effect — items further from center appear smaller and more transparent, simulating a curved drum)
- A fixed horizontal selector bar overlays the center of each wheel — a bright neon frame with corner brackets and a glow (like a crosshair locking onto a target)
- The wheel drum has a dark metallic texture background with subtle vertical ribbing to simulate a physical reel
- Each option text is styled on an individual tile — bold white text on a richly colored background, with the color varying per option (use a vibrant, varied palette — not all the same color)
- Wheel 1 has a **blue** accent glow, Wheel 2 has a **purple** accent glow, Wheel 3 has a **gold** accent glow

**The Spinning Animation — This is the Soul of the Game:**

When the moderator clicks **"SPIN"**, the following sequence plays:

1. **Launch phase (0–0.5s):** All 3 wheels simultaneously snap into a fast blur-spin. The options become an illegible streak of color. Play a rising mechanical whir sound (slot machine roar). The entire screen dims slightly as spotlights appear over the wheels.

2. **Full spin phase (0.5s–3s):** All 3 wheels spin at full speed. The options cycle rapidly. The audience cannot read any individual option. Camera-shake micro-animation on the wheel containers. Crowd-noise/casino ambiance sound fades in softly.

3. **Wheel 1 deceleration (starts at ~3s):** Wheel 1 begins slowing down dramatically — use an `ease-out` curve with a "bouncy" overshoot quality (it slightly overshoots the final position and bounces back, like a real mechanical reel). A tense drum-roll sound accompanies the slowdown.
   - **Wheel 1 locks:** A **THUD/CLUNK** mechanical sound plays. The selector frame on Wheel 1 flashes gold. The locked option lights up with a burst of light rays. Wheel 1's result text appears large above the wheel: **"[OPTION NAME]"** in glowing letters. Wheels 2 and 3 continue spinning.

4. **3-second suspense gap:** Wheels 2 and 3 keep spinning. A countdown tension animation plays — subtle, like a heartbeat pulse on the screen edges. A tick-tock or suspenseful pad sound plays.

5. **Wheel 2 deceleration (at ~6s):** Same mechanic as Wheel 1. Slightly longer deceleration to build more tension. Different THUD pitch. Wheel 2 result revealed. Wheels 3 continues spinning.

6. **3-second suspense gap:** Same as before but with visibly higher tension — the screen border glow intensifies.

7. **Wheel 3 deceleration (at ~9s):** The grand finale wheel. Wheel 3's deceleration is the most dramatic — it slows down very slowly, teasing the audience. The selector frame crackles with electricity. When it finally locks:
   - **Big SLAM sound** — louder and more dramatic than the others
   - Screen-wide flash of white light
   - Confetti burst from the top of Wheel 3
   - All 3 result labels light up simultaneously for a "full combination reveal" moment
   - A **triumphant short fanfare** plays

8. **Transition to Activity Phase:** After a 1.5-second pause to let the results breathe, the **Result Display Panel** slides in from the bottom with the full combination of all 3 results shown clearly.

**SPIN button behavior:**
- The **"SPIN"** button is large, prominent, and casino-styled — a big illuminated button with a satisfying press animation (it physically depresses with a shadow change on click)
- After clicking SPIN, the button is hidden/disabled for the entire spin sequence
- The button reappears only when the moderator is ready for the next round

---

#### RESULT DISPLAY PANEL (appears after all 3 wheels stop)

A stylish panel — think a casino results board — that displays:

- The **3 wheel labels** as column headers (e.g. "Topic", "Time Limit", "Bonus Rule")
- The **3 locked results** displayed as large, glowing, clearly readable tiles beneath each header
- A visual connector between them (e.g. **"Write about"** → [Topic result] → **"in"** → [Time result] → **"with"** → [Bonus result]) — this connector text is configurable or uses a sensible default
- A large **round points badge** showing the points up for grabs this round (e.g. **"⭐ 100 PTS"**) pulsing gently

---

#### ACTIVITY PHASE — The Race Begins

Immediately after the result panel appears, the game enters the **Activity Phase**:

1. **Activity Banner** drops from the top of the screen: a bold, animated banner reading **"GO! 🏁"** in giant letters, with a starting-gun sound effect (a bang + crowd cheer)

2. The wheels section shrinks or fades to give maximum space to the **Player Scoreboard**, which is now the focal point

3. The scoreboard shows each player's card (avatar + name + total score) in large format — easy to read from a distance across the classroom

4. Below the scoreboard heading: **"First to finish wins the round! Click the winner's name:"**

5. Each **player card becomes a large, pressable button** — glowing with their player color, with a subtle pulse animation inviting a click

6. The moderator watches the room. When a player completes the activity first, the moderator **clicks that player's card**

7. **A confirmation modal appears** (same danger-modal pattern as "Complete the Phrase" — consistent UX):
   - Shows the player's avatar and name prominently
   - Message: **"Award [Player Name] the [X] points for this round?"**
   - Two buttons: 🟢 **"YES — AWARD POINTS"** and ⬜ **"WAIT — GO BACK"**
   - This prevents misclicks in the chaos of an excited classroom

8. On confirm:
   - Points are added to that player's total score with a flying-number animation (the "+100" floats from the player card upward and dissolves)
   - A winner celebration plays — confetti, fanfare, player card enlarges briefly with a gold shimmer
   - A **round result banner** appears: **"[PLAYER NAME] wins the round! +[X] pts"**
   - After 3 seconds (or a moderator click), transition to the next round or end game

---

#### LIVE SCOREBOARD (sidebar during gameplay)

- Visible at all times during a round (right side or bottom strip)
- Shows each player's **avatar, name, and current total score** — note: in Lucky Spin, total scores ARE shown during gameplay (unlike Complete the Phrase), since points are only awarded once per round and there's no round-accumulation mechanic to hide
- Active/awaiting state is shown with a subtle glow
- After points are awarded, the winning player's score counter **animates** (count-up effect) and their card flashes gold

---

### SCREEN C — BETWEEN ROUNDS

- **"ROUND X COMPLETE!"** splash with the winner's avatar and name
- Shows updated full scoreboard for all players
- A **"NEXT ROUND →"** button for the moderator
- On the last round, the button reads **"SEE FINAL RESULTS →"**

---

### SCREEN D — GAME OVER / PODIUM

Same spectacular podium reveal as "Complete the Phrase" — consistent across both games:

- Podium reveals in order: 3rd place (bronze) → 2nd place (silver) → 1st place (gold, with confetti explosion and grand fanfare)
- Each position rises from below with bounce easing and a delay between each
- Show avatar, name, and final score on each podium step
- 1st place gets a **crown animation** — the crown drops from above onto the avatar
- A **"PLAY AGAIN"** button that resets Lucky Spin back to Game Configuration (keeping the same players)
- A **"← HOME"** button that returns to the Game Console Home Screen

---

## PART 4 — SOUNDS FOR LUCKY SPIN (Web Audio API — synthesized)

All sounds generated programmatically. No external files. Initialize `AudioContext` on first click only.

| Sound | Description |
|---|---|
| **Wheel launch whir** | Rising mechanical sound, like a slot machine engaging — starts low and climbs in pitch |
| **Full-spin ambient** | A continuous fast-cycling mechanical hum while all wheels spin at full speed |
| **Wheel deceleration drum roll** | A snare-style drum roll that slows in tempo as the wheel decelerates |
| **Wheel lock THUD** | A deep mechanical clunk — each wheel has a slightly different pitch (low, medium, high) |
| **Suspense heartbeat** | A subtle two-beat pulse (lub-dub) during the 3-second gaps between wheel stops |
| **Final wheel slam** | Deeper, louder, more dramatic version of the THUD — reserved for Wheel 3 |
| **Full combo fanfare** | Short triumphant melody when all 3 wheels are locked — 4–5 notes, ascending |
| **GO! starting gun** | A sharp bang followed by a brief crowd-cheer swell |
| **Round winner celebration** | Multi-note ascending fanfare, same style as "Complete the Phrase" win sound |
| **Confirmation modal open** | A soft chime — pleasant, not alarming |
| **Points award flying number** | A bright ascending ding synchronized with the number animation |
| **Podium reveal (shared)** | Grand synthesized fanfare, builds in layers — same as existing game |

---

## PART 5 — ANIMATIONS SPECIFICATION FOR LUCKY SPIN

| Animation | Specification |
|---|---|
| **Wheel spin — launch** | GSAP timeline: `duration: 0.3`, `ease: "power4.in"`, blur filter `0 → 8px` |
| **Wheel spin — full speed** | Continuous looping translateY, `duration: 0.08` per option height, linear ease |
| **Wheel deceleration** | GSAP timeline with custom ease: fast → medium → slow → overshoot → settle. Use `CustomEase` if available, otherwise chain multiple tweens with decreasing speed |
| **Wheel lock flash** | Selector frame: scale `1 → 1.15 → 1`, opacity flash, box-shadow color burst |
| **Result label reveal** | `gsap.from` with `y: -30, opacity: 0, duration: 0.5, ease: "back.out(1.7)"` |
| **Screen-edge tension pulse** | CSS `box-shadow` inset grows and pulses on `body` element during suspense gaps |
| **Confetti burst** | 60–80 CSS particle `<div>` elements, randomized color/size/trajectory, gravity simulation via GSAP |
| **GO! banner** | Drops from `y: -200` to center, bounces, holds 1.5s, then slides up and exits |
| **Player card pressable pulse** | CSS `animation: pulse 1.5s ease-in-out infinite` — scale 1 → 1.03 → 1, glow grows |
| **Points flying number** | GSAP: `y: 0 → -80`, `opacity: 1 → 0`, `scale: 1 → 1.4 → 0.8`, duration 1.2s |
| **Score count-up** | GSAP `innerText` counter from old value to new value over 1s |
| **Podium rise** | Staggered `gsap.from` with `y: 300, duration: 0.8, ease: "bounce.out"`, 1.2s delay between each |
| **Crown drop** | `y: -150 → 0`, bounce ease, slight rotation wobble on landing |
| **Home screen card hover** | `scale: 1 → 1.04`, `translateY: 0 → -8px`, box-shadow intensity ×2, duration 0.2s |
| **Neon flicker (title)** | CSS keyframe animation: opacity varies irregularly `1 → 0.85 → 1 → 0.7 → 1` with varied timing — simulate real neon |

---

## PART 6 — VISUAL DESIGN FOR LUCKY SPIN

Maintain full visual consistency with "Complete the Phrase" — same dark background, same font system, same neon accent philosophy. Lucky Spin adds its own **casino-floor layer** on top:

- **Color palette additions:** Deep burgundy `#1a0010` as the Lucky Spin background (vs. navy for Complete the Phrase) — creates instant visual distinction when switching modes while staying in the same dark family
- **Wheel container:** Dark metallic chrome frame with a subtle radial gradient. The wheel viewport has rounded corners and an inset inner shadow to simulate depth
- **Option tiles on wheels:** Each option tile has a unique vibrant background color drawn from a preset palette (use 8–10 colors: red, blue, green, purple, orange, teal, pink, gold, etc.) — never repeat the same color for adjacent tiles. Bold white text, `letter-spacing: 0.5px`
- **Selector bar:** A neon-bright horizontal bar (default: electric gold `#FFD700`) overlaying the center of each wheel. The bar has corner accent marks (like a gun sight) and an animated outer glow
- **Result display tiles:** Each of the 3 result tiles is styled as a raised casino chip — circular-ish, beveled edge, vibrant background matching the landed option's color, gold border
- **SPIN button:** Styled as a large casino button — deep red with a metallic sheen, raised with a hard drop shadow, rounded rectangle. Has a glass highlight on top. Text: **"SPIN"** in all-caps bold. On hover: glows brighter, shadow expands. On click: depresses (shadow reduces, slight scale-down)
- **Round points badge:** A casino chip icon next to the points value. Pulses gently with a golden glow.
- **Activity phase background:** When the GO! banner fires, the background briefly shifts to a warmer tone (a fast cross-fade), signaling the transition from passive watching to active competition

---

## PART 7 — IMPLEMENTATION NOTES (LUCKY SPIN SPECIFIC)

1. **Wheel animation must be smooth at all screen sizes above 1280px.** Use `requestAnimationFrame` or GSAP's rendering for the continuous spin loop — never use `setInterval` for visual loops.

2. **The wheel options must wrap infinitely** — when the reel reaches the bottom of the options list, it seamlessly loops back to the top without a jump. Achieve this by cloning the option list 3× and offsetting the start position so the loop is always seamless.

3. **Wheel 3's deceleration must be visibly slower than Wheels 1 and 2** to maximize tension. Aim for: Wheel 1 deceleration ≈ 1.5s, Wheel 2 ≈ 2s, Wheel 3 ≈ 3s.

4. **The result shown on each wheel must be a genuine random selection** from the options entered for that wheel — use `Math.random()` seeded at spin-time, not pre-selected.

5. **The SPIN button must be physically impossible to click during a spin sequence.** Use `pointer-events: none` + `opacity: 0.3` immediately on click, and restore only after all 3 wheels have fully locked and the result panel has appeared.

6. **Round points are configurable per session** (set in Game Configuration) but fixed for all rounds within a session — the moderator cannot change the point value mid-game.

7. **Player card click-to-award** must be locked out during the wheel spin sequence and only active during the Activity Phase. This prevents accidental premature point awards.

8. **Confirm before awarding points** — always use the confirmation modal pattern. No direct single-click award.

9. **"GO!" banner must be visible for at least 2 seconds** before player cards become pressable. This gives players time to start their activity before the moderator can end the round.

10. **Both games share the same player data** if navigated to sequentially in the same session — but if a user goes back to Home and starts a new game mode, they go through player registration again (this is intentional and correct behavior).

---

## PART 8 — NAVIGATION & STATE ARCHITECTURE

The app has the following state machine:

```
HOME SCREEN
    │
    ├──► [Complete the Phrase selected]
    │         │
    │         ├──► PLAYER REGISTRATION
    │         │         │
    │         │         └──► CTP GAME FLOW (existing)
    │         │
    │         └──► [← HOME] returns here (with confirm if in-game)
    │
    └──► [Lucky Spin selected]
              │
              ├──► PLAYER REGISTRATION
              │         │
              │         └──► LUCKY SPIN CONFIGURATION
              │                   │
              │                   └──► LUCKY SPIN GAME BOARD
              │                             │
              │                             ├──► BETWEEN ROUNDS
              │                             │         │
              │                             │         └──► (loops back to GAME BOARD)
              │                             │
              │                             └──► PODIUM / GAME OVER
              │                                       │
              │                                       └──► PLAY AGAIN (→ LS Config)
              │
              └──► [← HOME] returns here (with confirm if in-game)
```

- All screen transitions use the same cinematic fade/wipe system as "Complete the Phrase"
- The `[← HOME]` button must always be visible, but never intrusive — small, top-left, semi-transparent
- The in-progress warning modal: **"Return to home? Your current game session will be lost."** with **"YES, EXIT"** and **"KEEP PLAYING"** buttons

---

## PART 9 — DELIVERABLE

A single updated `index.html` file containing:

- ✅ The **Class Arcade home screen** with game selection
- ✅ The **shared player registration module**
- ✅ The **existing "Complete the Phrase" game** — fully preserved, zero regressions
- ✅ The **new "Lucky Spin" game** — built to the full specification above
- ✅ All sounds synthesized via Web Audio API
- ✅ All animations via GSAP `@3.14` + CSS
- ✅ Zero external dependencies beyond CDN links
- ✅ Runs by opening `index.html` in Chrome or Firefox — no install, no server, no configuration

**Do NOT ask for clarification.** Make all design and implementation decisions yourself based on this specification. Deliver the complete, working multi-game console in one shot.

---

## QUICK REFERENCE — LUCKY SPIN AT A GLANCE

| Parameter | Value |
|---|---|
| Number of wheels | 3 (fixed) |
| Options per wheel | 3 min / 12 max |
| Max option label length | 30 characters |
| Delay between wheel stops | 3 seconds (fixed) |
| Wheel stop order | 1 → 2 → 3 (always sequential) |
| Points per round | Moderator sets at start (10–1000, default 100) |
| Who awards points | Moderator only (click player card → confirm modal) |
| Score visibility | Full total scores shown during gameplay |
| Max players | 4 (shared with Complete the Phrase) |
| Max rounds | 15 |
| Offline activity | Happens physically — app does not time or judge it |
| Single file | Yes — `index.html` only |
