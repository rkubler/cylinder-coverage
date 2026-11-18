# Cylinder Shot Peening Coverage Simulator

Interactive educational simulator of shot peening coverage on a rotating cylinder,
by **Dr Régis Kubler**, Arts et Métiers Institute of Technology,
developed with the assistance of Claude Opus 5.5 (Anthropic), November 2026.

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
- Home page: https://rkubler.github.io/

## Versions

- 1.0 (18 November 2026): first release.

Educational model. It does not replace coverage verification according to the applicable
standards (e.g. SAE J2277).
