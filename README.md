<p align="center">
  <img src="assets/logo.svg" width="88" height="88" alt="SeqAlign" />
</p>

<h1 align="center">SeqAlign</h1>

<p align="center">
  An interactive visualiser for DNA sequence alignment — <b>Needleman-Wunsch</b> (global)<br/>
  and <b>Smith-Waterman</b> (local), one cell at a time.
</p>

<p align="center">
  <a href="https://zmy16.github.io/seqalign/"><b>Open the demo →</b></a>
</p>

<p align="center">
  <img alt="HTML5" src="https://img.shields.io/badge/HTML5-E34F26?style=flat&logo=html5&logoColor=white" />
  <img alt="CSS3" src="https://img.shields.io/badge/CSS3-1572B6?style=flat&logo=css3&logoColor=white" />
  <img alt="JavaScript" src="https://img.shields.io/badge/JavaScript-F7DF1E?style=flat&logo=javascript&logoColor=black" />
  <img alt="No build step" src="https://img.shields.io/badge/build-none-14140f" />
</p>

![SeqAlign interface](assets/preview-top.png)

## Why this exists

Alignment algorithms are usually taught as a formula. Here the DP matrix fills in one cell at a
time, the way the calculation would be written on a whiteboard. Every cell shows its three
candidates (diagonal, up, left) next to the MAX value that was actually used, with the arithmetic
spelled out beside it.

![Matrix and traceback](assets/preview-matrix.png)

## Features

- **Two algorithms** — Needleman-Wunsch (global) and Smith-Waterman (local), switchable without a reload.
- **Cell-by-cell stepping** — fill the matrix one cell at a time, or all at once.
- **Per-cell breakdown** — `diagonal + score`, `up + gap`, `left + gap`, then MAX.
- **Visual traceback** — the optimal path is marked in accent, and the resulting alignment is shown as stacked rows.
- **Presets** — Kelompok 4, Simple, Contoh Gap, Panjang.
- **Editable scoring** — match, mismatch, and gap penalty.
- **Exports** — matrix to PNG and Excel (a real `.xlsx`, written without external libraries).
- **Light/dark themes** — two complete palettes, contrast verified in both.
- **Keyboard** — `Enter` step, `F` fill, `T` traceback, `Ctrl+Z` undo, `Esc` close dialog.

## Usage

1. Open `index.html` in a browser — nothing to install — or use the [live demo](https://zmy16.github.io/seqalign/).
2. Pick an **algorithm**, enter **Sequence A** and **Sequence B** (A/C/G/T/N, 15 bases max), or load a preset.
3. Adjust **match / mismatch / gap** if needed, then hit **Generate**.
4. Use **Langkah →**, **Isi semua**, then **Traceback**.

## Presets

| Preset | Sequence A | Sequence B | Match | Mismatch | Gap |
|---|---|---|---|---|---|
| Kelompok 4 | `CTACGCATC` | `CAAGCCTAC` | +2 | +1 | −1 |
| Simple | `ACGT` | `AGT` | +2 | −1 | −2 |
| Contoh Gap | `AGTACG` | `ACATAG` | +2 | +1 | −1 |
| Panjang | `ATCGATCGATCG` | `ATGCATGCATGC` | +1 | −1 | −2 |

## How it works

**Needleman-Wunsch (global)**

1. First row and column are filled with multiples of the gap penalty.
2. Each cell: `MAX(diagonal + match/mismatch, up + gap, left + gap)`.
3. Traceback runs from the bottom-right corner back to the top-left.

**Smith-Waterman (local)**

1. First row and column are filled with zeros.
2. Each cell: `MAX(0, diagonal + match/mismatch, up + gap, left + gap)` — negatives are clamped to zero.
3. Traceback starts at the highest-scoring cell and stops when the score reaches zero.

## Project structure

```
seqalign/
├── index.html          # Page markup, dialog, and form
├── assets/
│   ├── logo.svg        # A mini DP matrix with its traceback diagonal
│   ├── script.js       # DP algorithms, traceback, rendering, exports
│   ├── style.css       # Design tokens and all styling
│   ├── preview-top.png
│   └── preview-matrix.png
├── .github/workflows/  # Automatic deploy to GitHub Pages
└── README.md
```

## Tech

- **Plain HTML, CSS, and JavaScript** — no framework, no build step, no `node_modules`.
- **IBM Plex Sans & Plex Mono** — one superfamily, for prose and for numbers.
- **html2canvas** — lazy-loaded from a CDN, only when exporting a PNG.
- **Hand-written Excel export** — the `.xlsx` is assembled as a ZIP (`crc32` plus headers) containing SpreadsheetML XML.
- **Strict Content-Security-Policy** — all user input is escaped before it reaches the DOM.

## Design

The visual direction is a "sequence plate": paper and ink, 1px hairlines, and a single accent
(vermilion) reserved for the **MAX value** and the **traceback path**. Hierarchy comes from weight
and size rather than colour, and every number is set in `tabular-nums` so the columns do not shift
as the matrix fills in.

## License

Built for educational use (Bioinformatics).
