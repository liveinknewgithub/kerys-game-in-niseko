# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

This is a browser-based ski resort tycoon game built entirely in vanilla HTML/CSS/JavaScript. The main game is a single-file application (`ski-resort-tycoon.html`) with no build system or external dependencies.

## Development

**Running the game**: Open `ski-resort-tycoon.html` directly in a browser, or use a local server:
```bash
python3 -m http.server 8000
```

**No build/test/lint commands** - this is a zero-dependency vanilla JS project.

## Architecture

### Main Game (`ski-resort-tycoon.html`)

Single-file architecture with embedded CSS and JavaScript (~2800 lines total):

**Core Systems:**
- **Isometric Grid Engine**: Uses `gridToScreen()` and `screenToGrid()` for coordinate conversion. Grid dimensions are dynamic based on land tier (40x50 to 120x150).
- **Game State**: Global variables manage `buildings[]`, `guests[]`, `animals[]`, `coins`, `gems`, `dayNumber`, etc. State is saved to localStorage.
- **Building System**: `BUILDINGS` constant defines ~100 building types across categories (lifts, slopes, buildings, decor). Each has `cost`, `income`, `width`, `height`, and `unlockAt` threshold.
- **Economy**: Buildings generate income per tick. Guests spawn and use lifts/slopes visually. Gems earned through milestones and daily bonuses.

**Key Functions:**
- `gameLoop()` - Main update/render cycle via requestAnimationFrame
- `placeBuilding()` / `isValidPosition()` - Building placement with collision detection
- `calculateResortRating()` - Star rating based on building variety/count
- `drawBuilding()` - Renders buildings with isometric sprites using canvas 2D
- `saveGame()` / `loadGame()` - localStorage persistence

**Player Roles:**
- `CREATOR_NAME` ('Kery') gets creator mode with free builds
- `ADMIN_NAMES` array defines admin users
- Regular players earn coins through gameplay

**Audio System:**
Procedural music generation using Web Audio API (`initAudio()`, `playMusicLoop()`). Creates ambient background music with chord progressions.

### Secondary Files

- `skiing-game.html`: Separate mini-game (endless runner style skiing)
- `og-image.html`: Canvas-based OG image generator for social sharing

## Key Patterns

- All rendering uses HTML5 Canvas 2D context
- Isometric projection: `TILE_WIDTH=64`, `TILE_HEIGHT=32`
- Pan/zoom via mouse drag updates `offsetX`/`offsetY`
- Building unlocks gated by `totalSpent` threshold
- Weather system affects gameplay (powder days = 2x income)
