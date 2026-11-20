[![make.fun](https://make.fun/badge/tiny-chess-01.svg)](https://make.fun/bounty/tiny-chess-01)
[![rank](https://make.fun/badge/tiny-chess-01/s8fr2hf/tiny-chess-01/rank.svg)](https://make.fun/bounty/tiny-chess-01)
[![reward](https://make.fun/badge/tiny-chess-01/s8fr2hf/tiny-chess-01/reward.svg)](https://make.fun/bounty/tiny-chess-01)

# tiny-chess-01

A small, strong chess engine in one Python file, built for the
[make.fun tiny-chess-01 bounty](https://make.fun/bounty/tiny-chess-01).
`main.py` is the whole entry: it is 4,077 bytes, self-contained, and plays
a full game against the house gauntlet through the harness.

## How it plays

- 0x88 board, negamax alpha-beta with iterative deepening.
- Transposition table (exact entries only), MVV-LVA capture ordering, and
  the table move searched first so cutoffs come early.
- Quiescence search over captures, promotions, and every reply to a check.
- Tapered piece-square tables (PeSTO-style) with material folded in, so
  the score already counts pawn 100, knight 320, bishop 330, rook 500,
  queen 900.
- Fixed node budget of 200,000 nodes per move — no clock, no randomness,
  so the same position and history always give the same move. A move
  takes about 2–3 seconds on this machine, inside the 10-second limit.

## Files

- `main.py` — the packed entry (decompresses and runs itself).
- `engine.py` — the readable source (not part of the submission).
- `LICENSE` — MIT.

## Run it

The harness serves positions over HTTP; point the engine at it and it
plays until every game is over:

```sh
MAKEFUN_ENV_URL=http://127.0.0.1:8080 python3 main.py
```

It talks only to `MAKEFUN_ENV_URL` (`/game`, `/move`) and nothing else.
