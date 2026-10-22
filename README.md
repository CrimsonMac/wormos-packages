# WormOS packages

The core package repository of WormOS. The packages are the assets of the
[`core` release](https://github.com/CrimsonMac/wormos-packages/releases/tag/core);
WormOS's package manager, `wh`, downloads them from there:

```
https://github.com/CrimsonMac/wormos-packages/releases/download/core/
```

## What is in the release

- `index` and `index.sig`: the list of packages with their versions, sizes
  and SHA-256 sums, and its Ed25519 signature. `wh update` refuses an index
  with a bad signature or one older than the index it already has, and
  every package must match the sum the index gives for it.
- `NAME-VERSION-RELEASE.wpkg`: the packages (zstd-compressed tar archives).
- `NAME-VERSION-RELEASE.src.tar`: the source of each package built from
  upstream code: its build recipe, any patches and extra files, and the
  upstream source archives it was built from.
- `wormos-build-tools.tar`: the scripts every recipe is built with (the
  cross toolchain and kernel scripts, `wbuild`, `wpack`, `wrepo`).
- `SHA256SUMS`: checksums of the source archives.

The signing key's public half, built into every WormOS system as
`/usr/share/wh/keys/core.pub`:

```
3cfa4cea0b2994c70ab1889c965fa8b11d4bb365f63a029bfcec097670f1fe7d
```

## Licences

Every package keeps the licence of its upstream project; the licence
texts are in the source archives. The firmware package holds Intel's GPU
firmware unchanged, under the licences it ships with
(`/usr/share/licenses/linux-firmware`).

Packages with WormOS's own programs (wh, snap, worm, the desktop shell
and others) are distributed as binaries only.
