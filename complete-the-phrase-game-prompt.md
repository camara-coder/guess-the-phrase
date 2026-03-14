# 🎯 CLAUDE CODE PROMPT — "COMPLETE THE PHRASE" CLASSROOM GAME

> **How to use:** Open Claude Code in your terminal (`claude` command), paste the prompt below, and Claude Code will generate a single `index.html` file. Open it in Chrome or Firefox on a widescreen display and you're ready to play. 🎮

---

## PROMPT (copy everything below this line)

---

Build a fully production-ready, single-page browser game called **"Complete the Phrase"** — a Wheel of Fortune-style classroom party game designed to be projected on a large screen and played verbally by groups of up to 4 players, moderated by one person at a computer.

---

## TECH STACK

- **Vanilla HTML + CSS + JavaScript** in a single `index.html` file (no build step, no frameworks, no backend)
- Use the **Web Audio API** for all sound effects (synthesized — no external audio files needed)
- Use **CSS animations + GSAP (loaded from CDN)** for all transitions and reveals
- Use **Google Fonts** (loaded via CDN) for typography — choose bold, game-show style fonts
- All assets must be inline or CDN-loaded — must run by simply opening `index.html` in a browser
- No data persistence needed — everything lives in memory during the session

---

## GAME FLOW & SCREENS

The game has the following sequential screens/states:

### SCREEN 1 — GAME SETUP

- A dramatic animated intro/title screen with the game logo **"COMPLETE THE PHRASE"** with glowing/pulsing animation
- A form to register **2 to 4 players**
- For each player slot:
  - Text input for **player name**
  - **Avatar selector**: a grid of at least 10 fun illustrated emoji-style avatars the player can click to choose (e.g. 🧑‍🚀 👩‍🎤 🧙 🦸 🐯 🤖 👾 🎩 🦊 🐸 — render these large and styled, not raw emoji — use styled `<div>` elements with colorful backgrounds per avatar)
- A **"+ Add Player"** button to add more player slots (up to 4), and a remove button per slot
- Input for **number of rounds** (default 3, minimum 1, maximum 10)
- A large **"START GAME"** button — disabled until at least 2 players are registered with names

### SCREEN 2 — ROUND SETUP (shown before each round)

- Display: **"ROUND X of Y"** with big animated entrance
- A text area where the moderator types the **phrase for this round** (hidden from players — blur or cover the screen until confirmed)
- Optionally, a **Category label** input (e.g. "Famous Sayings", "Movies", "Science") — shown on the game board
- A **"REVEAL BOARD"** button that transitions to the game board

### SCREEN 3 — GAME BOARD (main gameplay screen)

This is the most important screen. It must be visually spectacular.

**Layout:**

- Top bar: Round indicator + Category label
- Center: The **Letter Board** — a large grid of tile boxes, one per character in the phrase
  - Spaces between words are visible gaps (no tile)
  - Each letter tile starts **face-down/hidden** (show as a glowing blank square)
  - When a letter is revealed, it **flips over** with a 3D card-flip animation and lights up
  - Punctuation (apostrophes, hyphens, etc.) are shown immediately and not counted as guessable
- Right sidebar: **Live Scoreboard** — shows each player's avatar, name, and **current round points** (points accumulated this round, not total). Total score is NOT shown during gameplay.
- Bottom bar: **Active Player indicator** — whose turn it is, with a highlighted glow/pulse effect around their card

**Moderator Controls Panel** (below the board or as a slide-up panel — always accessible):

- A text input to **enter the guessed letter** (single character) + **"GUESS LETTER"** button
- A text input to **attempt a full phrase solve** + **"SOLVE PUZZLE"** button
- **"WRONG GUESS"** button — to manually trigger a wrong-guess event (turn passes, bad sound plays)
- **"NEXT PLAYER"** button — manually advance turn if needed
- The moderator controls must be visually distinct (semi-transparent overlay or bottom panel) so they don't break the projected game aesthetic

**Scoring Logic:**

- Each round has a **letter point value** — default 100 points per occurrence
- When a letter is correctly guessed: award `100 × number of times that letter appears` to the active player's round score, reveal all instances of that letter simultaneously with flip animations, play a success sound
- If the guessed letter is NOT in the puzzle: play a buzzer sound, show a visual "WRONG!" flash, pass turn to next player
- If a player solves the puzzle correctly: they keep all round points accumulated, the board fully reveals with a celebration animation (confetti, lights, fanfare sound), add round points to their **total score**, advance to round end
- If a solve attempt is wrong: play buzzer, show visual fail, pass turn — player loses their accumulated round points for this round (they go back to 0 for the round)
- Turns cycle: Player 1 → Player 2 → Player 3 → Player 4 → Player 1 → ...
- A turn ends on: wrong letter guess, failed solve attempt
- After the puzzle is solved: show a **ROUND WINNER banner** with the solver's name and avatar, their round points earned, then transition to next round or end game

### SCREEN 4 — BETWEEN ROUNDS

- Show a **"ROUND X COMPLETE!"** splash screen
- Reveal the **full phrase** one last time
- Show each player's **total accumulated score so far** (this is the only time total scores are shown mid-game)
- A countdown or "NEXT ROUND" button for the moderator to continue

