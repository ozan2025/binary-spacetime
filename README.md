# Binary Spacetime

An interactive WebGL visualization of two unequal masses in a tilted orbit, each
spinning around its own misaligned axis, and what that does to the space around them.

**Live: https://ozan2025.github.io/binary-spacetime/**

## How to read it

- **Brightness is warping.** A lattice line glows in proportion to how far it has
  been dragged from where it would sit in flat space.
- **Color is ownership.** The lattice turns orange where the heavy body's
  frame-dragging twist dominates and cyan where the lighter body's does. It
  stays grey where neither reaches. The boundary between them moves as they
  orbit.
- **The bands sweeping outward past the pair are the gravitational waves.**
  Their real displacement is far too small to see, so the ripple amplitude is
  given its own amplified glow.
- **The two faint rings are the orbital paths.** The heavier orange mass keeps
  the tighter one.
- **Each body carries a pole along its own spin axis**, with a bright marker at
  one end and a dark one at the other. The two poles are not parallel, which is
  what makes the frame-dragging twists fight each other in the middle.
- **The white marker at the center is the barycenter.** The faint line joining
  the two bodies runs through it. The heavier body sits on the shorter arm.
- **The three clocks are pinned to fixed coordinates.** They drift on screen
  because the coordinates themselves are being dragged. Their hands run slow in
  proportion to how deep in the potential they sit.

## Controls

Drag to orbit. Pinch or scroll to zoom. Double-tap or double-click to reset.

## What this is not

A simulation. The falloffs and frequencies are tuned to be legible, not correct.
Real frame dragging dies off far faster than what you see here, and the ripple
speed is not locked to the orbital frequency. It is a picture of the idea.

## Running it

One self-contained HTML file. No build, no dependencies, no server. Open
`index.html` in any browser with WebGL.
