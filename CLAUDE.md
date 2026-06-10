# Heart Quest

Browser dating simulator. 12 characters, 10 locations, 80+ actions.

## Stack
- Vanilla JavaScript
- Single HTML file (`index.html`) — all CSS, JS, and HTML embedded (~2,190 lines as of e8e76f7)
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

- `gameState` — single object holds all state: current screen, points, action history, character memory
- Character memory model tracks player behaviour per NPC: `trustLevel`, `likesHumor`, `likesDeepTalk`, `hateBragging`, etc.
- `analyzeCustomAction()` — parses free-text player input and rates it for tone and appropriateness; central mechanic for custom dialogue
- Relationship stages: Stranger → Interested → Dating → Committed (100 points to complete)
- Turn-based: 8 turns per game session
- Locations have time-of-day mechanics that affect character availability and dialogue

## Notes
- Version 2.0.0 (per code comments)
- Save/load via `localStorage` (automatic — no manual save needed)
- `analyzeCustomAction()` is the key differentiator — it lets players write anything and gets rated; understand this function before modifying dialogue flow
- GoatCounter analytics script at bottom of file
