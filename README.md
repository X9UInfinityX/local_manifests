# X9UInfinityX Infinity-X 4.0 local manifest

This branch contains the X9UInfinityX repositories already available for
Infinity-X 4.0 (Android 17), including the compatible lighthouse device,
common, camera, prebuilt-kernel, and hardware-support repositories. It also
includes the required SM8850 thermal/USB projects and Linux 6.12 kernel build
dependencies.

## Initialize and sync

Create and enter an empty source directory:

```bash
mkdir -p ~/android/X9UINFIX
cd ~/android/X9UINFIX
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

`x9u-4.0.xml` includes only X9UInfinityX repositories that currently have a
maintained `4.0` branch. Launcher3 is not overridden because its required fix is
already present upstream on Android 17.

The two SM8850 compatibility projects continue to track their maintained
`lineage-23.2` branches in OnePlus-SM8850-Development. The kernel-platform
manifest retains the Android 16-based Linux 6.12 revisions required by the
device while the Android userspace is based on Infinity-X 17.
