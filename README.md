# 🎨 Neon Turing Patterns

*Reaction-diffusion (Gray-Scott) activator-inhibitor system. Two chemicals diffuse and react, self-organizing into spots, stripes, mazes, and crawling worms. Click to inject activator. Adjust feed, kill, diffusion rates, timestep, color cycling. Presets for classic patterns. Watch emergence.*

---

## What is this?

Alan Turing's reaction-diffusion model describes how two chemicals (an activator and an inhibitor) interacting and diffusing at different rates can produce stable spatial patterns — the mathematical basis for animal coat markings, plant arrangements, and more. The Gray-Scott equations are a popular implementation:
- Activator (A) auto-catalyzes (promotes its own production) and creates inhibitor
- Inhibitor (B) suppresses activator and diffuses faster
- Under the right parameter regimes, the system self-organizes into spots, stripes, labyrinths, or dynamic "worm" states

This interactive simulator lets you explore the parameter space in real time with a neon glow aesthetic.

---

## Features

- **Gray-Scott reaction-diffusion** on a toroidal grid (2D array with periodic boundaries)
- **Preset patterns:** Spots, Stripes, Maze, Worms — each with carefully tuned feed/kill values
- **Adjustable parameters:**
  - Feed (f) — constant replenishment of A; controls overall activity
  - Kill (k) — removal rate of B; critical for pattern selection
  - Diffusion rates: ΔA and ΔI (inhibitor diffuses faster typically)
  - Timestep (dt) — integration step multiplier
  - Color cycle speed — global hue rotation for neon variety
- **Click interaction** — inject activator (set A=1, B=0) in a small region
- **Pause/Resume** and **Randomize** (pick random preset and reinitialize)
- **Stats overlay** — FPS, min/max of activator concentration
- **Single HTML file** — no dependencies

---

## How to Use

1. Open `index.html`
2. The simulation starts with a central seed. Watch the pattern evolve from that seed, spreading outward.
3. **Click** anywhere to add more activator, creating new pattern nuclei.
4. Use the controls:
   - **Preset selector** — switch between Spots, Stripes, Maze, Worms to jump to known interesting regions of parameter space
   - **Feed & Kill** — these are the most sensitive knobs. Tiny changes can completely change the pattern:
     - Lower feed / higher kill → fewer, sparser structures
     - Higher feed / lower kill → denser, sometimes chaotic
   - **Diffusion rates** — ratio of inhibitor to activator diffusion matters; typical is ΔI ≈ 0.5×ΔA. Larger ΔI smooths patterns more.
   - **Time Step** — larger steps simulate faster but can become unstable; start with 1.0
   - **Color Cycle** — adds temporal hue shift
5. Press **Randomize** to pick a random preset and restart from a fresh seed
6. **Pause** to freeze; **Resume** to continue

---

## Technical Details

- **Grid:** The simulation runs on a low-resolution grid (scale = 4 px per cell) to keep O(N) operations manageable. Each cell holds concentrations A and B (floats 0–1).
- **Update rule:** For each cell:
  - Compute Laplacian of A and B using 8-neighbor stencil (toroidal wrap)
  - Apply Gray-Scott equations:
    - ∂A/∂t = D_A ∇²A − A·B² + f·(1−A)
    - ∂B/∂t = D_B ∇²B + A·B² − (k+f)·B
  - Euler integration with timestep dt
- **Rendering:** The A/B grid is converted to an ImageData buffer; cells with B > 0.2 are drawn colored by A intensity + hue cycle; others are transparent (black). The ImageData is then scaled up to full canvas size using nearest-neighbor scaling for that pixel-art aesthetic.
- **Performance:** For a grid of ~500×300 cells, each update step touches ~150k cells; with dt=1.0 and 60fps target, this is light for JS.

---

## The Real Story

Reaction-diffusion patterns are Turing's forgotten legacy. While famous for codebreaking and AI, his 1952 paper on morphogenesis showed how simple physics could explain leopard spots and zebra stripes. The Gray-Scott model is a modern incarnation that exhibits an astonishing zoo of patterns. Tweaking feed/kill by a few thousandths morphs spots into stripes, stripes into mazes, and mazes into wriggling pseudo-organisms. It's a playground of emergence — complex order arising from two simple equations. The neon glow just makes it look like a living circuit board.

---

*Made with 🎨 and diffusion.*

**Repo:** https://github.com/Kiloooai/neon-turing-patterns
