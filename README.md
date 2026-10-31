# MyOS Store repository

This is a public HTTPS catalog for MyOS 2's Store. Add the catalog URL below in
**Store → Repositories**:

```text
https://raw.githubusercontent.com/ciroparada81-boop/myos2-store/main/index.txt
```

The Store currently supports MyScript source files (`.my`), not native ELF
programs or archives. It downloads each source over HTTPS, checks the SHA-256
listed in `index.txt`, and saves it under `/home/store`.

## Catalog format

Each non-comment line has exactly six pipe-separated fields:

```text
id|name|version|description|https-download-url|sha256
```

Package IDs contain lowercase letters, digits, hyphens, or underscores.
Descriptions and other fields must not contain `|`. Package URLs must be
direct HTTPS links ending in `.my`; redirects are not supported. Source files
must be at most 128 KiB.

SHA-256 detects a mismatch between the catalog and the downloaded file; it is
not a digital signature. Review scripts before running them: MyScript programs
can access files in your `/home`.

To publish a package, add its `.my` source under `apps/`, calculate its digest:

```sh
sha256sum apps/example.my
```

Then add or update the corresponding line in `index.txt` and push the changes
to the `main` branch.

## Included programs

**Utilities and demos**
- **Hola MyOS** - a simple welcome window.
- **Paleta** - choose colors with the mouse.
- **Pizarra** - free drawing with colors, brush size and eraser.
- **Cronometro** - stopwatch with laps and big seven-segment digits.
- **Mandelbrot** - fractal explorer, click to zoom (integer math).
- **Juego de la vida** - Conway's cellular automaton.
- **Lluvia digital** - Matrix-style screensaver.

**Games**
- **Pong** - two players, W/S and the arrow keys.
- **2048** - slide and merge tiles with the arrow keys.
- **Buscaminas** - minesweeper (F toggles flag mode).
- **Tres en raya** - against the PC or a friend.
- **Memoria** - find the 8 pairs.
- **Rompeladrillos** - breakout with levels and lives.
- **Invasores** - space invaders with waves.
- **Simon** - repeat the growing color sequence.

## MyScript notes for authors

- There are no character literals: `'w'` is a syntax error. Letters arrive from `key()` as ASCII codes (`w` = 119, `W` = 87).
- Numbers are integers only. `%` exists; there are no shift operators.
- The mouse position is refreshed inside `key(ms)`, so mouse-driven programs should call `key(20)`-ish in their loop
  and detect clicks by comparing `mouseb()` with its previous value.
- Keep sources ASCII-only (the console font is Latin-1, UTF-8 would show up garbled).
