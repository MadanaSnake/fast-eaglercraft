# Fast-Eaglercraft — Optimized Eaglercraft 26.2

> **Info:** Optimized entirely by AI *(yes, I vibecoded it :[ )*

Playable online at: **[https://madanasnake.github.io/fast-eaglercraft/](https://madanasnake.github.io/fast-eaglercraft/)**

This build of **Eaglercraft 26.2 (Wasm-GC + Wispcraft)** keeps the core game, assets, and multiplayer/proxy features untouched while optimizing startup speed, chunk handling, and frame pacing.

---

## What Was Optimized

- **Faster Startup & Decompression:**
  - 4 MB Brotli decompression chunks and parallel short-lived decode workers (`server-worker.wasm` + `terrain.wasm` decompress concurrently).
  - Fast `Uint8Array.fromBase64` / indexed decoding for Epoxy WASM (~1.27M fewer callback calls).
  - Deferred Wispcraft/Epoxy initialization until the title screen is ready (or first network request).
- **Smoother Frame Pacing & Less Stutter:**
  - Eliminated periodic 2-second main-thread `localStorage` + `JSON.stringify` crash-journal freezes.
  - Replaced `new Date().getTime()` with zero-allocation `Date.now` in `TeaVM.wasmGC` (main thread + workers).
  - Zero-overhead native `WeakRef` shim and self-detaching `AudioContext` unlock listeners.
  - WebGL2 `high-performance` GPU hint, `contain: strict` compositor isolation, and splash DOM cleanup after load.
- **Chunk & Worker Tuning (Baked Defaults):**
  - `chunkUnloadHardCap = true` by default *(override with `?chunkunload=uncapped`)*.
  - `meshWorkerCount = 2` by default *(override with `?meshworkers=1..4` or `?meshworkers=auto`)*.
- **Seperate task**
  - As you can see on top, 1 HTML of Eaglercraft 26.2 is split up, each file has their own thing (like `sounds.epk` for sfx/music) for faster loading time. If your loading time is somehow slow, blame your ISP or you having bad internet.

---

## Optional URL Flags

You can still override defaults anytime via URL parameters:
- `?chunkunload=uncapped` — Disable the chunk-unload cap
- `?meshworkers=auto` or `?meshworkers=1..4` — Change mesh worker thread count
- `?device=desktop-low` — Cap pixel ratio to `1.0` for extra FPS on HiDPI screens

---

## Overall review

I would recommended to try this fork (even tho there is no sign of forking in this repo) because how efficient the ram takes, how faster loading time is (similar loading time), how good it is for future improvement and anything else.... 

---

## Upcoming next

- **Modding**
  - Oh i forgor tis one, i make several mods for Eaglercraft since they implement internal modding using Lua, its pretty easy to understand but my brainded head still using AI to do that (best programming langugage is English). But haven't release it yet so just wait....
  - Mod repo link: [under da cunstroction](youtube.com)
- **Compare the peformance**
  - Upcoming video next, i had the file but my days, week, months, and year is full of other thing such as eating, showering, go to school, play battle cats, go to home, eat, sleep, repeat (okay did i just talk abt my self?)

--- 

## Credits & Note

<a href="https://eaglercraft.com" target="_blank">Eaglercraft</a> 26.2 by [lax1dude](https://github.com/lax1dude) and the <a href="https://eaglercraft.com" target="_blank">Eaglercraft</a> team.
<a href="https://github.com/MercuryWorkshop/wispcraft" target="_blank">Wispcraft</a> (Epoxy / Wisp proxy integration) by Mercury Workshop.

Note: This repository only hosts a shell/loader optimization of the single-file HTML build—all core game binaries, assets, and proxy technology belong to their original creators.
