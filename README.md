# Maze Maker

A maker for printable mazes for young kids (around age 5). Each maze has a picture at the way in (a mouse, a
bee, a rocket…) and what it's looking for at the way out (cheese, a flower, the moon…). The sheets have no
words, so they work for kids who can't read yet, and they're made for a black-and-white printer. The answer is
hidden in a QR code: scan it with a phone camera to watch a red line draw the way through.

Live at **https://tb-x.github.io/maze-maker/**

## How it plays

- Print a sheet. Each maze has a picture above its way in and another below its way out.
- Draw a line from the top picture, through the gaps, to the bottom picture, without crossing a wall.
- With stars turned on, there are stars along the way to pick up. They all sit on the one right way through,
  so a child who gets all of them has found it.
- Every maze has exactly one way through.
- Scan the QR code at the top right of the maze. The answer page draws the way through slowly in red, and the
  goal picture hops when the line gets there. Tap the maze to draw it again.

## The maker page

- **Size**: how many squares across, from Tiny (4) through Giant (18) up to Mega (24), Monster (30) and Enormous
  (36) for big kids and grown-ups. How many rows the maze has depends on how much room there is (Enormous is
  36 × 43 on a full page). Sizes whose corridors would be thinner than 5 mm are greyed out, so Giant and bigger
  need 1 maze per page.
- **Pictures**: mouse → cheese, bee → flower, rocket → moon, car → house, bunny → carrot, or A mix (each maze on a
  page gets different ones). Every picture is a black outline, so it can be coloured in.
- **Stars to pick up**: none, 2, 3 or 5.
- **Per page**: 1, 2 (side by side, tall and thin) or 4 mazes on each A4 page.
- **Pages**: how many pages to print at once (up to 20).
- **New mazes** makes new mazes for every page, ↻ on a page makes new ones for that page only, and **Print**
  prints them. Changing the pictures or stars keeps the same mazes. The settings are remembered in this browser.
- Clicking a QR code on screen opens its answer page.

The QR code holds the whole maze in the link: its width and height, the entrance and exit columns, the pictures,
the number of stars, then every inner wall as one bit, 13 bits to each 4 digits, e.g. `#8x8s3g4t4k3.0136211407…`.
The digits go into the QR code in its numeric mode, which packs them almost as tightly as raw bits. A big maze
still needs a bigger QR code, so it grows to keep every dot at least 0.4 mm (up to about 33 mm for Enormous).
Nothing is stored anywhere, so a printed sheet keeps working for as long as this page is online. The QR always
points at the published page on GitHub Pages, even when the maker runs on a local server.

## Running it

It's a single `index.html` with no build step. Serve the folder and open it:

```bash
python -m http.server 5217
```

Then go to http://localhost:5217/. Opening `index.html` straight from disk works too. The QR codes are made
by [qrcode-generator](https://github.com/kazuhikoarase/qrcode-generator) 1.4.4 (MIT), loaded from cdnjs.

After the first visit the page also works offline and can be added to a phone's Home Screen.

## Tuning knobs

Named constants at the top of the script in `index.html`:

| Constant | Default | What it does |
| --- | --- | --- |
| `DEFAULTS` | Small, a mix, no stars, 1 a page, 1 page | Starting settings |
| `SIZES` | 4, 6, 8, 11, 14, 18, 24, 30, 36 across | The sizes in the Size list, and their names |
| `STARS` | 0, 2, 3, 5 | The choices under "Stars to pick up" |
| `SHEETS_MAX` | 20 | Most pages printed at once |
| `GRID` | 1, 2 (side by side), 4 (2 × 2) | How the mazes sit on a page |
| `MAX_CELL` / `MIN_CELL` | 26 / 5 mm | Widest corridor, and the narrowest one allowed |
| `PIC_CELLS`, `PIC_MIN` / `PIC_MAX` | 1.7 squares, 13 / 30 mm | Size of the start and goal pictures |
| `WALL` | 0.1 | Wall thickness, as a share of a square |
| `NEWEST` | 0.75 | 1 makes long winding corridors with few dead ends, 0 makes lots of short dead ends |
| `QR_SIZE` | 24 / 22 / 19 mm | Smallest answer QR code with 1, 2 or 4 mazes a page |
| `QR_MODULE` | 0.4 mm | Smallest QR dot: big mazes get a bigger QR code so a phone can still read it |
| `PUBLIC_URL` | `https://tb-x.github.io/maze-maker/` | Where the QR codes point |
