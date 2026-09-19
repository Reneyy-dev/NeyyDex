# NeyyDex v12 Integration Plan

## Scope

Integrate the existing Neyy Luau decompiler Worker into Dex without rewriting Dex's Script Viewer or changing normal non-v12 decompile behavior.

## Tasks

1. Import the current Dex baseline from `infyiff/backup` pinned to commit `50f904455e3d55efc133d4c547c8acfd6796d272`.
2. Preserve the upstream MIT license notice from the Dex lineage.
3. Replace only Dex's decompiler initialization block with a wrapper that:
   - obtains raw bytecode,
   - routes version 12 to the Neyy Worker,
   - accepts common executor request response field variants,
   - returns successful source to the existing Script Viewer,
   - falls back to the existing native/legacy decompiler when appropriate.
4. Change the loader Creator label from `Developed by Moon` to `Created by Moon x Reneyy-dev` while preserving Moon attribution.
5. Add public documentation covering loader usage, attribution, compatibility scope, and the Worker boundary.
6. Keep deployment credentials out of the public Dex source.

## Verification gates

- Baseline anchors must exist exactly once before patching.
- The imported license must contain the upstream MIT notice.
- The Worker endpoint must occur exactly once after patching.
- Raw bytecode requests must use `application/octet-stream`.
- The v12 route guard must check `string.byte(bytecode, 1) == 12`.
- Native/legacy fallback must remain present.
- The Creator label must be replaced exactly once.
- No `CLOUDFLARE_API_TOKEN` string may exist in `dex.lua`.
- GitHub Actions bootstrap must complete successfully before review.

## Post-merge runtime check

Run the public `main` loader in an executor with `getscriptbytecode` and HTTP request support, open a known v12 script in Dex Script Viewer, and confirm that the displayed source matches a direct Worker decompile of the same bytecode. This runtime check supplements static verification; it does not expand the declared compatibility scope.
