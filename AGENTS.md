# lib-rand-js

JavaScript implementation of the `rand` ports via the `js` ffi.
Pure `.hnl`, single module (`src/index.hnl`, all 23 contract arrows).

## Layout

| path | role |
| --- | --- |
| `src/index.hnl` | all 23 contract arrows in one module |
| `pkg.hnl` | manifest — `modules: ["rand-js"]`, deps `rand` + `js` |
| `scripts/hnl-gate.sh`, `workflows/gate.yaml` | the gate / bupha entry |

## Contract

- Unseeded arrows derive everything from `next_u32` —
  `Math.random()` scaled to `[0, 2^32)` through `js.call`, mirroring
  `rand-c`'s entropy surface; `sqrt`/`log`/`cos`/`exp` go through
  `js.call("Math", ...)`, `pi` is a literal.
- The `seeded_*` family uses a pure-handl PRNG (`_smix`: a
  Numerical-Recipes LCG seeded by `(seed, k)`, four mixing rounds —
  deterministic, not cryptographic) since `Math.random` cannot be
  seeded.
- Import from `"rand-js"` — single module, no per-arrow submodules.

## Verification

```bash
HNL=../host-cli/target/debug/hnl scripts/hnl-gate.sh
```

CI skips the gate while `src/` has no `.hnl` sources (see `bupha.toml`).

## License

MIT.

<!-- myco:agent-awareness -->

## Agent awareness

- Session start / any work discussion: run `myco status --prompt`
  before proposing next steps — know the board first.
- User describes new work → offer to capture it (`myco todo add`,
  `issue new`, `proposal new`). Don't wait to be asked.
- Picking up a task: `myco resume <ID>` is the briefing.
- Wrapping up: `myco handoff <ID>` anything resumable; keep statuses
  honest (claimed → in-progress → blocked/done).

<!-- /myco:agent-awareness -->
