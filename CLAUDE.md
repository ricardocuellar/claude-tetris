# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Overview

Vanilla JavaScript Tetris using HTML5 Canvas. No dependencies, no build step, no tests, no linter. The README is written in Spanish, as are the in-game UI strings ("PAUSA", "Puntuación") — keep new user-facing text in Spanish.

## Running

- Open `index.html` directly, or serve statically: `python3 -m http.server 8000` then visit `http://localhost:8000`.

## Architecture

All game logic lives in a single script, `game.js` (loaded by `index.html`, styled by `style.css`). It relies on DOM element ids defined in `index.html` (`board`, `next-canvas`, `score`, `lines`, `level`, `overlay`, `overlay-title`, `overlay-score`, `restart-btn`) — renaming any of them requires updating the lookups at the top of `game.js`.

Key design points:
- **Global mutable state** (`board`, `current`, `next`, `score`, `lines`, `level`, `paused`, `gameOver`, `dropInterval`, ...) is declared in one `let` and reset by `init()`, which is also the restart handler.
- **Board** is a `ROWS×COLS` 2D array of color indices (0 = empty). Indices 1–7 map into both `PIECES` (shape matrices) and `COLORS`, so adding a piece means adding to both arrays and updating the `Math.random() * 7` in `randomPiece()`.
- **Game loop**: `requestAnimationFrame` `loop(ts)` accumulates time into `dropAccum` and gravity-drops when it exceeds `dropInterval`, then calls `draw()`. Pause/game-over cancel the frame via `animId`; unpausing restarts `loop`.
- **Piece lifecycle**: `lockPiece()` → `merge()` → `clearLines()` (updates score/level/`dropInterval`) → `spawn()` (promotes `next`, detects game over when the spawn position collides).
- **Rotation** is clockwise matrix rotation with simple horizontal wall kicks `[0,-1,1,-2,2]` (not SRS).
- **Scoring**: line clears use `LINE_SCORES × level`; soft drop gives +1 per cell, hard drop +2 per cell. Level = `floor(lines/10)+1`; `dropInterval = max(100, 1000 - (level-1)*90)`.
- **Input**: single `keydown` listener (arrows, `X` rotate, `Space` hard drop, `P` pause).
