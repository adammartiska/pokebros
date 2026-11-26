# Buddy Cards Studio

A playful web editor for designing trading cards of your friends: upload a photo, pick an element, add a move, and build a deck. Decks can be downloaded as print-ready files or (later) ordered as printed cards.

"Buddy Cards" is a working name. The card frames are original designs.

## Status

Playable prototype: a single self-contained page, `index.html`, with no build step.

- Three card styles: **Classic** (default), **Retro** and **Modern**
- Simple mode (photo → name, element, HP → move, with an optional second move) plus a **More options** switch for extras: title line, stage, ability, weakness/resistance/retreat, flavor text, made by, shiny frame
- A deck needs at least 5 finished cards (photo + name) before export unlocks
- The deck is saved in the browser's localStorage
- Export:
  - **Card images (.zip):** every card as a 600 dpi PNG, plus the card back
  - **Print sheets (.pdf):** A4, 9 cards per sheet, 300 dpi, fronts and backs
- Ordering printed cards is a mock (no checkout yet)

## Run it locally

Open `index.html` in a browser, or serve the folder:

```bash
npx serve .
# or
python3 -m http.server 8000
```

Fonts (Google Fonts) and jsPDF (cdnjs) load from the internet.

## Print spec

| | |
| --- | --- |
| Card size | 63 × 88 mm, 3 mm corner radius (rounded after cutting with an R3 corner punch; print files are square) |
| Card images | PNG, 600 dpi (1488 × 2079 px) |
| Print sheets | A4 PDF, 3 × 3 cards at 300 dpi, 2 mm bleed per card, crop marks |
| Backs | Every front page is followed by its back page, mirrored for a long-edge flip. The export dialog has an X/Y offset (mm) for alignment tests |
| Printing | Print at 100 % (actual size) on double-sided matte photo paper, laminate with 2 × 80 µm pouches, cut, round the corners |
| Photo check | The editor shows the photo's effective dpi in the card window: ≥ 250 sharp, ≥ 150 OK, below that blurry |

## How the code is organised (`index.html`)

- **Renderer:** one canvas renderer draws the preview, deck thumbnails and export files, in 0.1 mm units (card = 630 × 880). `drawClassic`, `drawRetro` and `drawCard` (Modern) are the three styles; `bodyContent` lays out ability, moves, stats and flavor text and auto-fits them.
- **Editor:** step panes, element picker, HP, moves and extras, bound to the current card in `state.cards`.
- **Deck tray:** thumbnails with copy/remove (remove has an undo).
- **Export:** a small built-in ZIP writer (store, no compression) and jsPDF for the A4 sheets. Inside claude.ai, files are saved through the artifact `downloads` capability; in a normal browser they download directly.

## Next

- Test-print the PDF on the Epson L8180 and tune the back offset
- Real app: accounts, cloud storage of decks, checkout + print order flow, showcase gallery
- Final brand name and card back design
