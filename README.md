# OPPO Find X9 Ultra Infinity-X 3.12 local manifests

Local manifests and build instructions for Infinity-X 3.12 on the OPPO Find X9
Ultra (`lighthouse`).

## What these manifests include

- OPPO Find X9 Ultra device, common-device, camera, kernel, hardware, and
  Qualcomm trees on their maintained main branches.
- The Android 16 Linux 6.12 kernel-platform dependencies.
- X9UInfinityX platform forks on the `3.12` branch.

## Requirements

- A Linux build host configured for Android builds.
- `repo`, Git, and Git LFS.
- Enough storage and memory for a complete Android source checkout and build.
- The full OPPO Find X9 Ultra ColorOS **16.0.9.403** firmware for proprietary files.

## Initialize a fresh source tree

Create and enter an empty source directory:

```bash
mkdir -p ~/android/X9U
cd ~/android/X9U
```

Initialize the upstream Infinity-X Android 16 manifest:

```bash
repo init --no-repo-verify --git-lfs \
  -u https://github.com/ProjectInfinity-X/manifest \
  -b 16 \
  -g default,-mips,-darwin,-notdefault
```

Clone these local manifests **after `repo init` and before `repo sync`**:

```bash
git clone -b 3.12 \
  https://github.com/X9UInfinityX/local_manifests \
  .repo/local_manifests
```

Sync the complete source tree, including every project selected by the local
manifests:

```bash
repo sync -c --no-clone-bundle --no-tags --optimized-fetch --prune \
  --force-sync -j$(nproc --all)
```

## Add the manifests to an existing initialized tree

From the root of a tree that has already been initialized with `repo init`:

```bash
git clone -b 3.12 \
  https://github.com/X9UInfinityX/local_manifests \
  .repo/local_manifests
```

If `.repo/local_manifests` already contains personal manifests, back them up or
merge them into this checkout first. Do not overwrite local manifests you still
need. Then run the full `repo sync` command shown above.

To update an existing clone of this manifest repository later:

```bash
git -C .repo/local_manifests pull --ff-only
repo sync -c --no-clone-bundle --no-tags --optimized-fetch --prune \
  --force-sync -j$(nproc --all)
```

## Extract proprietary files

Download the full **16.0.9.403** firmware for the OPPO Find X9 Ultra. Extract its
logical partition images, then unpack those images into partition directories.
The extraction source should contain directory trees such as:

```text
F9UEU/
├── odm/
├── product/
├── system/
├── system_ext/
└── vendor/
```

Run the following commands from the Android source root, setting `STOCK_ROOT`
to the directory containing the unpacked logical partitions:

```bash
source build/envsetup.sh
lunch infinity_lighthouse-user

export PYTHONPATH="$PWD/tools/extract-utils"
export STOCK_ROOT="/path/to/extracted/F9UEU"

python3 device/oppo/lighthouse/extract-files.py \
  --only-target "$STOCK_ROOT"

python3 device/oneplus/sm8850-common/extract-files.py \
  --only-target "$STOCK_ROOT"

python3 device/oppo/lighthouse-camera/extract-files.py \
  --only-target "$STOCK_ROOT"
```

Replace `/path/to/extracted/F9UEU` with the path containing your unpacked logical
partition directories. All three extraction commands must complete successfully
before building.

## Build Infinity-X

Build the `user` variant from the source root:

```bash
source build/envsetup.sh
lunch infinity_lighthouse-user
m bacon -j$(nproc --all)
```

Build artifacts are written under:

```text
out/target/product/lighthouse/
```

## Notes

- Use `infinity_lighthouse-user`, not `userdebug`.
- Keep the firmware source at 16.0.9.403 when regenerating proprietary files.
- Run `repo sync` from the Android source root.
- `--force-sync` may replace Git metadata for projects whose upstream source is
  overridden by these local manifests. Commit or back up personal changes first.
