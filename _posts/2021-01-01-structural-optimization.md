---
title: "Multi-Stage Structural Optimization for Complex Composite Structures"
excerpt: "Developed a multi-stage optimization workflow for composite structures on military platforms, resulting in weight margins of 10 - 20%."
tags: [numerical-optimization, finite-element-analysis]
---

Association(s): RTX

# Overview

Structural optimization is widely used across the aerospace industry for metallic components. However, when applied to composite assemblies, traditional gradient-based solvers (e.g., NASTRAN SOL200) struggle to account for the material's non-homogeneous properties. As a result, engineers often either treat the assembly as a homogeneous material using averaged properties or manually zone the structure into multiple optimization regions to force solver convergence. Both approaches introduce simplifications that frequently lead to suboptimal designs.

To solve this problem, I developed a multi-stage optimization workflow using the latest advancements in structural optimization available through Altair Hypermesh. My solution involved:
* **Stage 1 - Free-Size Optimization:** Determine optimal load paths with a global search algorithm.
* **Stage 2 - Size Optimization:** Develop detailed ply-maps for each thickness zone identified in Stage 1.
* **Stage 3 - Design of Experiments:** Run a parameterized simulation, then fit a regression model to approximate structural behavior for rapid trade-off analyses.

This methodology not only solved the challenges associated with composite assembly optimization, but also enabled faster design iterations. As a result, it was particularly well suited for the military programs supported by my engineering organization, which consisted of a high volume of relatively small assemblies, each with distinct requirements.

<br/>

# System Architecture

## Stage 1 & 2

![Optimization Workflow]({{ '/assets/images/StructuralOptArchitecture.jpg' | relative_url }})

In Stage 1, optimization is performed assuming a homogeneous material to establish primary load paths. Once these load paths are defined, Stage 2 determines detailed ply mappings. Separating the process into two phases speeds up convergence and improves solution quality for non-homogeneous assemblies.

## Stage 3

![Design of Experiments Workflow]({{ '/assets/images/DesignOfExperimentsArchitecture.jpg' | relative_url }})

Lastly, a constrained Design of Experiments (DOE) is performed by perturbing ply thicknesses around the Stage 2 optimum. A regression model is then fit to the resulting data to support downstream trade-off analyses driven by manufacturability and other non-structural constraints. Conducting this step last minimizes the search space, as it centers the DOE around the Stage 2 optimum rather than exploring the full design grid.

<br/>

# Future Opportunities

I was nominated for RTX's Engineer of the Year award for successfully developing and applying this methodology to multiple programs - I ended up winning Semi-Finalist, which was one of my greatest achievements as a Structural Engineer.

That being said - it's always good to have a continuous improvement mindset. Some proposals I had for expansion of this method was:
* **Bolted Structures:** Expand methodology to support optimization of fastener placements.
* **Rib Optimization:** Expand methodology to support optimization of bulkheads and vertical ribs. Currently, it's only capable of optimizing built-in composite ribs (lateral ribbing), but not a bulkhead bolted on.
