# N-Queens Visualizer — Next.js version

Interactive N-Queens backtracking visualizer: step-by-step animation with
conflict highlighting, bidirectional playback, attack-zone overlay, a full
solution browser (total + fundamental counts), and a live source view — for
boards of 4 to 12 queens.

Built with **Next.js 16 · React 19 · Tailwind CSS 4 · shadcn/ui**.

## Quick start

```bash
npm install
npm run dev
# open http://localhost:3000
```

Production build:

```bash
npm run build
npm start
```

> Prefer zero setup? The `standalone/` folder at the repo root contains the
> same visualizer as a single dependency-free `index.html` — just open it in
> a browser (or serve it on GitHub Pages).

## Project layout

```
src/
├── app/
│   ├── page.tsx                 # server component; reads nqueens.ts for the code tab
│   └── layout.tsx               # metadata + fonts
├── components/
│   ├── nqueens/
│   │   ├── NQueensApp.tsx       # shell: tabs + board-size slider (4–12)
│   │   ├── AnimatePanel.tsx     # playback controls, rAF loop, stats, shortcuts
│   │   ├── ChessBoard.tsx       # grid, attack tint, SVG conflict lines
│   │   ├── SolutionsPanel.tsx   # solution browser + notation
│   │   └── CodePanel.tsx        # "How It Works" cards + live source
│   └── ui/                      # shadcn/ui primitives used by the app
└── lib/
    └── nqueens.ts               # engine: NQueensAnimator, counting, enumeration
```

## How the animation works

The backtracking search is implemented as a **generator** that yields one
atomic event per action (`try`, `conflict`, `place`, `backtrack`, `solution`).
`NQueensAnimator` wraps the generator and keeps a compact undo log (each event
packed into a single 32-bit integer), so the playhead can move **forward and
backward** without re-running the search. The UI applies events through a
`requestAnimationFrame` loop with a logarithmic speed slider (1–2,000
events/second).

Keyboard: `Space` toggles play, `←`/`→` step one event, "Next solution" fast-forwards
to the next complete board.

## Notes

- `src/app/page.tsx` reads `src/lib/nqueens.ts` from disk at request time to
  display the real source in the "How It Works" tab. This works with
  `next dev` and `next start`; other deployment targets may need the source
  inlined instead.
- Solution counts are computed in the browser with the classic three-bitmask
  recursion; fundamental counts use canonical representatives over the eight
  board symmetries (values match OEIS A000170 / A002562).
