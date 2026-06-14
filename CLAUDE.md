# Heart Quest

Browser dating simulator. 12 characters, 10 locations, 80+ actions.

## Stack
- Vanilla JavaScript
- Single HTML file (`index.html`) — all CSS, JS, and HTML embedded (~2,270 lines)
- No framework, no build step, no external dependencies (except GoatCounter)

## How to Run
- Open `index.html` directly in browser — no server or build required
- `heart-quest-final.html` is a backup/alternate version kept alongside the main file

## Deployment
- GitHub repo: https://github.com/hulkiokantabak/heart-quest
- Live site: https://hulkiokantabak.github.io/heart-quest/
- Deploy: push `index.html` to `master` — GitHub Pages serves it directly

## Architecture
Everything is in `index.html`:

- `gameState` — single object holds all state: current screen, points, action history, character memory. Always build a clean state with `freshGameState(overrides)` — `quickStart`/`quickReplay`/`resetGame` all go through it so no field can drift between paths.
- Character memory model tracks player behaviour per NPC: `trustLevel`, `likesHumor`, `likesDeepTalk`, `hateBragging`, `recentApproaches`, etc.
- `analyzeCustomAction()` — parses free-text player input and rates it for tone and appropriateness; central mechanic for custom dialogue. Includes negation handling (`hasUnnegated`) so "you're not boring" isn't read as an insult, and a variety softener so repeating the same approach lands at half value (novelty/improvised input is never softened).
- Relationship stages (by points): 👋 Stranger (<15) → 🤝 Acquaintance (15) → 👥 Friend (40) → 💕 Love Interest (70) → 💑 Partner (100). Reach 100 to complete.
- **Momentum cap** (`STAGE_CAPS` / `stageCap()`): a single encounter can only add so many points, scaled by current stage (Stranger 18 → Partner 50). This keeps all 8 encounters meaningful instead of letting one optimized action win in two turns. Applies to both preset and custom actions.
- **Risk integrity**: in `calculatePoints`, positional "fit" bonuses (location/time/age/interest/personality/style/trust/streak) only apply to non-negative actions, so context never rescues a rude move. An attraction *mismatch* always penalizes.
- Turn-based: 8 turns (`maxEncounters`) per game session.
- Time of day grants a points bonus when it matches a character's preferred times (and is flagged with 💕 on the picker). It does not gate availability or change dialogue.

## Notes
- Version 2.1.0 (`VERSION` constant; About modal and footer read from it)
- Save/load via `localStorage` (automatic — no manual save needed). Reads and writes are wrapped in try/catch so corrupt or blocked storage can't white-screen the game.
- Accessibility: the selectable `<div>` cards (traits, characters, locations, times) are made keyboard-operable via `enhanceA11y()` + a delegated Enter/Space handler on `#app`; action risk is shown as a text label, not colour alone.
- `analyzeCustomAction()` is the key differentiator — it lets players write anything and gets rated; understand this function before modifying dialogue flow
- GoatCounter analytics script at bottom of file
- Validate JS after edits: `node -e "const fs=require('fs');const m=fs.readFileSync('index.html','utf8').match(/<script>([\s\S]*?)<\/script>/);new Function(m[1]);console.log('OK')"`
