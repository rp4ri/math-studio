---
name: math-modeling
description: Write mathematical modeling papers and thesis chapters — dynamical systems, ODEs/PDEs, bifurcation analysis, stochastic models — with step-by-step derivations, theorem-proof structure, and reproducible simulations. Use when the document centers on a mathematical model (deriving it, analyzing it, validating it against data).
allowed-tools: Read Write Edit Bash
license: MIT license
metadata:
    skill-author: rp4ri (fork of K-Dense claude-scientific-writer)
---

# Mathematical Modeling Papers

## Overview

This skill produces mathematical modeling documents where the *model itself* is the contribution:
deriving it from mechanisms, analyzing it (equilibria, stability, bifurcations), and validating it
against data. It complements `scientific-writing` (general IMRaD) with conventions specific to
applied mathematics.

## Core conventions

### 1. Every model must be DERIVED, not declared
- State the modeling heuristic explicitly: parsimony (simplest model reproducing the phenomenon),
  mechanism (each term maps to a documented real-world mechanism), emergence (qualitative behavior
  must *emerge* from ingredients, never be imposed).
- Show derivations **step by step**: separation of variables, partial fractions, linearization,
  Taylor expansions. Readers (and committees) value visible procedure over cited results.
- Box the key results: `\boxed{...}` for the central equation/solution.

### 2. Standard analysis pipeline for dynamical systems
1. **Fixed points**: solve f(x*) = 0; report all of them.
2. **Stability**: 1D — sign of f'(x*); nD — eigenvalues of the Jacobian. Derive the linearization
   (perturbation η, dη/dt = f'(x*)η) at least once per document.
3. **Bifurcations**: locate via tangency conditions (g = 0 and g' = 0 simultaneously); name the type
   (saddle-node/fold, transcritical, pitchfork, Hopf); include a bifurcation diagram.
4. **Basins of attraction / separatrices** when multistable; phase portraits with nullclines.
5. **Validation**: retrodiction or calibration against data; ALWAYS state N and whether parameters
   were fitted or structurally chosen.

### 3. Honesty section is mandatory
Every modeling document includes a "Limitations" section distinguishing:
- structural/illustrative models (parameters encode domain knowledge) vs calibrated models (fitted);
- consistency checks vs causal/predictive claims;
- aggregation assumptions, constant-parameter assumptions, deterministic vs stochastic gaps.

### 4. Tooling (user stack)
- **Python via `uv`** (never bare pip): numpy, scipy, matplotlib. Run with `uv run python script.py`.
- Simulations live in versioned scripts (one per figure), outputs to `results/`. Every figure in the
  paper must be regenerable by one script; reference the script name in a comment or appendix.
- Numerical recipes: `scipy.optimize.fsolve` for fixed points (multi-start grid), numerical Jacobian
  with central differences, RK4 (vectorized over initial-condition grids) for basins, parameter
  sweeps for bifurcation diagrams.
- For PDFs: LaTeX (pdflatex/latexmk) when available; otherwise HTML+MathJax printed via headless
  Chrome (`--print-to-pdf --virtual-time-budget=30000`) is an accepted fallback.
- GIFs of bifurcations (matplotlib FuncAnimation + PillowWriter) for talks; static montage of 3
  frames for the PDF version.

### 5. Figures that mathematical modeling papers need
- Phase portrait with nullclines, fixed points (filled = stable, X = saddle), sample trajectories,
  and basins shaded.
- Bifurcation diagram (equilibria vs parameter, stable solid / unstable dashed or crossed).
- Graphical fixed-point analysis (recruitment curve vs decay line) when explaining threshold-driven
  bistability — it is the most pedagogically effective single figure.
- Heuristic/ingredients figure when the model choice needs justification.
- ALWAYS pin imshow color scales (vmin/vmax) when rendering basin grids — autoscaling silently
  flips colors when a basin occupies 100% of the plane.

### 6. Literature anchoring
Anchor each model ingredient in its canonical lineage (cite the foundational paper, not a survey):
threshold collective behavior (Granovetter 1978; Schelling 1978; Kuran 1991), coupled excitable
populations (Wilson & Cowan 1972), bistability/hysteresis in applied systems (Ludwig-Jones-Holling
1978; Scheffer 2009), catastrophe theory (Thom/Zeeman), escape over barriers (Kramers 1940),
empirical reconstruction (Friedrich & Peinke; Kramers-Moyal estimators). State explicitly which
assembly is novel vs which pieces are standard.

## When to use this skill vs scientific-writing
- Model derivation/analysis is the paper's core → **math-modeling** (this skill).
- Reporting experimental/clinical/survey results in IMRaD → `scientific-writing`.
- Both apply for data-validated models: use this skill's analysis pipeline inside the IMRaD frame.
