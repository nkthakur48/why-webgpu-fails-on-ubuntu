# The cube that wouldn't spin

Slides for **"Why WebGPU works (or doesn't) on your Ubuntu desktop"**, a beginner talk for UbuCon.
Speakers: Aishwarya Mathuria and Nishant Kumar Thakur.

Repo: <https://github.com/nkthakur48/why-webgpu-fails-on-ubuntu>

A web page asks for your GPU and gets one of three answers: it works, it fails loudly, or it works slowly
because the CPU is doing the GPU's job. The talk shows how to tell which one you have, using five commands
and two Ubuntu 26.04 VMs (UTM on an Apple GPU).

## Open the slides

The slides are plain HTML with a bundled copy of [reveal.js](https://revealjs.com). No build step.

```bash
python3 -m http.server 8000
# then open http://localhost:8000/slides-interactive.html
```

Use a local server, not `file://`, so the demo videos in `assets/` play.

Press `S` for speaker notes (timings and facts are in them). Everything the slides need is in this repo:
`slides-interactive.html`, `assets/` (photos, QR codes, story art, demo videos) and `vendor/` (reveal.js).

## The five checks

```bash
sudo apt install vulkan-tools pciutils mesa-utils
lspci -k | grep -A3 -i vga                                      # 1. GPU and kernel driver
vulkaninfo --summary 2>/dev/null | grep -E 'deviceName|deviceType'   # 2. Vulkan driver (llvmpipe = CPU)
echo $XDG_SESSION_TYPE                                          # 3. Wayland or X11
snap list | grep -i -E 'firefox|chromium'                       # 4. How the browser is installed
firefox --version                                               # 5. Which build
```

Then open <https://webgpureport.org> and check `isFallbackAdapter`:
`false` means a real GPU, `true` means the CPU is pretending.

## What we found (our VM, not a general rule)

Same laptop, same GPU: Firefox 158 (snap) ran WebGPU, and Chromium 156 (snap) did not. We checked the
Vulkan driver, snap confinement (`opengl` connected, no AppArmor denials), the snap's Mesa and launch
flags, and still don't know exactly why. The takeaway: **if one browser fails, try a second one before
blaming your GPU.** Facts in the deck are as of November 2026.

## Licence

© 2026 Aishwarya Mathuria and Nishant Kumar Thakur. Slides and text are licensed under
[CC BY 4.0](LICENSE): reuse them freely, but credit the authors and link to this repo.
reveal.js (in `vendor/`) is MIT-licensed by Hakim El Hattab and contributors.