### SCREEN 5 — GAME OVER / PODIUM

- A spectacular **podium reveal screen**
- Reveal winners in this exact order with dramatic animation delays:
  1. First: **3rd place** rises from bottom with bronze styling
  2. Then: **2nd place** rises with silver styling
  3. Last: **1st place** rises highest, with gold, confetti explosion, fireworks effect, triumphant fanfare
- Show each winner's avatar, name, and final score
- A **"PLAY AGAIN"** button that resets everything back to Screen 1

---

## SOUNDS (Web Audio API — synthesized)

Generate all sounds programmatically using the Web Audio API. Required sounds:

- **Letter reveal**: a bright, ascending musical ding (like a xylophone hit)
- **Multiple reveals**: rapid staggered dings, one per revealed tile
- **Wrong guess / buzzer**: a descending harsh buzz (classic game show wrong answer)
- **Puzzle solved / round win**: a triumphant ascending fanfare (multi-note melody)
- **Correct letter typed**: a soft click/tick
- **Countdown tick**: a soft metronome-like tick
- **Game over / podium**: a grand orchestral-style synthesized fanfare, building in layers

---

## ANIMATIONS (GSAP + CSS)

Required animations:

- **Letter tile flip**: 3D CSS transform rotateY flip from blank to revealed letter, with a glow burst on completion
- **Wrong guess flash**: screen briefly flashes red with a shake effect on the active player card
- **Player turn indicator**: animated glowing border, pulsing halo around the active player
- **Round win**: confetti burst (CSS-only particles), tiles flash gold sequentially, banner drops from top
- **Podium reveal**: each podium position slides up from below with bounce easing, with delays between each
- **First place**: fireworks particle burst, gold shimmer effect, crown animation
- **Score update**: number counter animates (counts up) when points are added
- **Screen transitions**: smooth cinematic fade or wipe between all screens
- **Intro screen**: animated title with letter-by-letter entrance, glowing pulse

---

## VISUAL DESIGN

Go for a **bold, premium game-show aesthetic**. Think Las Vegas meets classroom fun:

- **Dark background** (deep navy or near-black) with vibrant neon accent colors
- **Letter tiles**: styled like classic game show squares — white/cream face, bold black serif letter, gold border, depth shadow
- **Typography**: Use a bold display font (like "Bebas Neue" or "Black Han Sans") for titles and a clean sans-serif for UI text. Load from Google Fonts.
- **Player cards**: each player has a distinct accent color (Player 1: blue, Player 2: red, Player 3: green, Player 4: yellow/gold). Cards have the avatar, name, and round score.
- **Scoreboard sidebar**: styled like a TV scoreboard — semi-transparent dark panel with glowing score numbers
- **Active player glow**: the active player's card has an animated outer glow/pulse in their player color
- **Moderator panel**: dark translucent bottom strip with subtle glass-morphism styling — clearly a "control area"
- Make it look like it belongs on television. Every element should feel polished, purposeful, and exciting.

---

## IMPORTANT IMPLEMENTATION NOTES

1. **Single file**: Everything must be in one `index.html`. No external JS or CSS files (CDN links for GSAP and Google Fonts are fine).
2. **Responsive only to wide screens**: Target 1280px+ width (projector/monitor). No need for mobile responsiveness.
3. **No data storage**: All state is in JavaScript memory. Refreshing the page resets the game — that's fine.
4. **Moderator controls must never be accidentally triggered**: Use clear, well-spaced buttons with confirmation on "SOLVE PUZZLE" (to prevent accidental clicks).
5. **The phrase input on Round Setup must be hidden or blurred** so players watching the screen cannot see it being typed. Use a `type="password"` field or a blur overlay with a "tap to reveal/edit" toggle.
6. **Letter guessing is case-insensitive**: normalize all input to uppercase.
7. **Duplicate letter guesses**: if a letter has already been revealed, show a gentle "Already revealed!" message and do not penalize or advance turn.
8. **Handle phrases with numbers and special characters**: numbers and special chars (!, ?, ., etc.) are shown immediately on the board but cannot be guessed — they don't count toward scoring.
9. **All interactive controls should have keyboard shortcuts** where sensible (e.g. Enter to submit a guess).
10. **Do NOT ask for clarification** — make all design and implementation decisions yourself based on these specs. Deliver a complete, working game in one shot.

---

## DELIVERABLE

A single `index.html` file that, when opened in Chrome or Firefox on a widescreen display, runs the complete game end-to-end with zero configuration, zero installation, and zero external dependencies beyond CDN links.

The game must be immediately usable by a teacher or moderator with no technical knowledge — clear labels, intuitive flow, and zero ambiguity in the UI.

---

## QUICK REFERENCE — CUSTOMIZATION TIPS

| Setting | Default | How to change |
|---|---|---|
| Points per letter | 100 | Tell Claude Code to use a different value before generating |
| Number of rounds | 3 (configurable 1–10) | Already handled in the UI |
| Max players | 4 | Already enforced in the UI |
| Avatar count | 10+ | Ask Claude Code to add more if desired |
| Screen target width | 1280px+ | Mention a different resolution if needed |
