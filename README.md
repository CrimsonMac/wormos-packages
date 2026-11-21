<div align="center">

# WormOS packages

**The signed package repository of [WormOS](https://github.com/CrimsonMac/WormOS).**

[![Package repository check](https://github.com/CrimsonMac/WormOS/actions/workflows/package-repo.yml/badge.svg)](https://github.com/CrimsonMac/WormOS/actions/workflows/package-repo.yml)
[![Browse the packages](https://img.shields.io/badge/browse-core%20release-7c3aed)](https://github.com/CrimsonMac/wormos-packages/releases/tag/core)

</div>

This repository is WormOS's equivalent of an apt or pacman mirror: the
packages its package manager, `wh`, installs and upgrades. Everything lives
in the assets of a single release, [`core`](https://github.com/CrimsonMac/wormos-packages/releases/tag/core),
which every WormOS system reads from:

```
https://github.com/CrimsonMac/wormos-packages/releases/download/core/
```

It holds packages only. WormOS itself is released on
[its own release page](https://github.com/CrimsonMac/WormOS/releases), and
WormOS Amnesia on [its own](https://github.com/CrimsonMac/WormOS-Amnesia/releases).

## Using it

On WormOS, nothing needs to be set up:

```sh
wh update          # fetch and verify the package index
wh search ffmpeg   # look for a package
wh install ffmpeg  # install it (with a snapshot first)
wh upgrade         # bring every package up to date
```

## What is in the release

| File | What it is |
|---|---|
| `index`, `index.sig` | Every package with its version, size, SHA-256 and dependencies, and the index's Ed25519 signature |
| `NAME-VERSION-RELEASE.wpkg` | The packages (zstd-compressed tar archives) |
| `NAME-VERSION-RELEASE.src.tar` | The complete source of each package built from upstream code: its build recipe, WormOS's patches and the unmodified upstream archives |
| `wormos-build-tools.tar` | The scripts every recipe is built with: the cross toolchain and kernel scripts, `wbuild`, `wpack`, `wrepo` |
| `SHA256SUMS` | Checksums of the source archives |

## How it is kept safe

- `wh update` accepts only an index signed with the WormOS key, and refuses
  one older than the index it already has, so a mirror replaying an old
  index cannot hold back security updates.
- Every package must match the size and SHA-256 that the signed index
  gives for it, or it is not installed.
- Every day, [a check](https://github.com/CrimsonMac/WormOS/actions/workflows/package-repo.yml)
  verifies the published index as a WormOS system would: its signature,
  every package against the size and SHA-256 the index gives, the
  dependencies and the source archives.

The public half of the signing key, built into every WormOS system as
`/usr/share/wh/keys/core.pub` (and in the WormOS repository as
[`keys/core.pub`](https://github.com/CrimsonMac/WormOS/blob/main/keys/core.pub)):

```
3cfa4cea0b2994c70ab1889c965fa8b11d4bb365f63a029bfcec097670f1fe7d
```

To check the index by hand, from a WormOS source checkout:

```sh
curl -fsSLO https://github.com/CrimsonMac/wormos-packages/releases/download/core/index
curl -fsSLO https://github.com/CrimsonMac/wormos-packages/releases/download/core/index.sig
build/host/wsign verify keys/core.pub index   # wsign is built by tools/wrepo
```

or run `scripts/check-package-repo.sh` there, which checks everything above.

## Building the packages yourself

Each package's recipe is in [`recipes/`](https://github.com/CrimsonMac/WormOS/tree/main/recipes)
of the WormOS repository, and [docs/building.md](https://github.com/CrimsonMac/WormOS/blob/main/docs/building.md)
explains how to build them. The source archives here contain the same
recipe together with the exact upstream sources a package was built from.

## Licenses

Every package keeps the license of its upstream project; the license texts
are in the source archives and installed in `/usr/share/licenses`. The
firmware package holds Intel's GPU firmware unchanged, under the licenses
it ships with. The packages of WormOS's own programs (wh, snap, worm, the
desktop shell and the rest) are free software under the GPL-3.0-or-later;
their source is the [WormOS repository](https://github.com/CrimsonMac/WormOS).
See [docs/legal.md](https://github.com/CrimsonMac/WormOS/blob/main/docs/legal.md)
for details.
