# N-Queens Visualizer — Next.js version

Interactive N-Queens backtracking visualizer: step-by-step animation with
conflict highlighting, bidirectional playback, attack-zone overlay, a full
solution browser (total + fundamental counts), and a live source view — for
boards of 4 to 12 queens.

Built with **Next.js 16 · React 19 · Tailwind CSS 4 · shadcn/ui**.

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
