# tiny-chess-01

A chess engine in 4,096 bytes of Python. `main.py` is the whole engine: a
compressed (LZMA) payload holding the readable source, which it unpacks and
execs. The engine is a 0x88 negamax search — iterative deepening with a
transposition table, killer moves, null-move pruning, late-move reductions
and quiescence over captures, promotions and check evasions — with a fixed
node budget per move (no clock, no unseeded randomness, so a trial replays
move for move). The budget is 40,000 nodes a move, about a second a move on
this machine and well inside the 10-second move limit. Evaluation is material plus classic piece-square tables,
with the king tapered from middlegame to endgame and a bishop-pair bonus.

It plays the make.fun gauntlet through the env at `MAKEFUN_ENV_URL`
(`POST /game` serves the game in play, `POST /move` plays a move) and exits
when every game is over.

## Run it

```sh
MAKEFUN_ENV_URL=http://127.0.0.1:8080 python3 main.py
```

## Build it

The readable source is `engine.py` (kept out of the submitted snapshot;
`makefun bundle` sends it beside the transcripts). Pack it with the kit:

```sh
node <kit>/cli/makefun.mjs compress engine.py --out main.py
```

## Tests

Perft from the start position (20 / 400 / 8902 / 197281) and from the
Kiwipete position (48 / 2039 / 97862) all match, the evaluation is
deterministic, and the engine plays 40 plies of self-play without crashing.
