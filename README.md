# Cylinder Shot Peening Coverage Simulator

Interactive educational simulator of shot peening coverage on a rotating cylinder,
by **Dr Régis Kubler**, Arts et Métiers Institute of Technology,
developed with the assistance of Claude Opus 5.5 (Anthropic), November 2026.

**User guide (PDF):** [cylinder_simulator_guide.pdf](docs/cylinder_simulator_guide.pdf), online at https://rkubler.github.io/cylinder-coverage/docs/cylinder_simulator_guide.pdf. LaTeX sources in [`docs/`](docs/).

Live page: https://rkubler.github.io/cylinder-coverage/

## What it simulates

A cylinder of radius R (5 to 500 mm) turns at N rpm (1 to 60 rpm). The nozzle is aimed
at the axis, perpendicular to the surface, and travels along the axis at the traverse
speed V. Relative to the surface the jet draws a helix:

- surface speed Vs = 2πR·N/60
- helix pitch p = 60·V/N (axial advance per turn), helix angle α = atan(V/Vs)

When the pitch is large compared with the jet size ΔX, the impacts pile up in helical
stripes. The page shows the rotating cylinder, the unrolled surface map
(axial position × circumference), a cross-section along the axis and the readouts.

## Model

- Mean impacts per point after one pass: K_c = c·a·Ṅ / (2πR·V), independent of the
  rotation speed (c = fraction of the shots that hit the cylinder).
- Average model: τ = 100·(1 − e^(−K_c·n)).
- Helical stripe model: along the helical coordinate ζ = z ∓ s·p/(2πR), λ is periodic
  with period p, λ(ζ) = a·Ṅ·Σ_k h(ζ − k·p), with the track profile
  h(ζ) = (1/Vs)·∫ g(ζ − σ·tan α)·g_s(σ) dσ. The mean coverage is the mean of 1 − e^(−λ).
- Back-and-forth passes draw helices of opposite hands; the mean coverage factorises:
  τ̄ = 1 − mean(e^(−λ₊))·mean(e^(−λ₋)).
- Curvature: a parallel jet aimed at the axis hits the arc s = R·asin(u/R); density per
  unit arc g_s(s) = g(R·sin(s/R))·cos(s/R); shots with |u| > R miss the cylinder.
- Gaussian jet: stripes below 3 % for p ≤ ΔX/2, i.e. N ≥ 120·V/ΔX rpm.

Jet profiles: uniform, Gaussian (σⱼ = ΔX/4) or flattened Gaussian (exponent p), see
https://rkubler.github.io/extended-simulator/jet_profile.html


## Coverage above 100 % (NF L 06-832)

The base time is the peening time needed to dent 98 % of the surface. Full coverage (100 %) is 1.25 × the base time (an option sets it to the base time instead), and 200 % or 300 % means 2 or 3 times the time of full coverage. On an even random treatment, full coverage corresponds to λ = 1.25·ln 50 = 4.89 impacts per point on average (99.25 % dented). Coverage itself is always the dented area divided by the total area. The readout "Coverage (NF L 06-832)" combines both: it is the dented area up to the base time t_b (98 %), goes from 98 % to 100 % between t_b and 1.25·t_b, and is 100·t / (1.25·t_b) above (200 % = twice the time of full coverage).

## Validation

The simulated mean coverage matches an independent brute-force calculation (point-sampled
footprint, small time steps) to within 0.1 % on seven cases covering radii from 5 to
200 mm, uniform, Gaussian and flattened jets, back-and-forth and one-way passes, jets wider
than the cylinder and surfaces with or without the curvature correction.

## Files

- `index.html`: the simulator (single self-contained page, no installation)
- `LICENSE`: MIT licence for the code; documentation under CC BY 4.0

## Related tools

- Shot Peening Coverage Simulator (local coverage): https://rkubler.github.io/peening-coverage/
- Extended Shot Peening Coverage Simulator (flat part, raster paths): https://rkubler.github.io/extended-simulator/
- Home page: https://rkubler.github.io/shotpeening/

## Versions

- 2.1 (22 November 2026): export of every figure as a PNG image (as shown at that instant) and of the data of every chart, map and table as a CSV file.
- 2.0 (22 November 2026): shorter label for the NF L 06-832 map (no horizontal scroll on phones); θ, ψ and two nozzles checked against an independent 3D ray brute force.
- 1.9 (22 November 2026): angle ψ tilting the nozzle plane around the axis direction: the jet no longer aims at the axis, u(s) = R[sin(s/R + ψ) − sin ψ]; cross-section view of the cylinder with the nozzle, the jet and the treated arc.
- 1.8 (22 November 2026): option for 2 nozzles in the same plane, aimed at the same point, at angles θ₁ ≥ 0 and θ₂ ≤ 0 (flow rate per nozzle); K = K₀·[f(θ₁) + f(θ₂)]. Both nozzles are drawn in the side view.
- 1.7 (22 November 2026): the nozzle is drawn tilted by θ in the side view, with the normal and the angle.
- 1.6 (22 November 2026): nozzle angle θ from the normal (0 to 60°, tilted in the plane of the path and the normal) and dent-area law a = a₀·cos θ or a₀·cos²θ; the footprint is ΔX/cos θ long along the path, which smooths the helical stripes.
- 1.5 (18 November 2026): readout "Min / max coverage (NF L 06-832)", which can exceed 100 %.
- 1.4 (18 November 2026): local coverage (NF L 06-832) of each point, above 100 %, in the hover readout and as a map mode.
- 1.3 (18 November 2026): new readout "Coverage (NF L 06-832)", which can exceed 100 %.
- 1.2 (18 November 2026): coverage-level readouts and map removed; coverage is the dented area divided by the total area.
- 1.1 (18 November 2026): coverage above 100 % (NF L 06-832): specified coverage, base time, 1.25 factor option.
- 1.0 (18 November 2026): first release.

Educational model. It does not replace coverage verification according to the applicable
standards (e.g. SAE J2277).
