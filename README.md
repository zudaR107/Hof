# Hof

Hof ("yard"/"courtyard" in German) is the meta-repo for the **Schloss
platform** — a small suite of self-hosted personal services. It doesn't
contain any application code itself; it's docs plus git submodules pinning
the four service repos to the commits that make up the current release.

## The platform

- [`schloss`](https://github.com/zudaR107/schloss) — the home page / launcher
- [`schlussel`](https://github.com/zudaR107/schlussel) — auth: accounts, login, tokens
- [`kuvert`](https://github.com/zudaR107/kuvert) — envelope budgeting
- [`tor`](https://github.com/zudaR107/tor) — the reverse-proxy gateway all of the above sit behind

Each is independently developed, tested, and deployed (its own CI, its own
Docker images, its own issue tracker). This repo exists to answer "what
versions of each service go together" and to hold cross-cutting planning
docs that don't belong in any single service's history.

## What's in here

- `ROADMAP.md` — development history and status across all four repos.
- `docs/stages/` — one file per development stage, with what was built and
  why.
- Four git submodules (`schlussel/`, `schloss/`, `kuvert/`, `tor/`), each
  pinned to a specific commit.

## Getting the code

```sh
git clone --recurse-submodules git@github.com:zudaR107/Hof.git
# or, if you already cloned without --recurse-submodules:
git submodule update --init --recursive
```

## Running the platform

See [`tor/README.md`](https://github.com/zudaR107/tor#readme) — one
`docker compose up` from `tor/` starts everything (`schloss`, `schlussel`,
`kuvert`, and the gateway itself) behind a single address, no ports to
remember.

## Updating this repo

This repo is committed to rarely, by design — only when bumping a submodule
pointer to a service's new release, or updating cross-cutting docs. Regular
development happens in the four service repos themselves, each with its own
issue/PR workflow. There's no branch protection here for that reason.

## License

AGPL-3.0 — see [LICENSE](LICENSE). Each submodule carries its own copy of
the same license.
