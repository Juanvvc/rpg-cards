# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

A Vue 3 + Vuetify 4 single-page app for browsing tabletop RPG card decks. The landing view shows all available decks as a cover grid; selecting one opens a carousel to browse its cards. No backend — all data is static JSON served from `public/data/`.

## Commands

```bash
npm install      # install dependencies
npm run dev      # dev server at http://localhost:3000
npm run build    # production build
npm run preview  # preview production build locally
```

## Stack

- **Vite 8** + **Vue 3.5** + **Vuetify 4** + **@mdi/font 7**
- Vuetify components are auto-imported via `vite-plugin-vuetify`
- Roboto loaded via Google Fonts `<link>` in `index.html`
- All components use `<script setup>` Composition API

## Architecture

### Data layout

`public/data/index.json` — array of deck JSON paths, loaded by `App.vue` on mount:

```json
["data/deck_a.json", "data/deck_b.json"]
```

Each deck JSON:

```json
{
  "style": "<CSS fallback applied to all cards>",
  "width": 400,
  "height": 600,
  "cycle": false,
  "cover": { "title": "...", "style": "..." },
  "cards": [
    { "title": "...", "text": "...", "footer": "...", "style": "..." }
  ]
}
```

- `cover` is rendered at index 0. Navigation counters are relative to `cards`, not `cover`.
- Per-card `style` overrides the deck-level `style`. Images use `background-image` in `style` — there is no dedicated image field.
- `cycle`: when `true`, navigation wraps from the last card back to the cover.
- The env var `VITE_DECK_INDEX` (default `data/index.json`) sets the index path.

### Component tree

```
App.vue          — fetches index, manages list↔deck navigation, owns shuffle logic
  DeckList.vue   — fetches all deck JSONs in parallel, renders cover grid; emits 'select'
  Deck.vue       — v-carousel wrapper; prev/next/home/shuffle buttons; position counter
    Card.vue     — v-sheet displaying title / text / footer with Vuetify typography classes
```

`App.vue` shows `DeckList` when no deck is selected; clicking a cover fetches that deck's JSON and switches to `Deck`. A back button returns to the list.

`Deck.vue` computes `availableCards = [cover, ...cards]` so index 0 is always the cover. `currentCard` (v-model on `v-carousel`) is the zero-based position within `availableCards`.

### Adding a new card field

1. Add the field to the deck JSON schema (document it in `README.md`).
2. Pass it as a prop from `Deck.vue` to `Card.vue`.
3. Render it in `Card.vue` with a `v-if` guard (same pattern as `title`, `text`, `footer`).
