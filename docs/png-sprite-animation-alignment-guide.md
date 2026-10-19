# Fix jitter in PNG sprite animations: an alignment checklist

Small changes in a sprite's position between PNG frames can make an otherwise smooth animation look shaky. Before redrawing the artwork, check whether the visible subject is anchored consistently.

This guide explains one way to diagnose that problem and preserve intentional movement. It is useful for sprite artists, pixel-art animators, and indie game developers working with transparent PNG frames or a uniformly divided sprite sheet.

## 1. Check the source

Keep an untouched copy of your frames. Confirm the order and preview the animation at its intended speed. Separate three problems:

- **Unwanted position jitter:** the whole sprite shifts around.
- **Intentional movement:** jumps, breathing, recoil, or bobbing that should remain.
- **Different artwork:** moving limbs or changing silhouettes that a simple offset cannot repair.

Alignment only addresses the first of these.

## 2. Choose an anchor

For a standing or walking character, a visible **bottom-center** point is a useful starting point. For centered effects or rotating objects, **visible center** may work better. When the original layout contains deliberate motion, **preserve placement** and make small manual adjustments instead.

No automatic anchor understands anatomy or artistic intent. Inspect the result frame by frame.

## 3. Compare original and aligned playback

Use a ghosted overlay or side-by-side preview if your editor has one. Look for sudden jumps of the torso or feet, then check that any intended arc or bounce still reads correctly.

Do not force every pose to occupy the exact same vertical position. A jump should still look like a jump.

## 4. Inspect every frame before export

Check unusually tall weapons, hair, particle effects and wide poses for clipping. Open the exported frames and inspect at least one complete playback loop before importing into your game.

## 5. Save your working project

Keep editable offsets so you can revisit alignment decisions without repeating the process.

## Optional visual workflow: Anchor Loom

[Anchor Loom on itch.io](https://nasi-tools.itch.io/anchor-loom) is a local, desktop-browser tool for comparing and aligning PNG frame sequences and uniformly divided sprite sheets. It lets you:

- Compare original and adjusted animation.
- Choose visible bottom-center, visible center, or unchanged placement.
- Apply per-frame X/Y offsets.
- Export PNG frames, a grid sprite sheet, and generic JSON coordinates.
- Save an editable project.

It is a paid product; the [public repository](../README.md) is a showcase and issue tracker, **not** the application download. The current README lists input limits and tested browsers.

### When it is not a fit

The current tool supports still PNG input, not GIF/video import. Alpha-bound alignment can flatten intentional motion. Its metadata is generic rather than engine-specific. See the README for detailed size limits and other constraints.

---

*This is an educational checklist, not a claim about sales results, performance gains, or compatibility with every game engine.*
