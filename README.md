# RPG Deck Viewer

A simple browser-based card deck viewer for tabletop RPGs. Load any deck defined in a JSON file and flip through the cards one by one, or shuffle them for a random draw. Cards can display a title, body text, a footer, and a custom background (solid color or image).

Built with Vue 3, Vuetify 4, and Vite 8. No backend required.

## Setup

```bash
npm install
```

## Development

```bash
npm run dev
```

Opens at `http://localhost:3000` with hot-reload.

## Production build

```bash
npm run build    # outputs to dist/
npm run preview  # serve the dist/ build locally
```

## Configuring the deck

Set the path to your deck JSON in `.env`:

```
VITE_DECK_DATA=data/my_deck.json
```

Place the JSON file (and any image assets it references) inside `public/`. The path is relative to the site root, so `data/my_deck.json` maps to `public/data/my_deck.json`.

### Deck JSON format

```json
{
  "style": "background-image: url('data/paper-texture.jpg'); background-size: cover;",
  "width": 400,
  "height": 600,
  "cycle": false,
  "cover": {
    "title": "My Deck",
    "style": "background-image: url('data/back.jpg'); background-size: cover;"
  },
  "cards": [
    { "title": "Card title", "text": "Body text", "footer": "Fine print" },
    { "title": "Image card", "style": "background-image: url('data/card1.jpg'); background-size: cover;" }
  ]
}
```

| Field | Description |
|---|---|
| `style` | CSS applied to every card as a fallback |
| `width` / `height` | Pixel dimensions of the card viewer |
| `cycle` | Whether navigation wraps from the last card back to the cover |
| `cover` | Optional cover card shown at position 0 |
| `cards[].title` | Large heading text |
| `cards[].text` | Body text |
| `cards[].footer` | Small italic footer text |
| `cards[].style` | Per-card CSS that overrides the deck-level `style` |

A working example deck is in `public/data/test_deck.json`.

## Image credits

- `game-card-back.jpg`: Public domain — https://www.publicdomainpictures.net/en/view-image.php?image=156326
- `paper-texture.jpg`: CC0 — https://www.publicdomainpictures.net/en/view-image.php?image=14384
