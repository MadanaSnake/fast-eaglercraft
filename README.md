<p align="center">
  <img width="128" height="128" src="https://github.com/user-attachments/assets/9df5d3a8-5720-4e3d-9a64-2dd8137b2a14"
</p>
  
# Fast-Eaglercraft — Optimized Eaglercraft

> **Info:** Optimized entirely by AI *(yes, I vibecoded it :[ )*

Playable online at: **[https://madanasnake.github.io/fast-eaglercraft/](https://madanasnake.github.io/fast-eaglercraft/)**

This build of **Eaglercraft (Wasm-GC + Wispcraft)** keeps the core game, assets, and multiplayer/proxy features untouched while optimizing startup speed, chunk handling, and frame pacing.

- I Recommended using 26.2 for stability/performance on both of your devices (Desktop & Mobile). 

## What's new?

- Version 26.3 is now live! Both 26.2 and 26.3 are available on the landing page, and you can switch between them anytime.
- Touch mode is added externally because the Eaglercraft version is not customizeable. Dont turn off the "Touch Mode" in Controls setting unless you want to lose control or want to use keyboard & Mouse
- I Make a bridge to make Eaglercraft can connect Perfectly fine to normal Java Server :D. heres the repo link: [https://github.com/MadanaSnake/eaglercraft-bridgetcp](https://github.com/MadanaSnake/eaglercraft-bridgetcp)

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
- **Separate assets**
  - As you can see on top, 1 HTML of Eaglercraft is split up, each file has their own thing (like `sounds.epk` for sfx/music) for faster loading time. If your loading time is somehow slow, blame your ISP or you having bad internet (if you're using the online version)

---

## Repo layout

Each version keeps its build and payloads in its own folder. `play.html` stays at the repo root as a redirect stub, so old `/play.html` links (with their URL flags) keep working.

| Path | What it is |
| --- | --- |
| `index.html` | Launcher: version select, touch toggle, key guide |
| `play.html` | Redirect stub → `26.2/index.html`, keeps `?query` and `#hash` |
| `sounds.epk` | Shared by both versions (byte-identical, 25.9 MB): one copy, one browser cache entry |
| `26.2/index.html` + `26.2/*` | 26.2 build and its 5 payloads |
| `26.3/index.html` + `26.3/*` | 26.3 build and its 5 payloads |
| `1.12.2/index.html` + `1.12.2/assets.epk` | Official EaglercraftX 1.12.2 WASM-GC build: 12.8 KB page + 17.3 MB payload |
| `1.8.8/index.html` + `1.8.8/assets.epk` | Official EaglercraftX 1.8.8 WASM-GC build: 13.1 KB page + 12.6 MB payload |

All 11 payloads are byte-identical (SHA-256) to the ones embedded in the optimized single-file builds. The picker starts on **26.2** (the recommended one); 26.3 stays listed above it.

The two official folders are the upstream downloads with their single `EAG$WASM` payload moved next to the page (the page keeps only `assetsURI = "assets.epk?v=<hash>"`). Nothing inside the payload was touched — it is the exact bytes that were base64-encoded in the download. These are unoptimized upstream builds: no fork tuning. Both connect to the [bridge](https://github.com/MadanaSnake/eaglercraft-bridgetcp) over `wss://`: 1.12.2 with protocol `340`, 1.8.8 with protocol `47` (retry once if the first handshake times out). The optimized versions are 26.2/26.3.

---

## Optional Tweak

**URL Flags**

You can still override defaults anytime via URL parameters:
  - `?chunkunload=uncapped` — Disable the chunk-unload cap
  - `?meshworkers=auto` or `?meshworkers=1..4` — Change mesh worker thread count
  - `?device=desktop-low` — Cap pixel ratio to `1.0`, one mesh worker, lower memory
  - `?device=desktop|mobile|mobile-low` — Force a specific device profile
  - `?touch=custom` — Force the extra on-screen touch overlay (same as the launcher checkbox)
  - `?client=full|lite` — Force the client module. The lite client is not shipped yet, so `lite` currently falls back to `full`.

**Chrome Flags**

By using these flags, it may improve your performance a little bit 
- `#enable-vulkan` — Enables vulkan as primary renderer (Recommended on Linux)
- `#default-angle-vulkan` — Set Vulkan ANGLE to be defaulted on your browser (Recommended on Linux)
- `#vulkan-from-angle` — Makes the browser engine (Skia) share the exact same Vulkan instance with ANGLE (Recommended on linux)
Note: In Vulkan situation, always check if Vulkan is applied to your browser. Visit `chrome://gpu`

If these flags do something to you, you can reset it by opening Terminal/Command Prompt and type `chrome.exe --no-experiments` or on Linux Terminal `google-chrome --no-experiments` (change `google-chrome` with your default browser like `chromium` or `brave-browser` for example)

---

## Overall review

I would recommended to try this fork (even tho there is no sign of forking in this repo) because how efficient the ram takes, how faster loading time is (similar loading time), how good it is for future improvement and anything else.... 

---

## Upcoming next

- **Modding**
  - Oh i forgor tis one, i make several mods for Eaglercraft since they implement internal modding using Lua, its pretty easy to understand but my brainded head still using AI to do that (best programming langugage is English). But haven't release it yet so just wait....
  - Mod repo link: coming soon (no link yet)
- **Compare the peformance**
  - Upcoming video next, i had the file but my days, week, months, and year is full of other thing such as eating, showering, go to school, play battle cats, go to home, eat, sleep, repeat (okay did i just talk abt my self?)

--- 

## Credits & Note

<a href="https://eaglercraft.com" target="_blank">Eaglercraft</a> by [lax1dude](https://github.com/lax1dude) and the <a href="https://eaglercraft.com" target="_blank">Eaglercraft</a> team.
<a href="https://github.com/MercuryWorkshop/wispcraft" target="_blank">Wispcraft</a> (Epoxy / Wisp proxy integration) by Mercury Workshop.

Note: This repository only hosts a shell/loader optimization of the multiple assets of the HTML. Build—all core game binaries, assets, and proxy technology belong to their original creators.

---

# Why am i making this?

~~I Wanna get put my Google AI Pro to use. thats it :>~~ ~~Jio Promotional Google AI Pro is expiring my subscription :(~~
I found an alternative, its DeepAI :D
