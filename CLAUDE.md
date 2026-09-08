# binary-spacetime

A single-file WebGL visualization of a tilted, unequal-mass binary that inspirals
and warps a 3D coordinate lattice. Public repo, served from GitHub Pages, shared
with friends. Hobby project.

Live: https://ozan2025.github.io/binary-spacetime/

`README.md` explains what the visualization shows and how to read it. This file
covers what will bite you while changing it.

## Shape

One file, `index.html`, about 880 lines. Raw WebGL 1, hand-written matrix math,
no libraries, no build, no server. Open it in a browser and it runs.

Two shader programs. `lineProg` draws the lattice, the wave rings, the orbit
paths, the mechanics overlay and the starfield. `solidProg` draws the two bodies
and the flat-shaded cone markers.

## Invariants that break quietly

**`deformCPU()` in JS is a hand-port of `deform()` in GLSL.** The GPU warps the
lattice. The CPU copy exists only to place the three floating clock divs onto the
lattice they are pinned to. They must stay in step. Change one without the other
and the clocks drift off the lattice with nothing erroring. Both carry a comment
marking the pairing.

**Orbit phase is accumulated, never computed.** Never write `angle = rate *
uTime`. The inspiral changes the rate every frame, so that formula gives the
wrong angle and the bodies jump. `updateOrbit()` integrates `orbitPhase` and
`wavePhase` in JS and passes them in as uniforms, along with the two orbit radii
and the wave amplitude. JS is the single source of truth for all orbit state.

**Both phase accumulators wrap at 2*pi.** They only feed sin and cos. Unwrapped,
they drift into float32 precision loss on a page left open for hours.

## WebGL constraints that shaped the design

**Line width is clamped to 1 pixel.** Chrome ignores `gl.lineWidth`. A one-pixel
line is invisible inside a lattice built from thousands of identical one-pixel
lines. That is why the spin axes are cones rather than thick lines, and why the
first attempt at drawing them as plain lines failed completely.

**Lines blend additively with depth writes off.** `gl.blendFunc(SRC_ALPHA, ONE)`
plus `depthMask(false)`. Overlapping strands then glow instead of fighting over
draw order, which is order-independent and free. Bodies draw first with normal
alpha and depth writes on, so they still occlude correctly.

**Marker shading matters.** The cone markers use a `uFlat` path in `SOLID_FS`
with no rotating surface bands. When they were spheres drawn with the full body
shader, they read as five extra small masses and the scene looked like a
six-body system.

## Tuning is coupled

The line intensity curve, the per-buffer alphas, the wave glow gain and the wave
amplitude all feed one output number. Change one and the rest need retuning. Two
real failures from this:

- Switching to additive blending without lowering the alphas made everything
  about ten times too dim and the orbit rings invisible.
- Growing the wave amplitude for the inspiral pushed the glow past its ceiling
  across the whole far field and washed the lattice to solid white.

Always look at a screenshot after touching any of these. Reasoning about them
does not work.

## Deliberate non-goals

**This is not a simulation.** Falloffs and frequencies are tuned for legibility.
Real frame dragging dies off far faster than what is drawn. The ripple speed is
not locked to twice the orbital frequency. The page badge says "conceptual" and
the README says so plainly. Do not push the constants toward accuracy without
asking. It makes the piece duller and Ozan decided against it.

**Mass is defined in four places and they disagree.** Orbit radii imply 1.83:1,
lattice pull 1.59:1, clock potential 1.64:1, sphere size 1.26:1. This was raised
and Ozan chose to leave it. The numbers were tuned by eye.

**The pair does not merge.** It inspirals, then eases back out over five seconds
and loops. A merge with ringdown was offered and Ozan chose the loop.

**Color collapses to neutral at closest approach, on purpose.** Every point is
then roughly equidistant from both bodies, so neither frame-dragging twist owns
any region. Correct, not a bug.

**The spin-axis halves are equal in size, direction marked only by brightness.**
A size difference reads as a defect. This was iterated on three times. Leave it.

## Verifying a change

Use the verify-Chrome lane, never the browser extension:
`~/.claude/skills/review-loop/scripts/verify-cdp.mjs`

Two things that cost time here:

- `run` accepts steps only via stdin: `node verify-cdp.mjs run - < steps.json`.
  Passing a file path fails with a JSON parse error, despite the usage line
  saying it takes one.
- Synthetic pointer events need a frame boundary before you read the DOM.
  Dispatching a drag and reading clock positions in the same synchronous block
  always reports no movement, because no animation frame has run yet. Put a
  `wait` step between them. This produced a false regression report once.

Minimum check for any visual change: shaders compile (the `#error` div stays
hidden), 60fps on desktop and on an emulated 390x844 dpr-3 viewport, drag and
zoom and double-click-reset all still work with no JS errors, no horizontal
overflow on mobile, and a screenshot you actually open and look at.

## Deploying

Commit to `main` and GitHub Pages rebuilds on its own. Two gotchas:

- Pages caches HTML for about ten minutes. Verify the live URL with a
  cache-busting query string or you will confirm a stale copy and believe it.
- Wait for the rebuild before telling Ozan it is live. Sending him to refresh
  into the old version wasted a round trip.

## Working agreement

**Commit straight to `main`. No issue, no branch, no PR, no merge ask.** Ozan
confirmed this for this repo on 2026-09-08. It overrides the branch-and-ask
chain in `~/.claude/CLAUDE.md` here, and only here.

It works because this is a solo hobby repo with no collaborators, nothing in
production depends on it, and Ozan reviews the result visually in the browser
rather than reviewing diffs. The review that matters is him looking at the live
page, so a PR would add ceremony with no reviewer behind it.

One commit per coherent change, with a real commit message. The message is the
only record of why a constant moved, so write it properly.

Everything else in the global rules still holds. Force-push, discarding
uncommitted work, `reset --hard`, `git clean`, deleting an unmerged branch, and
anything destructive still need an explicit yes.
