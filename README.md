# Aegis — tiny-chess-01 entry

A compact Python chess engine built for make.fun's tiny-chess-01
(4 KB engine limit). One file, standard library only, fully
deterministic: it searches to a fixed node budget, never a clock,
so a replayed trial plays the same moves byte-for-byte.

## Approach

- Mailbox-64 board, pseudo-legal movegen with make/unmake legality
  checks (perft-verified: startpos depth 2 = 400, depth 3 = 8,902).
- Negamax alpha-beta with iterative deepening under a fixed budget
  of 25,000 nodes per move (~0.5–1.8 s/move locally, worst < 5 s).
- Transposition table keyed by Zobrist hashes from a fixed seed,
  with exact/lower/upper bound flags.
- Quiescence search over captures and promotions, stand-pat, with
  simple delta pruning.
- Move ordering: TT move, captures by MVV-LVA, then two killer
  moves per ply.
- Evaluation: material (100/320/330/500/900) plus piece-square
  tables tapered between middlegame and endgame by phase, and a
  bishop-pair bonus.

## Size

The minified engine source is ~6.2 KB, over the 4,096-byte cap, so
`main.py` ships it as an lzma-compressed payload with a two-line
unpacker — a form the competition rules expressly allow (the house
baseline does the same). The packed `main.py` is 3,724 bytes.

## License

MIT (see LICENSE).
