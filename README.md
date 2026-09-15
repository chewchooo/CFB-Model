# CFB-Model

Hosted data and page for **CFB Edge**, an opponent-adjusted game-outcome board for
college football.

- `cfb-edge/index.html` — the board, self-contained
- `cfb-edge/data.js` — the current slate, ratings and measured verdicts
- `cfb-edge/meta.js` — a small stamp for polling

**This repository is a delivery mechanism, not the project.** The model, the backtest
and every measurement live in the Jarvis vault; only built artefacts are published here.
The history is deliberately held at a single commit — a data file recommitted weekly
would grow the repo by that much each time for nothing.

## The model

    points = expected possessions x points per possession

Team quality comes from a ridge regression over every drive in the training window,
solving for all teams simultaneously, so a rating reflects the opponents faced rather
than the schedule drawn. Garbage time and end-of-half drives are excluded. Pace is
projected separately, because tempo and quality are different facts.

The projected margin decomposes exactly into three terms — offence gap, defence gap,
home field — and the board prints all three on every row.

## What it claims, measured

Walk-forward over 3844 games, 2022-2026. For each week, ratings were fitted only on
drives from earlier weeks.

| | model | baseline |
|---|---|---|
| Straight-up winner | **73.4%** | 64.2% always-home |
| Edge over baseline | **+9.1pp** [7.2, 11.2] | |
| Margin MAE | 13.37 | market **12.04** |
| Against the spread | 50.4% | 52.4% break-even |
| Brier | **0.171** | 0.230 base rate |

The straight-up edge is real. The ATS record is not, and the board says both in the
same typeface — it is 1.33 points of margin error worse than the closing spread.

Betting lines are a median of pregame sportsbooks and are **never an input** to the
model; they exist only so the board can be measured against them.
