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
- **A spike marks each body's spin axis.** Both halves are the same size. The
  bright end points along the body's angular momentum, by the right-hand rule,
  and the dark end marks the other side. The two are not parallel and the two bodies turn opposite ways, which is
  what makes their frame-dragging twists fight each other in the middle. The
  direction matters: a bare unmarked line would not tell you which way a body
  spins, and two bodies sharing an axis but spinning opposite ways drag space in
  opposite senses.
- **The white marker at the center is the barycenter.** The faint line joining
  the two bodies runs through it. The heavier body sits on the shorter arm.
- **The three clocks are pinned to fixed coordinates.** They drift on screen
  because the coordinates themselves are being dragged. Their hands run slow in
  proportion to how deep in the potential they sit.

- **The stars are for scale, not for parallax.** Orbiting the camera around a
  fixed center rotates the whole world rigidly, so distant stars swing around
  with everything else. They are there so the lattice reads as sitting inside a
  larger space rather than floating in a void.

## Controls

Drag to turn the view: press and hold, then move, and the camera swings around
the pair. Scroll or pinch to zoom. Double-click or double-tap to reset.

On a Mac trackpad with tap-to-click enabled, a light tap-and-slide will not
register as a drag. Press the trackpad down and keep it held while moving.

## What this is not

A simulation. The falloffs and frequencies are tuned to be legible, not correct.
Real frame dragging dies off far faster than what you see here, and the ripple
speed is not locked to the orbital frequency. It is a picture of the idea.

## Running it

One self-contained HTML file. No build, no dependencies, no server. Open
`index.html` in any browser with WebGL.
