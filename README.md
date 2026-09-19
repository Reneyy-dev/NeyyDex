# NeyyDex

NeyyDex is a modified distribution of Dex with a maintained fallback path for newer Roblox Luau bytecode.

## Credits

- Dex was originally created by Moon.
- This repository preserves the MIT-licensed Dex lineage published by LorekeeperZinnia / Infinite Yield.
- NeyyDex integration and maintenance are by Reneyy-dev.
- The remote Luau fallback is maintained separately in `Reneyy-dev/neyy-luau-decompiler`, based on the open-source `shiny` / `luau-lifter` work credited there.

NeyyDex does not claim the original Dex implementation as work authored solely by Reneyy-dev.

## Loader

```lua
loadstring(game:HttpGet("https://raw.githubusercontent.com/Reneyy-dev/NeyyDex/main/dex.lua"))()
```

## Luau v12 fallback

When both `getscriptbytecode` and an executor request function are available, NeyyDex wraps Dex's decompiler path.

For bytecode whose first version byte is `12`, NeyyDex sends the raw bytecode to:

`https://neyy-luau-decompiler.apa-kah1337.workers.dev/decompile`

The request uses `Content-Type: application/octet-stream`. A successful HTTP `200` response body is returned to Dex's normal Script Viewer. If the remote v12 request fails and a native/legacy decompiler exists, NeyyDex falls back to that path.

For non-v12 bytecode, NeyyDex keeps the native/legacy Dex path rather than routing it to the Worker.

No Cloudflare deployment credential or API token is embedded in NeyyDex.

## Compatibility scope

The v12 backend path has been validated against real Roblox Luau v12 bytecode samples. This is practical compatibility evidence, not a guarantee that every current or future opcode, constant format, debug-info layout, or bytecode version will decompile successfully.

## License

The imported Dex baseline is MIT licensed. See [`LICENSE`](LICENSE) for the preserved upstream notice.
