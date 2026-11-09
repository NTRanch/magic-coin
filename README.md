# magic-coin — torogao-coin

A clean-room, from-measurement reimplementation of the systems of
**Magic Coin** (DLsite RJ318915; Unity, Mono), built for the Torogao
remake programme by Nice Thoughts Ranch. No assets, text, or code from
the studied game are included: every constant in
`data/coin-dynamics.ron` was measured from the original's behaviour and
rebuilt as estate-authored data and Rust systems.

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
from this one crate plus its data file. Platform shells bind to the
crate; the systems and data live here only.

## Determinism

Sessions are deterministic: a fixed input stream replays to a fixed
trajectory hash (FNV-1a over the banked-points/gauge stream). The
trajectory fixture is exercised by the test suite, and the same hash
must match on Linux and Android (the programme's S7 gate).

## Build and test

```sh
cargo build
cargo test
cargo run --bin coin-smoke   # scripted session; prints the smoke line
```

## License

BSD 2-Clause. See `LICENSE`. The studied game remains the property of
its respective developer; this repository is an independent
reimplementation of measured behaviour.
