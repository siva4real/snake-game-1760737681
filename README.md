# Snake — Minimal, Modern

## Summary
This repository contains a minimal, production‑ready implementation of the classic Snake game. It runs entirely in the browser with no external dependencies. The UI is clean and responsive, supports keyboard and touch controls, and persists your best score locally. The game is configurable at runtime via a query parameter (?url=...) so you can load theme and gameplay options from a remote JSON (or image/text) file.

Key features:
- Playable Snake on an HTML5 canvas
- Keyboard (Arrow keys / WASD) and optional on‑screen touch controls
- Pause/Resume, Restart, score and best score (stored in localStorage)
- Configurable speed, grid size, wrap-around behavior, and colors
- Optional background image or color
- Works on GitHub Pages with a single self‑contained index.html

## Setup
No build step is required.

- Local: Open index.html in any modern browser (Chrome, Edge, Firefox, Safari).
- GitHub Pages:
  1. Push this repository to GitHub.
  2. Enable GitHub Pages for the repo (Settings → Pages → Build and deployment → Source: Deploy from a branch; Branch: main, Folder: /root).
  3. Visit the published URL; the game will load immediately.

The app has no external dependencies and will work offline after the first load thanks to standard browser caching.

## Usage
- Start: The game starts automatically. The snake initially moves to the right.
- Move: Arrow keys or WASD to change direction.
- Pause/Resume: Press P or click “Pause/Resume.” Space toggles pause; if you’re on the Game Over screen, Space also restarts.
- Restart: Click “Restart.”
- Touch controls: On mobile, a D‑pad appears in the overlay (configurable).

Scoring:
- +1 point per food eaten
- Speed increases gradually every few points (configurable)
- Best score is saved locally and shown in the HUD

### Configuration via ?url=...
You can pass a URL that returns configuration to customize the game. Add a query parameter `?url=<your-config-url>` to the page address.

Supported formats:
- JSON (recommended): content-type application/json or .json
- Image: content-type image/* or common image extension (.png/.jpg/.jpeg/.gif/.webp/.svg)
- Plain text: simple `key=value` lines (fallback)

Recognized JSON keys:
- speed (number, 4–30) — initial speed in tiles per second
- grid or gridSize (number, 10–60) — number of cells per side
- wrap (boolean) — wrap-around edges instead of wall collisions
- snakeColor (string, any valid CSS color)
- foodColor (string, any valid CSS color)
- background (string, CSS color or image URL)
- showDpad (boolean) — show on-screen touch D‑pad when overlay is visible

Examples:
- JSON example:
  {
    "speed": 10,
    "grid": 24,
    "wrap": true,
    "snakeColor": "#16a34a",
    "foodColor": "#f97316",
    "background": "#0a0f1f",
    "showDpad": true
  }

- Image background:
  https://yourdomain.tld/snake?url=https%3A%2F%2Fexample.com%2Fbg.jpg

- Text fallback (key=value lines):
  speed=12
  grid=22
  wrap=true
  snakeColor=#22c55e
  foodColor=#ef4444
  background=#0b1225

Notes:
- CORS must allow your page to fetch the provided URL. If fetching fails, the game continues with defaults.
- If `background` is a valid CSS color, it’s applied to the page background; otherwise it’s treated as an image URL.

## Code Explanation
- index.html
  - Contains the full app: HTML structure, modern CSS, and JavaScript for game logic.
  - Canvas rendering:
    - The board is a square canvas scaled for high-DPI displays.
    - Grid tiles are drawn with a subtle style; snake and food are rounded rectangles.
  - Game state:
    - Config object (speed, grid, wrap, colors) with sensible defaults.
    - State (snake body, direction, food, score, speed).
  - Loop:
    - requestAnimationFrame with a fixed-step accumulator (based on `speed`).
  - Input:
    - Keyboard (Arrow/WASD), Space and P for control.
    - Touch: An optional D‑pad appears in the overlay; taps set direction and can resume/restart.
  - Mechanics:
    - Food spawns on a free cell.
    - Collision with walls or self ends the game (unless wrap is enabled).
    - Speed ramps up over time; best score persists in localStorage.
  - Responsiveness:
    - Canvas resizes to fit the viewport while maintaining a square aspect ratio.
    - High-DPI aware via devicePixelRatio.
  - ?url handling:
    - On load, the app checks `URLSearchParams` for `url`.
    - If present, it fetches the resource with a timeout.
    - JSON: applies recognized keys and restarts the game.
    - Image: uses it as a page background.
    - Text fallback: parses simple key=value lines.
    - All failures are handled gracefully and surfaced via a small toast.

Notable implementation details:
- No external libraries or assets; portable and GitHub Pages-friendly.
- Best score key is namespaced by grid size and wrap setting.
- Background can be set to a color or image; color validity is detected using DOM CSS parsing.
- A lightweight toast is used for user feedback on config loading.

## License
MIT License

Copyright (c) 2025

Permission is hereby granted, free of charge, to any person obtaining a copy of this software and associated documentation files (the "Software"), to deal in the Software without restriction, including without limitation the rights to use, copy, modify, merge, publish, distribute, sublicense, and/or sell copies of the Software, and to permit persons to whom the Software is furnished to do so, subject to the following conditions:

The above copyright notice and this permission notice shall be included in all copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY, FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM, OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE SOFTWARE.