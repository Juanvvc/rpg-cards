# RPG Deck Viewer

A browser-based card deck viewer for tabletop RPGs. The app opens on a grid showing all available decks; click one to browse its cards one by one, or shuffle them for a random draw. Cards can display a title, body text, a footer, and a custom background (solid color or image).

Built with Vue 3, Vuetify 4, and Vite 8. No backend required — all data is static JSON.

## Docker

```bash
docker compose up --build
```

Opens at `http://localhost:8080`.

## Local development

```bash
npm install
npm run dev      # http://localhost:3000 with hot-reload
npm run build    # production build → dist/
npm run preview  # serve the dist/ build locally
$env:VITE_DECK_INDEX = 'others/index.json' ; npm.cmd run dev  # Run with a custom index.json in Windows
```

## Adding decks

The list of available decks is defined in `public/data/index.json`:

```json
[
  "data/my_deck.json",
  "data/another_deck.json"
]
```

Each entry is a path relative to the site root. Place the deck JSON files (and any image assets) inside `public/` so they are served as static files.

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
| `cycle` | When `true`, navigation wraps from the last card back to the cover |
| `cover` | Cover card shown at position 0 and in the deck selection grid |
| `cards[].title` | Large heading text |
| `cards[].text` | Body text |
| `cards[].footer` | Small italic footer text |
| `cards[].style` | Per-card CSS that overrides the deck-level `style` |

A working example deck is in `public/data/test_deck.json`.

## Image credits

- `game-card-back.jpg`: Public domain — https://www.publicdomainpictures.net/en/view-image.php?image=156326
- `paper-texture.jpg`: CC0 — https://www.publicdomainpictures.net/en/view-image.php?image=14384
