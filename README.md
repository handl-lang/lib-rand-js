# lib-rand-js

JavaScript implementation of the `rand` ports (via the `js` ffi).

Unseeded arrows derive everything from `next_u32` — `Math.random()`
scaled to `[0, 2^32)` through `js.call` — mirroring `rand-c`'s entropy
surface; `sqrt`/`log`/`cos`/`exp` go through `js.call("Math", ...)`,
`pi` is a literal. The `seeded_*` family uses a pure-handl PRNG
(`_smix`: a Numerical-Recipes LCG seeded by `(seed, k)`, four mixing
rounds — deterministic, not cryptographic) since `Math.random` cannot
be seeded.

## Layout

```text
src/index.hnl  the .hnl sources (all 23 contract arrows)
pkg.hnl        package manifest — modules, deps, description, license
ci.sh          bupha entry → scripts/hnl-gate.sh
```

Dependencies: rand, js

## Gate

```bash
HNL=../host-cli/target/debug/hnl scripts/hnl-gate.sh
```

CI skips the gate while `src/` has no `.hnl` sources.

## License

MIT.
