# Minesweeper Teacher

A Minesweeper implementation built to teach beginners *why* a move is safe, rather than just letting them guess. Alongside the usual board, it computes the probability that each covered square hides a mine, explains the reasoning behind those probabilities in plain language, and can replay an AI solver's moves one step at a time.

Final term project for 15-112 (Fundamentals of Programming and Computer Science) at Carnegie Mellon, December 2019.

## What it does

**Probability overlay.** Press `p` and every covered square adjacent to a revealed number gets a probability of containing a mine, displayed as a fraction. These come from a constraint solver rather than a naive per-square count, so squares constrained by several numbers at once get the correct joint probability.

**Explanations.** Clicking a probability tile brings up an explanation of how that number was derived. The game scans for the well-known number runs — `11`, `12`, `121`, and `1221` — in all four directions from the clicked square, and when it finds one it explains the deduction in terms of that pattern rather than raw arithmetic. Squares that aren't part of a recognized pattern fall back to the general explanation: the fraction of consistent solutions in which that square holds a mine.

**Strategy reference.** Press `s` for a summary of the standard patterns and the deductions they license.

**AI solver.** After a game ends, run the solver to watch how the "teacher" would have played the same board. Its moves are recorded as a sequence you can step through with the arrow keys, forward and backward, so a losing board becomes a post-mortem.

The solver uses a tank-style constraint search: it enumerates the mine arrangements consistent with the revealed numbers, then derives per-square probabilities from how often a square holds a mine across all valid arrangements. Squares that are mines in every arrangement are certain, squares that are mines in none are provably safe, and everything else falls somewhere in between.

## Running it

Requires Python 3 with `tkinter` (included in most Python installations). No third-party packages needed.

```bash
python minesweeper.py
```

`cmu_112_graphics.py` must be in the same directory — it's the course's animation wrapper around tkinter and is included in this repo.

## Controls

| Input | Action |
| --- | --- |
| Left click | Uncover a square |
| Right click | Flag a square |
| `p` | Toggle the probability overlay |
| Click a probability tile | Explain that probability |
| `s` | Show common strategies |
| `←` / `→` | Step back / forward through the AI's moves |

Board size and mine count are set in `appStarted` in [minesweeper.py](minesweeper.py). Presets for the standard beginner (9×9, 10 mines), intermediate (16×16, 40 mines), and expert (16×30, 99 mines) difficulties are defined there. Note that the checked-in default is a small hardcoded 4×4 test board — change `self.rows`, `self.cols`, and `self.mines` and use the commented-out board initializer for a randomly generated game.

## Files

- [minesweeper.py](minesweeper.py) — the game, probability engine, explanation logic, and solver
- [cmu_112_graphics.py](cmu_112_graphics.py) — course-provided tkinter animation framework
- [scrap.py](scrap.py) — scratch work kept from development
- [Project Proposal.docx](Project%20Proposal.docx) — the original project proposal
