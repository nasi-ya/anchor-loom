# Fix jitter in PNG sprite animations: an alignment checklist

Small changes in a sprite's position between PNG frames can make an otherwise smooth animation look shaky. Before redrawing the artwork, check whether the visible subject is anchored consistently.

This guide explains one way to diagnose that problem and preserve intentional movement. It is useful for sprite artists, pixel-art animators, and indie game developers working with transparent PNG frames or a uniformly divided sprite sheet.

## 1. Check the source

Keep an untouched copy of your frames. Confirm the order and preview the animation at its intended speed. Separate three problems:

- **Unwanted position jitter:** the whole sprite shifts around.
- **Intentional movement:** jumps, breathing, recoil, or bobbing that should remain.
- **Different artwork:** moving limbs or changing silhouettes that a simple offset cannot repair.

Alignment only addresses the first of these.

## Quick test: is the jitter in the PNG frames or the game engine?

Before changing the art, preview the original PNG sequence at a fixed canvas position, with the same frame order and speed as in the game.

- **It shakes in the standalone preview too:** inspect transparent padding, visible bottom-center or center alignment, and frame-by-frame offsets. Check that the original files all use the intended canvas size.
- **It looks steady in the preview but shakes in-game:** check the engine's sprite origin/pivot, trimming or packing settings, scaling, and camera movement before editing the PNGs. Changing source-frame alignment may not fix an engine-side issue.
- **Only a jump, recoil, or bounce looks different:** preserve that intentional movement. Use manual offsets rather than forcing every silhouette to the same anchor.

For a sprite sheet, also verify that each cell is divided at the intended width and height. A wrong grid can look like alignment jitter even when the original frames are consistent.

## Aseprite → Godot: reduce the canvas without shifting poses

If an Aseprite animation has large transparent margins and looks oversized or offset in Godot's AnimatedSprite2D, avoid trimming every exported frame independently.

1. Work on a copy of the Aseprite project and keep all animation tags together.
2. Set one shared canvas size with **Sprite → Canvas Size**, or make a selection that covers the intended shared rectangle and use **Sprite → Crop**. See the [official Aseprite canvas documentation](https://aseprite.org/docs/canvas).
3. Choose bounds large enough for the widest pose across every tag, including attacks, weapons, jumps, and effects. Do not crop tightly around the walking pose if another animation needs more room.
4. Scrub through every tag after cropping. Check that frames still have identical dimensions and that the character's feet or chosen pivot remain at the same pixel coordinates. Keep enough transparent space for the largest pose.
5. Export again and compare the PNG sequence at a fixed canvas position before importing it into Godot.
6. If the standalone sequence is stable but Godot still shifts, inspect the AnimatedSprite2D origin/pivot, frame dimensions, trimming/packing, scaling, and horizontal flipping. A shared canvas crop alone does not diagnose every engine-side offset.

Aseprite notes that changing canvas size also changes slice positions, so recheck any slices after resizing.

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
- Export PNG frames, a grid sprite sheet, generic JSON coordinates, and optional engine-oriented files for Godot 4, Phaser 3, PixiJS, and Roblox.
- Save an editable project.

It is a paid product; the [public repository](../README.md) is a showcase and issue tracker, **not** the application download. The current README lists input limits and tested browsers.

### When it is not a fit

The current tool supports still PNG input, not GIF/video import. Alpha-bound alignment can flatten intentional motion. Engine-oriented export files do not guarantee a one-click import; Unity and Roblox Studio setup has been documented but not tested inside those editors. See the README for detailed limits and tested browsers.

---

*This is an educational checklist, not a claim about sales results, performance gains, or compatibility with every game engine.*
