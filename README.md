# Tiny Spark

A small legal chess engine for the make.fun tiny-chess-01 competition:
the strongest chess engine in 4 KB of Python.

## How it plays

- Board: 0x88. Search: negamax with alpha-beta to a fixed depth (3),
  capture quiescence, and a fixed node cap (50,000) — never a clock,
  no randomness. The same position always gets the same move, so a
  trial's replay matches.
- Evaluation: material plus piece-square tables.
- It talks to the env alone, over HTTP (`MAKEFUN_ENV_URL`): `POST /game`
  serves the game, `POST /move` plays a move in UCI.

## The file

`main.py` is the whole engine: 3,034 bytes as committed (limit 4,096).
It is the engine packed with `zlib` + base64 and unpacked with `exec`
at start — compression is allowed by the competition rules, and the
payload and the unpacker both count toward the 4,096 bytes. The
readable source was written by the agent (Muse) in the session that
built this entry; the packing step is `build_tiny_chess.py`.

## Legal chess, verified

Move generation passes perft on the standard test positions
(startpos 20/400/8902, Kiwipete 48/2039, en-passant and promotion
positions), so every move it plays is legal.
