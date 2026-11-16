# magic-coin — torogao-coin

A clean-room, from-measurement reimplementation of the systems of
**Magic Coin** (DLsite RJ318915; Unity, Mono), built for the Torogao
remake programme by Nice Thoughts Ranch. No assets, text, or code from
the studied game are included: every constant in
`crates/torogao-coin/data/coin-dynamics.ron` was measured from the
original's behaviour and rebuilt as estate-authored data and Rust
systems.

## Repository shape

This repository is a standalone Cargo workspace with two crates:

- `crates/torogao-coin` — the systems crate (below), carrying the
  `coin-smoke` and `coin-play` binaries.
- `crates/torogao-coin-android` — the Android spike shell: the coin
  fixture running inside a winit NativeActivity window (Torogao
  D-212 Route 1). Not the Torogao app; no rendering beyond a
  success-colour fill.

The shape mirrors the Torogao workspace (Owner ruling D-1568,
2026-11-16), where this work was built and gated. The Torogao
graphical app host lives in the Torogao repository and consumes
`torogao-coin` from there; the dependency runs app → coin, never
the reverse, so the host is not part of this artefact.

## What the crate models

- The timed amplitude swing: points bank only on direction crossings,
  as (max displacement since crossing)² × 1.5.
- The three-stat preparation economy: gauge = points × (hp+1)(mp+1)(sp+1)
  × coefficient, with day-minute costs per preparation action.
- Level-scaled resistance as a table (gauge maxima
  0.5 / 4.8 / 15 / 30 / 70); the session timer is constant across levels.
- Day-frame sessions with saving only at day's end.

## Dual-target law

The remake programme ships every title on native Linux **and** Android
from this workspace. The systems and data live in `torogao-coin` only;
platform shells (the Android spike shell here, the Torogao app host
in its own repository) bind to the crate.

## Determinism

Sessions are deterministic: a fixed input stream replays to a fixed
trajectory hash (FNV-1a over the banked-points/gauge stream). The
trajectory fixture is exercised by the test suite, and the same hash
must match on Linux and Android (the programme's S7 gate).

## Build and test

```sh
cargo build
cargo test
cargo run -p torogao-coin --bin coin-smoke   # scripted session; prints the smoke line
cargo run -p torogao-coin --bin coin-play    # playable day loop (line protocol on stdin/stdout)
```

## License

BSD 2-Clause. See `LICENSE`. The studied game remains the property of
its respective developer; this repository is an independent
reimplementation of measured behaviour.
