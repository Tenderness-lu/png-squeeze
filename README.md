# png-squeeze

[![make.fun · png-squeeze-01-run-20261127](https://make.fun/badge/png-squeeze-01-run-20261127.svg)](https://make.fun/bounty/png-squeeze-01-run-20261127?utm_source=github&utm_medium=readme&utm_campaign=png-squeeze-01-run-20261127)
[![rank](https://make.fun/badge/png-squeeze-01-run-20261127/Tenderness-lu/png-squeeze/rank.svg)](https://make.fun/bounty/png-squeeze-01-run-20261127/leaderboard?utm_source=github&utm_medium=readme&utm_campaign=png-squeeze-01-run-20261127)
[![reward](https://make.fun/badge/png-squeeze-01-run-20261127/Tenderness-lu/png-squeeze/reward.svg)](https://make.fun/bounty/png-squeeze-01-run-20261127?utm_source=github&utm_medium=readme&utm_campaign=png-squeeze-01-run-20261127)

Lossless PNG recompression for the make.fun `png-squeeze-01-run-20261127` bounty:
squeeze 1280×720 screenshots into the smallest PNG, byte for byte.

## How it works

`main.ts` (Node 22, stdlib only) reads each `/input/<id>/image.png` and writes
`/output/<id>/image.png`:

1. **Colour-type reduction** — palette (≤ 256 colours, transparent entries first, with
   tRNS), grey, grey+alpha, RGB, RGBA, or a single-transparent-value tRNS key when
   every non-opaque pixel is the same fully transparent value.
2. **Per-row filter screening** — fixed filters 0/1/2/4, minimum signed-sum, fewest
   distinct byte pairs, and brute force (each of the five filters deflated per row at
   level 4 against the previous 32 KiB, smallest kept).
3. Every candidate is screened by zlib level 6; the winner is re-compressed at level 9
   (default and filtered strategies, smallest kept).

Deterministic: fixed work per input, no clock or environment reads, so two runs write
identical bytes.

## Run

```sh
node main.ts
```

Reads `/input/<id>/image.png` (or `$MAKEFUN_INPUT_DIR`), writes
`/output/<id>/image.png` (or `$MAKEFUN_OUTPUT_DIR`), one run for the whole set.

## License

MIT
