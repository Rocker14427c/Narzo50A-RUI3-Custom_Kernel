# Kernel SusFS / ReSukiSU Inspection Report

Repo: https://github.com/Rocker14427c/Narzo50A-RUI3-Custom_Kernel
Branch: master
Kernel version: 4.14.186 (arm64 / k68v1_64_defconfig)
Inspection date: 2026-08-01

## 1. Full codebase scan (first pass)
- Cloned master (commit ef0463c7b, parent cbe880986).
- The "Add ReSukiSU and SuSFS support" commit (cbe880986) added KernelSU folder + kernel patches.
- Key modified files: fs/susfs.c, include/linux/susfs.h, include/linux/susfs_def.h, fs/namei.c, fs/namespace.c, fs/exec.c, fs/open.c, fs/proc/*, drivers/input/input.c, arch/arm64/configs/k68v1_64_defconfig, KernelSU/.
- Missing from upstream 4.14 susfs4ksu patch set: fs/dcache.c, fs/devpts/inode.c, fs/inode.c — NOT required for this newer SUSFS version that uses ND_STATE_LOOKUP_LAST / susfs_def.h instead of direct i_state bit checks.
- No missing KernelSU core files; setup.sh, Kbuild, and Kconfig are intact.

## 2. Line-by-line review of SusFS / ReSukiSU additions (second pass)
### Find defects:
- **drivers/input/input.c logic error**: extern declarations guarded by `CONFIG_KSU`, but hook usage guarded by `CONFIG_KSU_SUSFS`. If `CONFIG_KSU=y` and `CONFIG_KSU_SUSFS=n`, externs exist but hook is skip (okay). If `CONFIG_KSU_SUSFS=y` and `CONFIG_KSU=n` (theoretical, because KSU folder needs CONFIG_KSU to compile), usage tries to reference missing externs — link error. Safe fix: align usage guard to `CONFIG_KSU` so the hook activates only when KernelSU core is compiled.
- **fs/susfs.c include cleanup**: three duplicate `#include <linux/version.h>` lines (lines 1, 15, 23 in original). Removed duplicates — does not affect functionality, reduces compiler noise.
- **fs/susfs.c consistency check**: `susfs_is_inode_sus_path()` correctly version-gated (`#if LINUX_VERSION_CODE >= KERNEL_VERSION(6, 6, 0)` vs `#else`) matching 4.14. `susfs_def.h` defines `ND_STATE_LOOKUP_LAST` etc. matching `fs/namei.c` patches. `fs/namei.c` `nameidata` `state` field added correctly.
- **fs/Makefile**: only `obj-$(CONFIG_KSU_SUSFS) += susfs.o`; upstream 4.14 also has `sus_su.o`, but `CONFIG_KSU_SUSFS_SUS_SU` is not present in this repo’s Kconfig and `sus_su.c` is not included. Adding the line without the file could create a future trap, so left unchanged to stay safe.
- **security/selinux/hooks.c / avc.c**: modifications are properly `#ifdef CONFIG_KSU_SUSFS` guarded and reference `susfs_is_current_ksu_domain()` / static keys correctly.
- **KernelSU/kernel/Kbuild**: references `susfs_compat.mk` correctly; no missing tool files.
- **defconfig**: `CONFIG_KSU=y`, `CONFIG_KSU_SUSFS=y`, and all SUSFS sub-features (`SUS_PATH`, `SUS_MOUNT`, `SUS_KSTAT`, `SPOOF_UNAME`, `ENABLE_LOG`, etc.) set correctly.

## 3. Re-check previously found defects + apply safe fixes
- Applied fix to `drivers/input/input.c` (changed `#ifdef CONFIG_KSU_SUSFS` to `#ifdef CONFIG_KSU` for the hook usage block).
- Applied include cleanup in `fs/susfs.c` (removed duplicate `<linux/version.h>` includes).
- No modifications made to existing kernel features (sched, mm, network, block, etc.). All changes are inside `#ifdef CONFIG_KSU_SUSFS` or `CONFIG_KSU` blocks, or are include cleanups.

## 4. Safety verification
- `drivers/input/input.c`: the `ksu_handle_input_handle_event` is guarded by `static_branch_unlikely(&ksu_is_input_hook_enabled)`. Even when enabled, the static key is off by default unless userspace (`ksud`) turns it on. Changing the guard to `CONFIG_KSU` does not alter runtime behavior; it only ensures the symbol is declared when the hook is referenced.
- `fs/susfs.c`: removing duplicate includes does not change compiled code or ABI.

## 5. Commit
- Changes committed to `master` with message: `Fix susfs/ReSukiSU input hook guard and clean fs/susfs includes`

## 6. Notes for future builds
- Build requires ARM64 cross-compiler (`aarch64-linux-android-gcc` or `aarch64-linux-gnu-gcc`).
- If you intend to enable `CONFIG_KSU_SUSFS_SUS_SU`, you must also add `fs/sus_su.c` (upstream 4.14 version) and update `fs/Makefile`. It is not needed for current feature set.
- If build errors appear in `fs/namei.c`, verify that `include/linux/susfs_def.h` is present (it is) and that `struct nameidata` `state` field is defined by the patch (it is, inside `#ifdef CONFIG_KSU_SUSFS_SUS_PATH`).
