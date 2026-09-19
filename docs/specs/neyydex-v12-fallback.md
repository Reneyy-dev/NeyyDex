# NeyyDex Luau v12 Fallback Specification

## Goal

Keep Dex behavior unchanged for bytecode it can already decompile while adding a deterministic remote fallback for Roblox Luau bytecode version 12.

## Inputs

The integration may use these executor capabilities when present:

- `getscriptbytecode`
- `request`, `http_request`, `syn.request`, `http.request`, or `fluxus.request`
- native `decompile`

Dex's existing Konstant fallback remains available when no native `decompile` function exists and the existing prerequisites are present.

## Dispatch behavior

1. Capture the native or existing Dex decompiler as the legacy path.
2. If both `getscriptbytecode` and an HTTP request function are available, install a wrapper as `env.decompile`.
3. The wrapper obtains raw bytecode with `getscriptbytecode`.
4. If the first byte is exactly `12`, POST the raw bytecode to the Neyy decompiler Worker using `Content-Type: application/octet-stream`.
5. Accept executor response status fields `StatusCode`, `Status`, or `status_code`, and response body fields `Body` or `body`.
6. A remote result is valid only when status is HTTP 200 and the body is a non-empty string.
7. If the v12 remote request fails and the legacy/native decompiler exists, fall back to it.
8. Non-v12 bytecode stays on the legacy/native path.
9. If the wrapper prerequisites are unavailable, expose only the legacy/native path.

## Endpoint

`https://neyy-luau-decompiler.apa-kah1337.workers.dev/decompile`

## Security constraints

NeyyDex must not contain Cloudflare deployment credentials, API tokens, or account secrets. The public Worker endpoint is runtime infrastructure only.

## Attribution

The original Dex implementation remains attributed to Moon and the preserved MIT-licensed Dex lineage. Reneyy-dev attribution applies to the NeyyDex integration and maintenance, not sole authorship of Dex.

## Compatibility boundary

This integration intentionally routes only bytecode version 12 to the remote backend. It does not claim universal support for every Roblox Luau version, opcode, constant encoding, or debug-info format.
