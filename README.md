# Binary Spacetime

An interactive WebGL visualization of two unequal masses in a tilted orbit, each
spinning around its own misaligned axis, and what that does to the space around them.

**Live: https://ozan2025.github.io/binary-spacetime/**

## How to read it

- **Brightness is warping.** A lattice line glows in proportion to how far it has
  been dragged from where it would sit in flat space.
- **Color is ownership.** The lattice turns orange where the heavy body's
  frame-dragging twist dominates and blue where the lighter body's does. The
  boundary between them moves as they orbit.
- **The two faint rings are the orbital paths.** The heavier orange mass keeps
  the tighter one.
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
