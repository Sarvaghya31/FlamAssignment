

File: interactive-bezier-rope.html
Platform: Web (plain HTML + JavaScript + Canvas)
Purpose: Interactive cubic Bézier curve that behaves like a springy rope.

What this contains:

1. Full implementation of cubic Bézier math (B(t)) and derivative B'(t).
2. Spring-damper physics for control points P1 and P2:
   acceleration = -k * (position - target) - damping * velocity
   Uses an explicit Euler integrator with variable dt.
3. Interaction:
   - Mouse move sets the "target" for dynamic control points.
   - Click + drag a control point to move it directly.
   - Toggle options and sliders to change stiffness (k), damping, tangent length, and samples.
4. Rendering:
   - Samples the curve with small t increments (default 0.01) and draws the path by connecting sampled points.
   - Draws normalized tangent vectors at regular intervals along the curve.
   - Shows control points as draggable circles and endpoint anchors.
5. Performance:
   - Uses requestAnimationFrame and tracks dt to keep motion consistent.
   - Keeps drawing cheap primitives and uses sampling instead of heavy algorithms to keep frame rate high.

Math & Physics notes:

Cubic Bézier (four points P0..P3):
B(t) = (1-t)^3 * P0 + 3(1-t)^2 t * P1 + 3(1-t) t^2 * P2 + t^3 * P3

Derivative (tangent):
B'(t) = 3(1-t)^2 (P1 - P0) + 6(1-t)t (P2 - P1) + 3t^2 (P3 - P2)
Normalize B'(t) for unit direction when drawing tangent blades.

Spring-damper model used per control point:
acc = -k * (pos - target) - damping * vel
vel += acc * dt
pos += vel * dt

Design choices:

- Both P1 and P2 are dynamic and use the same physics constants by default.
- The mouse position acts as the "global" target; each control point's target is a blend of its anchor position and the mouse to give more natural rope behavior.
- Dragging control points overrides the physics input until released.

