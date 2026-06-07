# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

A Vue 3 + Vuetify 4 single-page app that displays a deck of cards as a carousel. Cards can have a title, body text, footer text, and inline CSS styling (including background images). No backend — all data is static JSON served from `public/data/`.

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

### Deck configuration

The deck JSON path is set via `.env`:

```
VITE_DECK_DATA=data/test_deck.json
```

`App.vue` fetches that JSON at mount time and passes the parsed object as `deckInfo` to `<Deck>`.

### Deck JSON schema

```json
{
  "style": "<CSS string applied to all cards as fallback>",
  "width": 400,
  "height": 600,
  "cycle": false,
  "cover": { "title": "...", "style": "..." },
  "cards": [
    { "title": "...", "text": "...", "footer": "...", "style": "..." }
  ]
}
```

- `cover` is always rendered as index 0 (the deck face). Navigation shows position relative to `cards`, not `cover`.
- Per-card `style` overrides the deck-level `style`. Images use `background-image` in the `style` field — there is no dedicated image field.
- `cycle`: when `true`, navigation wraps from the last card back to cover.

### Component tree

```
App.vue       — fetches JSON, owns shuffle logic, shows snackbar on shuffle
  Deck.vue    — v-carousel wrapper; prev/next/home/shuffle buttons; card position counter
    Card.vue  — v-sheet displaying title / text / footer with Vuetify typography classes
```

`Deck.vue` computes `availableCards = [cover, ...cards]` so index 0 is always the cover. `currentCard` (v-model on `v-carousel`) is the zero-based position within `availableCards`.

### Adding a new card field

1. Add the field to the JSON schema (document it in `README.md`).
2. Pass it as a prop from `Deck.vue` to `Card.vue`.
3. Render it in `Card.vue` with a `v-if` guard (same pattern as `title`, `text`, `footer`).
