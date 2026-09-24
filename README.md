# X9UInfinityX Infinity-X 4.0 local manifest

This branch contains only the X9UInfinityX platform repositories already ported
to Infinity-X 4.0 (Android 17). Device, kernel, camera, hardware, and proprietary
vendor manifests are intentionally not included yet.

## Initialize and sync

Create and enter an empty source directory:

```bash
mkdir -p ~/android/X9UINFIX4.0
cd ~/android/X9UINFIX4.0
```

Initialize the upstream Infinity-X Android 17 manifest:

```bash
repo init --no-repo-verify --git-lfs \
  -u https://github.com/ProjectInfinity-X/manifest \
  -b 17 \
  -g default,-mips,-darwin,-notdefault
```

Clone this local manifest after `repo init` and before `repo sync`:

```bash
git clone -b 4.0 \
  https://github.com/X9UInfinityX/local_manifests \
  .repo/local_manifests
```

Sync the source tree:

```bash
repo sync -c --no-clone-bundle --no-tags --optimized-fetch --prune \
  --force-sync -j$(nproc --all)
```

To update an existing local-manifest checkout later:

```bash
git -C .repo/local_manifests pull --ff-only
repo sync -c --no-clone-bundle --no-tags --optimized-fetch --prune \
  --force-sync -j$(nproc --all)
```

## Scope

`x9u-4.0.xml` overrides only the X9UInfinityX repositories that currently have
a maintained `4.0` branch. Launcher3 is not overridden because its required fix
is already present upstream on Android 17.

This branch does not yet provide a complete buildable OPPO Find X9 Ultra device
manifest. Device-specific repositories will be added after their Android 17
ports are ready.
